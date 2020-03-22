# 我的一次安装库的过程
今天打开昨天运行良好的代码报错，提示如下：(此时notebook还是能打开的)

```javascript
ImportError: No module named 'seaborn'
```

可是我昨天明明装了呀？？  

之后命令行安装conda install seaborn,然而又报错：

```
module 'pandas' has no attribute '__version__'
```
然后重装pandas，因为卸载pandas过程中把seaborn也卸了，所以我一直在卸载安装的轮回中  

不知道为什么又出现了新的错误  

```
No module named 'pandas.compat'
```

然后我的notebook在VScode中就打不开了 ？？？

```
Activating Python 3.5.6 64-bit ('tensorflow': conda) to run Jupyter failed with Error: StdErr from ShellExec,
Error processing line 1 of F:\Anaconda\envs\tensorflow\lib\site-packages\matplotlib-3.0.3-py3.5-nspkg.pth:

  Traceback (most recent call last):
    File "F:\Anaconda\envs\tensorflow\lib\site.py", line 167, in addpackage
      exec(line)
    File "<string>", line 1, in <module>
    File "<frozen importlib._bootstrap>", line 574, in module_from_spec
  AttributeError: 'NoneType' object has no attribute 'loader'

Remainder of file ignored
.
```
然后卸载matplotlib，又因为网络原因无法安装一直提示Retry  

再换成国内的源

```
pip install matplotlib -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
```

终于安装成功，然并卵，还是打不开

最后我从Anaconda Navigator中，安装matplotlib成功，此时终于可以正常使用了！
## 总结
安装库(tensorflow以及相应的库)的时候有一个大坑:**版本兼容问题**  

使用命令行：conda/pip install package-name 安装的是最新版本，可能无法兼容，需要指定相应版本安装

初学者(我)最好从anaconda界面中安装，他会自动搜索适合的版本以及相应的依赖库
