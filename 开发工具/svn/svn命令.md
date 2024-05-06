# SVN 命令

- SVN Cleanup错误 Faild to run the wc db work queue

    

```
    在 cmd 中通过 cd 命令进入到 svn 工程中的 .svn 目录中
    执行命令 sqlite3 wc.db "select * from work_queue" 查看有哪些文件报错
    执行命令 sqlite3 wc.db "delete from work_queue"
    执行命令 sqlite3 wc.db "delete from wc_lock"
```

- svn 冲突

```
svn revert -R ./
svn update
```

- 一次性增加所有新增的文件到svn库

```
 #svn status列出 ? 开头的文件表示尚未添加进过版本库的文件
 svn st | awk '{if ($1 == "?") {print $2} }' | xargs svn add         
```

- 一次性从svn库删除所有需要删除的文件

```
svn st | awk '{if ($1 == "!") {print $2}}' | xargs svn rm
```

- 最后直接提交你的修改(注意：这里的-F 代表上传的注释从comment.txt文件读取)

```
svn ci -F comment.txt
```

- 删除所有.svn目录

```
find . -type d -name ".svn"|xargs rm -rf
```

- 取消目录

```
svn revert --depth=infinity 目录名
```



