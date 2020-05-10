
## Range
客户端 Range
第一个字节的位置和最后一个字节的位置
Range:(unit=first byte pos) - [last byte post]
两边都是闭区间
Range:bytes=0-499
Range:bytes=500-999
Range:byte=-500 最后500
Range:byte=500- 500开始到文件结束
Range:byte=500-600,601-999 两个都可以开始

## Content-Range
服务器 Content-Range
返回单签接受的范围和文件总大小
Content-Range:(unit=first byte pos) - [last byte post]/[entity length]
200 不实用断点续传
206 partial content 使用断点续传




## 自己实现断点续传
todo:自己实现断点续传

