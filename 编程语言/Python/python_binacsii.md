# 二进制数据和ASCII字符串之间进行转换的功能

## 将字符串转换为十六进制值,binascii.hexlify函数

```python
import binascii  
  
# 假设你有一个字符串  
s = "Hello, World!"  
  
# 使用binascii.hexlify方法将字符串转换为十六进制值  
hex_string = binascii.hexlify(s.encode('utf-8'))  
  
print(hex_string)
```

## 十六进制值转换回原始的字符串，你可以使用`binascii.unhexlify()`函数

```python
import binascii  
  
# 假设你有一个十六进制字符串  
hex_string = "48656c6c6f2c20576f726c6421"  # 这是"Hello, World!"的十六进制表示  
  
# 使用binascii.unhexlify方法将十六进制值转换回字符串  
original_string = binascii.unhexlify(hex_string)  
  
print(original_string)  # 输出: b'Hello, World!'
```







