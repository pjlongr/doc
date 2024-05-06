
- anconda + pycharm + pyqt6

- D:\ProgramData\Anaconda3  anaconda3安装目录

- 使用base 环境

- 使用管理员身份打开   Anaconda Prompt (Anaconda3)

- 由于conda 命令 没有 pyqt6，所以使用pip命令安装

- 修改pip命令，默认安装路径

    - 打开D:\ProgramData\Anaconda3\lib\site.py

    - 找到USER_SITE,修改成以下目录

        ```
        USER_SITE = "D:\ProgramData\Anaconda3\lib\site-packages"
        USER_BASE = "D:\ProgramData\Anaconda3\lib\Scripts"
        ```

- 安装 pyqt6

    ```
    pip install sip
    pip install PyQt6
    pip install PyQt6-tools
    ```

- 安装完成后，D:\ProgramData\Anaconda3\Lib\site-packages 目录下多出了一些文件

    ![image-20230902182810336](anaconda%E4%B8%8B%E5%AE%89%E8%A3%85%E7%9A%84python%E7%8E%AF%E5%A2%83%EF%BC%8C%E6%B7%BB%E5%8A%A0pyqt.assets/image-20230902182810336.png)

- D:\ProgramData\Anaconda3\Scripts 目录下多了 pyuic6.exe

- 在Pycharm中配置pyqt工具

    - 配置 designer

        ![image-20230902182948640](anaconda%E4%B8%8B%E5%AE%89%E8%A3%85%E7%9A%84python%E7%8E%AF%E5%A2%83%EF%BC%8C%E6%B7%BB%E5%8A%A0pyqt.assets/image-20230902182948640.png)

        添加QTDesigner工具（可视化制作GUI）
        Program中的路径在**D:\ProgramData\Anaconda3\Lib\site-packages\qt6_applications\Qt\bin\designer.exe**
        Working directory 为 `$ProjectFileDir$` 对应当前目录

    - 配置pyuic

        ![image-20230902183124625](anaconda%E4%B8%8B%E5%AE%89%E8%A3%85%E7%9A%84python%E7%8E%AF%E5%A2%83%EF%BC%8C%E6%B7%BB%E5%8A%A0pyqt.assets/image-20230902183124625.png)

        Program中的路径在**D:\ProgramData\Anaconda3\Scripts\pyuic6.exe**

        pyuic是将QTdesigner中生成的.ui文件转换为.py文件的工具，直接生成python代码，可以用命令直接实现
        `-x $FileName$ -o $FileNameWithoutExtension$.py`
    
- 使用 PyQt6的包

- 测试程序

    ```
    import sys
    from PyQt6 import QtWidgets
    from PyQt6.QtWidgets import QApplication,QLabel
    
    
    
    # Press the green button in the gutter to run the script.
    if __name__ == '__main__':
        app = QApplication(sys.argv)
        label = QLabel("Hello World!")
        label.show()
        app.exec()
    ```

    