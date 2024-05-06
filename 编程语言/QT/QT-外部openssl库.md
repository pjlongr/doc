

# 环境

win10 + QT 6.2.4 + Qt Creator 10.0.2 (Community) + Win64OpenSSL-3_1_2.exe

# openssl 安装

[Win32/Win64 OpenSSL Installer for Windows - Shining Light Productions (slproweb.com)](https://slproweb.com/products/Win32OpenSSL.html)

![屏幕截图 2023-08-23 233123](QT-%E5%A4%96%E9%83%A8openssl%E5%BA%93.assets/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202023-08-23%20233123.png)

PS：注意安装目录最好不要有空格，否则待会qmake有可能会出问题

# 导入openssl库

- 在项目上右键，选择添加库

    ![image-20230824202905892](QT-%E5%A4%96%E9%83%A8openssl%E5%BA%93.assets/image-20230824202905892.png)

- 选择外部库

    ![image-20230824202938740](QT-%E5%A4%96%E9%83%A8openssl%E5%BA%93.assets/image-20230824202938740.png)

- 选择openssl 安装目录下lib目录下的<font color=red>libcrypto.lib</font>

    ![image-20230824203325908](QT-%E5%A4%96%E9%83%A8openssl%E5%BA%93.assets/image-20230824203325908.png)

- 选择openssl 安装目录下lib目录下的<font color=red>libssl.lib</font>

    ![image-20230824203453092](QT-%E5%A4%96%E9%83%A8openssl%E5%BA%93.assets/image-20230824203453092.png)

- 在构建中执行qmake

    ![image-20230824203630533](QT-%E5%A4%96%E9%83%A8openssl%E5%BA%93.assets/image-20230824203630533.png)

- 在构建中执行重新构建

![image-20230824203649887](QT-%E5%A4%96%E9%83%A8openssl%E5%BA%93.assets/image-20230824203649887.png)

- PS：在添加库的时候一定要去掉<font color=red> 为debug版本添加 'd的'后缀</font>>，没有去掉会提示找不到库

# AES （16 +CBC）加解密

aes.h

```C
#ifndef AES_H
#define AES_H

#include <QByteArray>

class AES {
public:
    // 加密函数
    static QByteArray encrypt(const QByteArray &key, const QByteArray &iv, const QByteArray &data);
    // 解密函数
    static QByteArray decrypt(const QByteArray &key, const QByteArray &iv, const QByteArray &data);
private:
    // 对数据进行padding的函数
    static QByteArray pad(const QByteArray& data);
};

#endif // AES_H

```

aes.cpp

```C
#include "AES.h"
#include "qdebug.h"
#include <openssl/aes.h>
#include <openssl/rand.h>
#include <string.h>
#include <QDebug>
// 对数据进行padding的函数实现
QByteArray AES::pad(const QByteArray& data) {
    int paddingLength =0;
    if((data.length() % 16)!=0)
    {
        paddingLength = 16 - (data.length() % 16);
    }
    QByteArray paddedData = data;
    for (int i = 0; i < paddingLength; i++) {
        paddedData.append((unsigned char)paddingLength);
    }
    return paddedData;
}
// 使用 OpenSSL 进行 AES 加密
QByteArray AES::encrypt(const QByteArray &key, const QByteArray &iv, const QByteArray &data) {
    AES_KEY encryptKey;
    // 设置密钥，这里假设密钥长度为128位（16字节）
    AES_set_encrypt_key(reinterpret_cast<const unsigned char*>(key.data()), 128, &encryptKey);
    QByteArray paddedData = pad(data);
    // 复制初始向量，因为加密后的数据会覆盖掉它
    unsigned char iv_copy[AES_BLOCK_SIZE];
    memcpy(iv_copy, iv.constData(), AES_BLOCK_SIZE);

    // 创建缓冲区存储加密后的数据
    unsigned char outbuf[1024];
    unsigned char ftr[AES_BLOCK_SIZE];
    unsigned len = paddedData.length();
    qDebug()<<len;
    unsigned char *in = (unsigned char*)malloc(len + AES_BLOCK_SIZE);

    // 将数据复制到输入缓冲区
    memcpy(in, paddedData.constData(), len);

    // 使用 OpenSSL 进行 AES 加密
    AES_cbc_encrypt(in, outbuf, len, &encryptKey, iv_copy,AES_ENCRYPT);

    // 将加密后的数据转换为 QByteArray 返回
    QByteArray output(reinterpret_cast<const char*>(outbuf), len);
    free(in);
    return output;
}

// 使用 OpenSSL 进行 AES 解密
QByteArray AES::decrypt(const QByteArray &key, const QByteArray &iv, const QByteArray &data) {
    AES_KEY decryptKey;
    // 设置密钥，这里假设密钥长度为128位（16字节）
    AES_set_decrypt_key(reinterpret_cast<const unsigned char*>(key.data()), 128, &decryptKey);

    // 复制初始向量，因为解密后的数据会覆盖掉它
    unsigned char iv_copy[AES_BLOCK_SIZE];
    memcpy(iv_copy, iv.constData(), AES_BLOCK_SIZE);

    // 创建缓冲区存储解密后的数据
    unsigned char outbuf[AES_BLOCK_SIZE];
    unsigned len = data.length();
    unsigned char *in = (unsigned char*)malloc(len + AES_BLOCK_SIZE);

    // 将数据复制到输入缓冲区
    memcpy(in, data.constData(), len);

    AES_cbc_encrypt(in, outbuf, len , &decryptKey, iv_copy,AES_DECRYPT);

    // 解密后的数据已经存储在 outbuf 中，我们只需要将其转换为 QByteArray 并返回即可
    QByteArray output(reinterpret_cast<const char*>(outbuf), len);
    free(in);
    return output;
}



```

main.cpp

```c++
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    MainWindow w;
    w.show();
    return a.exec();
}
    QString    key_str("11111111111111111111111111111111111");
	//将字符串转换成QByteArray
    QByteArray  key_array=QByteArray::fromHex(key_str.toLatin1());
    QString    iv_str("11111111111111111111111111111111111");
    QByteArray  iv_array=QByteArray::fromHex(iv_str.toLatin1());

    QString    pdata_str("11111111111111111111111111111111111");
    QByteArray  pdata_array=QByteArray::fromHex(pdata_str.toLatin1());
    QByteArray en_data = AES::encrypt(key_array,iv_array,pdata_array);
	//按照十六进制的字符串输出，tohex
    qDebug()<<en_data.toHex();

    QString    data_str("11111111111111111111111111111111111");
    QByteArray  data_array=QByteArray::fromHex(data_str.toLatin1());
    QByteArray de_data = AES::decrypt(key_array,iv_array,data_array);
	//按照十六进制的字符串输出，tohex
    qDebug()<<de_data.toHex();
```

