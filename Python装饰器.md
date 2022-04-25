## 基本实现

```python
@函数名
def Function():
	pass

当我们执行Function()时的流程如下: 
	Function = 函数名(Function)
	Function()

例子:
def outer(origin):
    def inner():
        print('before')
        result = origin()
        print('after')
        return result
    return inner

@outer
def Function():
    print([1,2,3,4,5,6])
result = Function()
输出:
	before
	[1,2,3,4,5,6]
	after
等价于:
	Function = outer(Function)
	Function()#此时其实是在调用inner函数,因为我们的Function函数的地址已经被换为inner的地址了
```

## 问题1

- 我们的Function此时无法传参,这是不恰当的,因此我们需要做如下改进,从而让`inner`函数可以接收多个参数

  ```python
  def outer(origin):
      def inner(*args,**kwargs):
          print('before')
          result = origin(*args,**kwargs)
          print('after')
          return result
      return inner
  
  @outer
  def Function():
      print([1,2,3,4,5,6])
  result = Function()
  ```

## 问题2

- 虽然此时问题已经解决,但是我们会发现使用`Function.__name__`的输出为`inner`,`Function.__doc__`的输出会是`inner`的注释,因此我们通过引入`functools`库来解决这个问题

  ```python
  import functools
  def outer(origin):
      @functools.wraps(origin)
      def inner(*args,**kwargs):
          print('before')
          result = origin(*args,**kwargs)
          print('after')
          return result
      return inner
  
  @outer
  def Function():
      print([1,2,3,4,5,6])
  result = Function()
  ```

  