# python3 使用AES加解密

- 安装Crypto模块，依赖库pycryptodome
```
conda install pycryptodome
```
key字符长度决定加密结果,长度16：加密结果AES(128),长度32：结果就是AES(256)
IV 字符串长度是16
```python
from Crypto.Util.Padding import pad
import base64
import random
from base64 import b64decode, b64encode
from Crypto.Cipher import AES

# 去除补位
unpad = lambda s: s[:-ord(s[len(s) - 1:])]

def aes_encrypt(key, iv,aes_str):
    try:
        aes = AES.new(key.encode('utf-8'), AES.MODE_CBC,iv.encode('utf8'))
        pad_pkcs7 = pad(aes_str.encode('utf-8'), AES.block_size, style='pkcs7')  # 选择pkcs7补全
        encrypt_aes = aes.encrypt(pad_pkcs7)
        # 加密结果
        encrypted_text = str(base64.encodebytes(encrypt_aes), encoding='utf-8')  # 解码
        encrypted_text_str = encrypted_text.replace("\n", "")
    except:
        encrypted_text_str="aes_encrypt error"
    
    return encrypted_text_str


def aes_decrypt(key,iv,text):
    try:
        encrypted_text = b64decode(text)
        cipher = AES.new(key=key.encode(), mode=AES.MODE_CBC, IV=iv.encode())
        decrypted_text = cipher.decrypt(encrypted_text)
        return unpad(decrypted_text).decode('utf-8')
    except:
        decrypted_text="text error"
        return decrypted_text
```