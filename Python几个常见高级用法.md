## filter(function, iterable)
- function：过滤的条件
- iterable：要被过滤的对象

```
>>>a=[1,2,3,4,5,6,7]
>>>filter(lambda x:x>5, a)
>>>[6,7]
```

## map(function, iterable, ...)
把函数作用在对象的每个元素中
- function：作用函数
- iterable：作用对象
- 
```
>>>a=[1,2,3]
>>>b=[4,5,6]
>>>map(lambda x,y:x+y, a,b)
>>>[5,7,9]
```

## __import__(name, globals=None, locals=None, fromlist=(), level=0)
用来import模块，另外返回值是模块对象
- import spam相当于
```
spam = __import__('spam', globals(), locals(), [], 0)
```
- import spam.ham相当于
```
spam = __import__('spam.ham', globals(), locals(), [], 0)
```
- from spam.ham import eggs, sausage as saus相当于
```
food = __import__('spam.ham', globals(), locals(), [eggs, sausage], 0)
eggs = food.eggs
sauage = food.sausage
```
- level是用于决定是否执行绝对导入。-1是绝对相对都会尝试，默认。0是绝对。正数表示相对当前模块的父目录的层数。

## format()
这是字符对象的函数，用来格式化字符串
- 基本用法
```
'{0},{1}'.format('kzc',18)  
>>>'kzc,18'  
'{},{}'.format('kzc',18)  
>>>'kzc,18'  
'{1},{0},{1}'.format('kzc',18)  
>>>'18,kzc,18'
```
- 占位符
：前面是表示对应后面的第几个数
：后面一位表示用这个填充
：后面的>表示向右对齐

```
'{:>8}'.format('189')
>>>'     189'
'{:0>8}'.format('189')
>>>'00000189'
'{:a>8}'.format('189')
>>>'aaaaa189'
```
