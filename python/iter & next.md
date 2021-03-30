# iter与next的用法

对于一个可迭代对象（列表，元组，`torch.utrils.data.DataLoader`等），使用`iter()`函数可以获取它们的迭代器，使用`next()`函数可以获取迭代器的下一个对象。

> 当迭代完所有的元素之后再次调用`next()`，会抛出异常。

```python
train_dataloader = DataLoader(training_data, batch_size=64, shuffle = True)
test_dataloader = DataLoader(testing_data, batch_size=16, shuffle = True)

train_features, train_labels = next(iter(train_dataloader))
```

