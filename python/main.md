# 常用的脚本文件main函数

```python
import os
import argparse

def main(opt):
    print('this message is from main function')

def get_args():
    parser = argparse.ArgumentParser('Tesing')
    parser.add_argument('-p', '--project', type=str, default='coco')
    parser.add_argument('-c', '--compound_coef', type=int, default=0)

    args = parser.parse_args()
    return args
    
if __name__ == '__main__':
    opt = get_args()
    main(opt)
```

