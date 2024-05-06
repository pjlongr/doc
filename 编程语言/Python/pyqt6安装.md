
# pyqt6 安装和pycharm配置，anconda 安装的python环境

## 安装pyqt6

```
pip install pyqt6 pyqt6-tools
```
pyqt6 将安装在 Anaconda3\Lib\site-packages  目录下
## pycharm 配置
### 配置designer
pycharm -->> setting --->> External Tools 点击 + 
![在这里插入图片描述](https://img-blog.csdnimg.cn/c022f85affbc43c4bf7b84056440d798.png#pic_center)
```
Program : D:\ProgramData\Anaconda3\Lib\site-packages\qt6_applications\Qt\bin\designer.exe
Working directory :  $ProjectFileDir$
```
### 配置pyuic
pycharm -->> setting --->> External Tools 点击 + 
![在这里插入图片描述](https://img-blog.csdnimg.cn/f5539c51a26e4cb690081b8a3501cfcf.png#pic_center)
```
Program :  D:\ProgramData\Anaconda3\Scripts\pyuic6.exe
Arguments :  -x $FileName$ -o $FileNameWithoutExtension$.py
Working directory: $FileDir$
```
-x 参数用于生成可执行的 Python 代码，而不是简单的 Python 类定义。



