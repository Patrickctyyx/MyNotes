就像模块的名字一样，这个模块就是为了实现一些实用功能

# 第一部分


> - find_modules
> - get_entries
> - join_url

这开头的三个模块，是为了注册蓝图


```Python
def find_modules(init_file, fpattern=None):
    """
    List names of modules in the same directory as init_file. The function is
    usually used in __init__.py and returns value fit for __all__.
    If you need to import it, use __import__ with level 1.
    """

    import pkgutil

    fpattern = re.compile(r"^[a-zA-Z][a-zA-Z0-9_]*$") \
        if fpattern is None else re.compile(fpattern)

    dirname = os.path.dirname(init_file)
    entries = [modname for _, modname, _ in pkgutil.iter_modules([dirname])]  # 之所以写三个是因为pk那个是一个生成器，有三个参数
    entries = list(filter(lambda n: fpattern.match(n), entries))  # 过滤出非__init__.py的内容

    return entries


def get_entries(init_file, glb):  // 这个一般在API下一级目录的__init__.py使用
    _entries = []

    for modname in find_modules(init_file):
        mod = __import__(modname, globals=glb, level=1)  # mod是一个模块对象，level指定了从上一级开始导入
        if hasattr(mod, "Entry"):
            _entries.append((modname, mod.Entry))

    return _entries


def join_url(*args, **kwargs):
    """
    Join a args with seps. example:

        >>> join_url("abc/", "/def/", "fed", "yui")
        '/abc/def/fed/yui/'
        >>> join_url("abc", "def#", "##fed", "yui", sep=('^', '#', '$'))
        '^abc#def##fed#yui$'

    """
    sep = kwargs["sep"] if "sep" in kwargs else ('/', '/', '/')  # 主要是根据这个来连接url的

    concat_str = sep[0]
    concat_str += args[0][1:] if args[0][0] == sep[0] else args[0]

    for a in args[1:]:  # url前后都要判断
        concat_str += "" if concat_str[-1] == sep[1] else sep[1]
        concat_str += a[1:] if a[0][0] == sep[1] else a

    concat_str += "" if concat_str[-1] == sep[2] else sep[2]

    return concat_str
```

准备工作做好了就要注册蓝图了

```
def get_blueprint():
    from flask_restful import Api
    from flask import Blueprint
    from common.utils import find_modules, join_url

    # 注册了蓝图并初始化应用
    bp = Blueprint('api', __name__)
    api = Api(bp)

    for modname in find_modules(__file__):
        mod = __import__(modname, globals=globals(), locals=locals(), level=1)
        if hasattr(mod, "get_entries"):
            for entname, entry in mod.get_entries():  # 主要是这里面一个个的add很麻烦
                # 前面的所有准备工作都是为了这个    
                api.add_resource(entry, join_url(modname, entname),
                                 endpoint="{}.{}".format(modname, entname))

    return bp
```

# 第二部分
> Session Interface

不是很懂

# 第三部分
> API Test

```Python
import json
import unittest


def test_context(func):
    from flask import current_app

    def wrapper(self, *args, **kwargs):
        # 创建一个测试的环境
        with current_app.test_request_context():  
            with current_app.test_client() as client:
                self.client = client  # 这个client真心强大，后面好多都用到了
                self.login_record = None

                func(self, *args, **kwargs)

                del self.client
                del self.login_record

    return wrapper

whole_record = {}


class ApiTest(unittest.TestCase):

    def jsonify(self, obj):  # json化返回值
        import json
        return json.dumps(
                obj,
                ensure_ascii=False,
                indent=4
            )

    # 自动生成文档
    def record_requests(self, method, url, view, indata, response):
        global _RECORD_STR
        global whole_record

        outdata = self.load_data(response.data)
        user = "No user"
        if hasattr(self, 'login_record') and self.login_record:
            user = "User `%s`" % self.login_record

        if url not in whole_record:
            doc = ""
            if view and getattr(view, 'view_class', None):
                doc = view.view_class.__doc__ or ""
            whole_record[url] = [doc]

        whole_record[url] += [{
                'method': method,
                'url': url,
                'user': user,
                'indata': indata,
                'outdata': self.jsonify(outdata),
            }]

    def load_data(self, data):
        if isinstance(data, bytes):
            data = data.decode('utf8')
        return json.loads(data)

    def login_user(self, account):
        self.login_record = account.uid

        from flask import url_for
        response = self.client.post(
            path=url_for("api.auth.login"),
            data={'uid': account.uid, 'passwd': account.p}
        )
        self.assertEqual(response.status_code, 200)

    def open(self, method, endpoint, **kwargs):
        from flask import url_for, current_app
        kwargs['path'] = url_for(endpoint)

        resp = getattr(self.client, method)(**kwargs)
        view = None
        if endpoint in current_app.view_functions:
            view = current_app.view_functions[endpoint]
        indata = u'No argument.'
        if 'data' in kwargs:
            indata = kwargs['data']

        self.record_requests(
                method=method.upper(),
                url=kwargs['path'],
                view=view,
                indata=indata,
                response=resp
            )

        return resp

    def get(self, **kwargs):
        return self.open('get', **kwargs)

    def post(self, **kwargs):
        return self.open('post', **kwargs)

    def assertApiError(self, respdict, errcls):
        self.assertIn("status", respdict)
        self.assertIn("code", respdict["status"])
        self.assertEqual(respdict["status"]["code"], errcls.error_code)

    # 每进行一个测试都要执行下面这两个
    def setUp(self):
        from flask import g, current_app
        g.db.create_all()
        self.dbsess = g.db.create_scoped_session()

    def tearDown(self):
        from flask import g
        del self.dbsess
        g.db.drop_all()
```
看到这里真的很佩服志平...果然差距还是太大了呀ToT