# python 文件生成 exe ,pyinstaller + spec文件


## 安装 pyinstaller
```
pip install pyinstaller
```
## conda 创建一个虚拟环境
conda常用的命令

1）查看安装了哪些包
```
conda list
```
2)查看当前存在哪些虚拟环境
```
conda env list 
conda info -e
```
3)检查更新当前conda
```
conda update conda
```
3.Python创建虚拟环境
```
conda create -n your_env_name python=x.x
```
anaconda命令创建python版本为x.x，名字为your_env_name的虚拟环境。your_env_name文件可以在Anaconda安装目录envs文件下找到。

4.激活或者切换虚拟环境
```
打开命令行，输入python --version检查当前 python 版本。

Linux:  source activate your_env_nam
Windows: activate your_env_name
```
5.对虚拟环境中安装额外的包
```
conda install -n your_env_name [package]
```
6.关闭虚拟环境(即从当前环境退出返回使用PATH环境中的默认python版本)
```
deactivate env_name
或者`activate root`切回root环境
Linux下：source deactivate 
```
7.删除虚拟环境
```
conda remove -n your_env_name --all
```
8.删除环境钟的某个包
```
conda remove --name $your_env_name  $package_name 
```
8、设置国内镜像

http://Anaconda.org的服务器在国外，安装多个packages时，conda下载的速度经常很慢。清华TUNA镜像源有Anaconda仓库的镜像，将其加入conda的配置即可：
添加Anaconda的TUNA镜像
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```
TUNA的help中镜像地址加有引号，需要去掉

设置搜索时显示通道地址
```
conda config --set show_channel_urls yes
```
9、恢复默认镜像
```
conda config --remove-key channels
```
## 配置spec文件
1、生成spec文件
```
pyinstaller -w -F main.py
```
就会在当前目录下生成一个 main.spec文件
2、Pyinstaller的Spec文件用法[
[Pyinstaller的Spec文件用法](https://blog.csdn.net/tangfreeze/article/details/112240342)
```
pyinstaller main.Spec
```

外部资源文件添加可以写在Analysis datas
```
a = Analysis(
    py_files,
    pathex=[],
    binaries=[],
    datas=[('xls\\*.xlsx','xls')],
    hiddenimports=[],
    hookspath=[],
    hooksconfig={},
    runtime_hooks=[],
    excludes=[],
    noarchive=False,
)
```
修改在exe上的虚拟目录
````
# 资源文件目录访问
def source_path(relative_path):
    # 是否Bundle Resource
    if getattr(sys, 'frozen', False):
        base_path = sys._MEIPASS
    else:
        base_path = os.path.abspath(".")
    return os.path.join(base_path, relative_path)

    # 修改当前工作目录，使得资源文件可以被正确访问
    cd = source_path('')
    os.chdir(cd)
```
## 路径
```
os.path.abspath(sys.argv[0])  当前脚本运行的绝对路径
```
## dist
会在当前目录生成dist 目录，里面有个 main.exe
## 关于 OPENSSL_Uplink(XX……XX,08): no OPENSSL_Applink 处理
[关于 OPENSSL_Uplink(XX……XX,08): no OPENSSL_Applink 处理](https://blog.csdn.net/qq_17328759/article/details/127802435)