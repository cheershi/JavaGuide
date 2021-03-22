### <u>find</u>

#### 1.在所有目录中找出大于超过10M的文件

```sh
find / -type f -size +10M
```

#### 2.在所有目录下查找“log”,如发现则无需提示直接删除它们

```sh
find / -name log -exec rm {} \
```

### <u>cat</u>

#### 1.查找日志关键值命令

```sh
cat test.log | grep "关键字" -C 10
```

### netstat

#### 1.查看运行的pid

```sh
netstat -tunlp
```

### du

#### 1.查看当前文件夹大小

```sh
du -ah --max-depth=1
```

