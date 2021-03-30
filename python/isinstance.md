# isinstance()

`isinstance(obj, type)`用于判断某一个变量是否是某一个类别（或属于多个类别中的某一个 `isinstance(obj, (type1, type2))`）：

```
a = [1, 2, 3]

>>> print(isinstance(a, list)) # True
>>> print(isinstance(a, (list, tuple))) # True
```

