# python 删除字符串中\xe2\x94\u25cf\U0001f511的unicode字符

```python
# 使用unicode-escape编码集，将unicode内存编码值直接存储，并替换空白字符
    content = pre_text.text.encode('unicode_escape').decode('utf-8').replace(' ', '')
    print(pre_text.text.encode('utf-8'))
    # 利用正则匹配\x开头的特殊字符
    result = re.findall(r'\\x[a-f0-9]{2}', content)
    for x in result:
	    # 替换找的的特殊字符
        content = content.replace(x, '')
    #利用正则匹配\u开头的特殊字符
    result = re.findall(r'\\[u|U][a-f0-9]{4,8}', content)
    print(result)
    for x in result:
	    # 替换找的的特殊字符
        content = content.replace(x, ' ')
    # 最后再解码
    content = content.encode('utf-8').decode('unicode_escape')
```