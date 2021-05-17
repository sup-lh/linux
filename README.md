
#   写入json文件的时候，有两种方式：


##   第一种是，一行紧接着一行挤着写入，这时就需按美化键来分行，即这种写入方式为JavaScript的标准写法


##   第二种是，分行写，这时特容易飘红报错，但是不虚，能读就可以，让他飘！

>[!Note|lable:第一种：] 不需要解释太多，咱dumps化然后写进的时候就是这种，咱进去json文件第一件事就是美化不是么

>[!Danger|lable:第二种] 方法有很多种来实现，以下咱👧直接上python代码！

 
-   ```python
    json_list=json.dumps(dictda,ensure_ascii=False)
    with open('json.json','a',encoding='utf-8',newline='')as f:
        f.write(json_list)
        f.write('\n')
    ```

-   ```python
    # >>>  scrapy pipeline里面的写入时的方法
    # >>>  存json
    from scrapy.exporters import JsonItemExporter,JsonLinesItemExporter
    class MyprojectPipelineJson(object):


    def __init__(self):
        self.file=open('datas.json','wb')
        self.exporter = JsonLinesItemExporter(self.file,encoding='utf-8')
        self.exporter.start_exporting()
    
    def process_item(self,item,spider):
        self.exporter.export_item(item)
        # >>>  self.exporter.indent=1
        return item
    
    def close_spider(self,spider):
        self.exporter.finish_exporting()
        self.file.close()


    # >>>  这里的JsonLinesItemExporter或者 self.exporter.indent=1  都可以换行

    
    ```

# 存入json时出现乱码的解决办法

##  requests
- 处理方法：
- html.encoding = 'unicode_escape'
- 或
- json.dumps(html.text,ensure_ascii=False)
##  scrapy
- 处理方法：
- response.body.decode('unicode_escape')
- 因为scrapy默认编码不是ascii，而是别的

