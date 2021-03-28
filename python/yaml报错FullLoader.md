`module 'yaml' has no attribute 'FullLoader'`

解决方案：在`import yaml`的python文件中，加入以下函数：

```python
def load_cfg(cfg_path):
    with open(cfg_path) as f_cfg:
        # cfg = yaml.load(f_cfg, Loader=yaml.FullLoader)
        cfg = yaml.load(f_cfg)
 
    assert cfg is not None, 'File Not Found ' + cfg_path
 
    return cfg
```

