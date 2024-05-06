

# QByteArray类的成员函数

## 构造函数

- QByteArray::QByteArray(int size, char ch = '\0')：创建一个指定大小的QByteArray对象，并将其初始化为空字符串。size参数指定QByteArray的大小，ch参数指定QByteArray的初始值。

- QByteArray::QByteArray(const char *str, int n)：创建一个QByteArray对象，并将一个字符数组转换为QByteArray。

## 拷贝构造函数

- QByteArray::QByteArray(const QByteArray &other)：创建一个新的QByteArray对象，并将other的内容复制到新的QByteArray对象中。

- QByteArray::QByteArray(const char *str)：将一个字符数组转换为QByteArray。

## 析构函数

- QByteArray::~QByteArray()：析构QByteArray对象。

## 赋值运算符

- QByteArray &QByteArray::operator=(const QByteArray &other)：将other的内容赋值给this对象。

- QByteArray &QByteArray::operator=(const char *str)：将一个字符数组转换为QByteArray，并将其赋值给this对象。



- QByteArray &QByteArray::operator+=(const QByteArray &other)：将other的内容追加到this对象的末尾。

- QByteArray &QByteArray::operator+=(const char *str)：将一个字符数组转换为QByteArray，并将其追加到this对象的末尾。

- QByteArray &QByteArray::operator<<(const QByteArray &other)：将other的内容插入到this对象的指定位置。

- QByteArray &QByteArray::operator<<(const char *str)：将一个字符数组转换为QByteArray，并将其插入到this对象的指定位置。

- QByteArray &QByteArray::operator>>(QByteArray &other)：将this对象的内容复制到other对象中。

- QByteArray &QByteArray::operator>>(char *&str)：将this对象的内容复制到一个字符数组中，并将其指针赋值给str变量。

- QByteArray &QByteArray::operator&(const QByteArray &other)：将两个QByteArray对象按位与运算。

- QByteArray &QByteArray::operator|(const QByteArray &other)：将两个QByteArray对象按位或运算。

- QByteArray &QByteArray::operator^(const QByteArray &other)：将两个QByteArray对象按位异或运算。

- QByteArray &QByteArray::operator~()：将QByteArray对象转换为其对应的ASCII码值，并返回一个QByteArray对象。

## 其他成员函数

- isEmpty():  判断QByteArray对象是否为空。

- size(): 返回QByteArray对象的长度。

- append(const QByteArray &other): 将other的内容追加到this对象的末尾。

- prepend(const QByteArray &other): 将other的内容插入到this对象的指定位置。

- replace(int pos, int n, const QByteArray &str):  替换this对象中从pos位置开始的n个字符为str的内容。

- insert(int pos, const QByteArray &str): 在this对象的指定位置插入str的内容。

- remove(int pos, int n): 删除this对象中从pos位置开始的n个字符。

- trimmed(): 返回QByteArray对象去掉空格后的结果。

- replace(char ch, const QByteArray &str): 替换this对象中的指定字符为str的内容。

- replace(const char *str): 将str的内容替换到this对象中。

- insert(int pos, const char *str): 在this对象的指定位置插入str的内容。

- indexOf(char ch, int from): 返回this对象中从from位置开始的第一个字符为ch的字符的位置。

- lastIndexOf(char ch, int from): 返回this对象中从from位置开始的最后一个字符为ch的字符的位置。

- left(int n): 返回QByteArray对象的前n个字符。

- right(int n): 返回QByteArray对象的后n个字符。

- mid(int index, int n): 返回QByteArray对象中从index位置开始的n个字符。

- replace(const char *str, int n): 替换QByteArray对象中的指定字符为str的内容。

- replace(char ch, const char *str): 替换QByteArray对象中的指定字符为str的内容。

## 其他成员函数：

- indexOf(QChar ch, int from): 返回QByteArray对象中从from位置开始的第一个字符为ch的字符的位置。

- lastIndexOf(QChar ch, int from): 返回QByteArray对象中从from位置开始的最后一个字符为ch的字符的位置。

- toHex(): 返回QByteArray对象的十六进制表示。

- toBase64(): 返回QByteArray对象的Base64编码表示。

- toPercentEncoding(): 返回QByteArray对象的URL编码表示。

- setNum(int num): 将QByteArray对象中的字符转换为指定的数字。

- toLatin1(): 将QByteArray对象中的字符转换为Latin-1编码。

- fromLatin1(): 将Latin-1编码的字符转换为QByteArray对象中的字符。

- fromPercentEncoding(const QByteArray &str): 将URL编码表示的字符串转换为QByteArray对象中的字符。

- toLocal8Bit(): 将QByteArray对象中的字符转换为本地编码表示的字符串。

- fromLocal8Bit():  将本地编码表示的字符串转换为QByteArray对象中的字符。

- toPercentEncoding(bool *ok = nullptr): 将QByteArray对象中的字符转换为URL编码表示的字符串。如果ok为非空指针，则返回转换是否成功的标志。

- fromPercentEncoding(const char *p, bool *ok = nullptr): 将URL编码表示的字符串转换为QByteArray对象中的字符。如果ok为非空指针，则返回转换是否成功的标志。

- toUtf8(): 将QByteArray对象中的字符转换为UTF-8编码的字符串。

- fromUtf8(): 将UTF-8编码的字符串转换为QByteArray对象中的字符。

- toLocal8Bit(QString& str): 将QByteArray对象中的字符转换为本地编码表示的字符串。

- fromLocal8Bit(const QString& str): 将本地编码表示的字符串转换为QByteArray对象中的字符。

- toLatin1(QString& str): 将QByteArray对象中的字符转换为Latin-1编码的字符串。

- fromLatin1(const QString& str): 将Latin-1编码的字符串转换为QByteArray对象中的字符。

- fromStdString(const std::string& str): 将std::string类型的字符串转换为QByteArray对象中的字符。

- toStdString(std::string& str): 将QByteArray对象中的字符转换为std::string类型的字符串。

