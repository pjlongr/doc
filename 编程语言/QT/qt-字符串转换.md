

- char* 转换成 QByteArray

    ```c++
    char *s=new char[8];
    QByteArray data= QByteArray(s,8);
    //如果char *s里面的值是16进制的值，直接转成16进制显示
    data.toHex();
    ```

- QByteArray 转 QString

    ```c++
    QByteArray data;
    QString str=QString(data);
    ```

- QString的mid()函数来截取字符串。它有两种用法：

    1. QString mid(int pos, int length)：返回从pos位置开始长度为length的子字符串。
    2. QString mid(int pos)：返回从pos位置到字符串末尾的子字符串。

- 将16进制表示的QString 转换成对应的byte数据

    - 方法一

    ```C
    QByteArray  data_array=QByteArray::fromHex(data_str.toLatin1());
    ```

    - 方法二：

    ```c++
    QByteArray MainWindow::hexstrToByte(QString str)
    {
        QByteArray bytearry;
        bool ok;
        if(str.size()%2!=0)
        {
             
            return QByteArray::fromHex("字符串不符合标准");
        }
        int len = str.size();
        for(int i=0;i<len;i+=2)
        {
            bytearry.append(char(str.mid(i,2).toUShort(&ok,16)));
        }
        return bytearry;
    
    ```

- 判断字符串是否是16进制的字符串

    > 使用 `toLatin1()` 函数将 `QChar` 转换为 `uchar`，然后使用 `std::isxdigit()` 函数来检查字符是否是有效的16进制数字（0-9和A-F）。如果发现非法的字符，我们将 `isValid` 设置为 `false` 并退出循环。如果循环完成后 `isValid` 仍然为 `true`，则表示整个字符串是有效的16进制字符串。
    >
    > 请注意，为了使用 `std::isxdigit()` 函数，你需要包含 `<cctype>` 头文件。此外，请确保你的编译环境支持 C++ 标准库。

    ```c++
    #include <QChar>  
    #include <cctype>  
      
    bool isValidHexString(const QString& str) {  
        bool isValid = true;  
        for (const QChar& ch : str) {  
            uchar c = ch.toLatin1();  
            if (!std::isxdigit(c)) {  
                isValid = false;  
                break;  
            }  
        }  
        return isValid;  
    }
    ```

    