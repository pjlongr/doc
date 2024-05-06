

```c++
QString file_full, file_name, file_path，file_suffix ;
QFileInfo fileinfo;
file_full = QFileDialog::getOpenFileName(this,.....);
fileinfo = QFileInfo(file_full);
 //文件名
file_name = fileinfo.fileName(); 
//文件后缀
file_suffix = fileinfo.suffix()
//绝对路径
file_path = fileinfo.absolutePath();
```

