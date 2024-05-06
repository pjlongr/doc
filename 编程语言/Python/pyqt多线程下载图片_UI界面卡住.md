# pyqt多线程下载图片，UI界面卡住

## 创建一个Qthead类，专门用来进行多线程下载图片

```python
class ImageDownloader(QThread,QObject):
    setin = pyqtSignal(str)
    setout = pyqtSignal()
    def __init__(self, target_url):
        super().__init__()
        self.target_url = target_url
        self.showinfo = ''

    def run(self):
    	# 发信号给UI，UI进行刷新
        self.setin.emit(self.showinfo)
        max_threads = os.cpu_count()
        print(max_threads)
        # 创建线程池，多线程下载数据
        with ThreadPoolExecutor(max_workers=(max_threads / 2)) as executor:
            for icon_info in url_info_list:
                executor.submit(self.download_one_pic, icon_info)
		# 线程结束，发送信号给UI，通知已经结束了
        self.setout.emit()
```

## 界面类调用Qthread 类的时候，得用界面类的成员变量，不能使用临时变量

```python
        # 开启qt  线程
        self.work = ImageDownloader(target_url)
        self.work.setin.connect(self.icon_tab_refersh)
        self.work.setout.connect(self.icon_tab_finish_refersh)
        self.work.start()

```
