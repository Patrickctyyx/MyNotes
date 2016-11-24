
```
class Patrick:
    
    @classmethod
    def cty(cls):
        print('This is a class method')
        
    @staticmethod
    def cheng():
        print('This is a static method')
        
    @property
    def tian(self, name='cty')
        print('name {} cannot bechanged'.format(name))
        
pat = Patrick()
pat.cty()
pat.cheng()
```
二者分别用上面的装饰器来修饰

类方法以类名为参数，使用时与实例无关

静态方法使用时与实例无关

另外@property修饰的函数是一个只读函数