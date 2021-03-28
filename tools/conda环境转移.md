# conda环境转移

## 单机环境复制

```bash
conda create -n BBB --clone AAA
```



## 双机环境转移

导出所需包

```bash
conda env export > environment.yaml
```

```bash
pip freeze > requirements.txt
```

发送到主机

```bash
scp -P 2253 <filepath> vsislab@202.194.203.129:<des_filepath>
```

从主机下载

```bash
scp -P 22 cook@101.37.78.172:<filepath> <des_filepath>
```

环境重建

```bash
conda env create -f environment.yaml
```

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
```

在家目录新建.condarc文件

```bash
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
show_channel_urls: true
ssl_verify: true
```

