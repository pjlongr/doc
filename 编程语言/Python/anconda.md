# anconda 命令

- conda info -e         查看所有的虚拟环境

- conda create -n python34 python=3.4  创建python版本是3.4的环境

- python -m site  查看pip安装的默认路径

- python -m site -help  修改pip默认安装目录

    ```
    D:\ProgramData\Anaconda3\lib\site.py [--user-base] [--user-site]
    
    Without arguments print some useful information
    With arguments print the value of USER_BASE and/or USER_SITE separated
    by ';'.
    
    Exit codes with --user-base or --user-site:
      0 - user site directory is enabled
      1 - user site directory is disabled by user
      2 - user site directory is disabled by super user
          or for security reasons
     >2 - unknown error
    ```

    - 打开D:\ProgramData\Anaconda3\lib\site.py
    - 找到USER_SITE,修改成以下目录

    ```
    USER_SITE = "D:\ProgramData\Anaconda3\lib\site-packages"
    USER_BASE = "D:\ProgramData\Anaconda3\lib\Scripts"
    ```

- activate name 切换到不同的虚拟环境，name是虚拟环境的名字

- conda env list 查看所有的环境

- conda remove --name python36 --all 卸载环境

- conda list 查看当前环境中所有安装了的包可以用