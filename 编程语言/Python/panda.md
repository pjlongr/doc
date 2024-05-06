# python pandas 保存内容到excel

filepath：xlsx文件路径
sheetname：
datelist：需要写入的数据，是 list
```python
import pandas as pd
import os
def write_to_one_excel(filepath,sheetname,datelist):
    print("正在保存 %s to %s\n" % (sheetname,filepath))
    #创建一个空的Dataframe
    #columns 只有一项是用[]
    # result =pd.DataFrame(columns=['name'])
    result =pd.DataFrame(columns=('name','url','icon'))
    for list in datelist:
        #print(list)
        result=result.append(pd.DataFrame({'name':[list[0]],'url':[list[1]],'icon':[list[2]]}),ignore_index=True)
    #判断文件是否存在
    if os.path.exists(filepath):
        # 然后再实例化ExcelWriter
        #使用with方法打开了文件，生成的文件操作实例在with语句之外是无效的，因为with语句之外文件已经关闭了。无需writer.save()和writer.close()，否则报错
        with pd.ExcelWriter(filepath,engine="openpyxl",mode='a') as writer:
            # 保存到本地excel
            # 接下来还是调用to_excel, 但是第一个参数不再是文件名, 而是上面的writer
            result.to_excel(writer,sheet_name=sheetname)
    else:
        result.to_excel(filepath,sheet_name=sheetname, index=False)
    # writer.save()
    # writer.close()
```