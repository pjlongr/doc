

 qt 完成CRC32 函数，Qbytearray作为参数,已经知道crcTable

```C

const uint32_t crcTable[256] = {
    0x00000000, 0x04c11db7, 0x09823b6e, 0x0d4326d9, 0x130476dc, 0x17c56b6b,

    0x1a864db2, 0x1e475005, 0x2608edb8, 0x22c9f00f, 0x2f8ad6d6, 0x2b4bcb61,

    0x350c9b64,0x31cd86d3,0x3c8ea00a,0x384fbdbd,0x4c11db70, 0x48d0c6c7,

    0x4593e01e, 0x4152fda9, 0x5f15adac, 0x5bd4b01b, 0x569796c2, 0x52568b75,

    0x6a1936c8, 0x6ed82b7f, 0x639b0da6, 0x675a1011, 0x791d4014, 0x7ddc5da3,

    0x709f7b7a,0x745e66cd,0x9823b6e0,0x9ce2ab57,0x91a18d8e,0x95609039,

    0x8b27c03c,0x8fe6dd8b,0x82a5fb52, 0x8664e6e5, 0xbe2b5b58,0xbaea46ef,

    0xb7a96036, 0xb3687d81, 0xad2f2d84, 0xa9ee3033, 0xa4ad16ea, 0xa06c0b5d,

    0xd4326d90, 0xd0f37027, 0xddb056fe,0xd9714b49,0xc7361b4c, 0xc3f706fb,

    0xceb42022, 0xca753d95, 0xf23a8028, 0xf6fb9d9f, 0xfbb8bb46, 0xff79a6f1,

    0xe13ef6f4,0xe5ffeb43,0xe8bccd9a,0xec7dd02d,0x34867077, 0x30476dc0,

    0x3d044b19, 0x39c556ae, 0x278206ab, 0x23431b1c, 0x2e003dc5, 0x2ac12072,

    0x128e9dcf, 0x164f8078, 0x1b0ca6a1, 0x1fcdbb16, 0x018aeb13, 0x054bf6a4,

    0x0808d07d, 0x0cc9cdca, 0x7897ab07, 0x7c56b6b0, 0x71159069,0x75d48dde,

    0x6b93dddb,0x6f52c06c,0x6211e6b5,0x66d0fb02, 0x5e9f46bf, 0x5a5e5b08,

    0x571d7dd1, 0x53dc6066, 0x4d9b3063, 0x495a2dd4, 0x44190b0d, 0x40d816ba,

    0xaca5c697, 0xa864db20, 0xa527fdf9,0xa1e6e04e,0xbfa1b04b, 0xbb60adfc,

    0xb6238b25, 0xb2e29692, 0x8aad2b2f, 0x8e6c3698, 0x832f1041, 0x87ee0df6,

    0x99a95df3, 0x9d684044, 0x902b669d, 0x94ea7b2a, 0xe0b41de7, 0xe4750050,

    0xe9362689, 0xedf73b3e,0xf3b06b3b,0xf771768c,0xfa325055, 0xfef34de2,

    0xc6bcf05f, 0xc27dede8, 0xcf3ecb31, 0xcbffd686, 0xd5b88683, 0xd1799b34,

	---未完
};

uint32_t MainWindow::crc32(const QByteArray &data, uint32_t initialCrc)
{
    int i;
    unsigned long result = 0xffffffff;
    const unsigned char *ptr = (const unsigned char*)data.constData();
    for (i = 0; i < data.length(); i++)
    {
        result = (result << 8) ^ crcTable[((result >> 24) ^ (*ptr++)) & 0xff];
    }
    return result;
}
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);
    QString    iv_str("xxxxxxxxxxxxxxxxxxxxxxxx");
    //xxxxxxxxxxxxxxxxxxxxxxxx 3a749514
    QByteArray  data=QByteArray::fromHex(iv_str.toLatin1());
    uint32_t crcValue = crc32(data);
    qDebug() << "CRC value:" << crcValue;
    // 将uint32_t转换为字节序列
    QByteArray byteArray = QByteArray::fromRawData(reinterpret_cast<const char*>(&crcValue), sizeof(uint32_t));
    qDebug()<<byteArray.toHex();

}

```

