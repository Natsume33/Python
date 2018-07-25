### 日拱一卒无有尽，功不唐捐终入海

---
**代码如下**
```javascript
from  urllib.request import Request,urlopen
import re
import xlwt

class Job(object):
    def __init__(self,cityName='',jobName=''):
        self.baseUrl ='https://search.51job.com/list/'
        self.total_page =1
        self.jobName =jobName
        self.current_page =1
        self.cityName =cityName
        self.headers ={
            'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.62 Safari/537.36'
        }

    def get_city_code_with_name(self, cityName):
        # 获取字典当中所有的value值  也就是城市名字  并且转化成list
        valuelist = list(area.values())
        # 获取指定城市在数组当中的索引
        valueIndex = valuelist.index(cityName)
        # 获取指定城市在数组当中的索引
        keyList = list(area.keys())
        # 获取所有城市对应的key值  并将它转化成list
        key = keyList[valueIndex]
        # 取出城市key值列表中对应的 城市key值
        return key
        
    def start_spider(self):
        code=self.get_code_with_url()
        self.get_total_page(code)

        result=self.get_content_with_code(code)
        sheet,workBook=self.open_execl_file(result)

        row =1
        for index in range(1,self.total_page+1):
            self.current_page=index
            code = self.get_code_with_url()

            result = self.get_content_with_code(code)
            for job,company in result:
                sheet.write(row,0,job)
                sheet.write(row,1,company)
                row +=1
        workBook.save('职位表.xls')
        
    def open_execl_file(self,result):
        workBook=xlwt.Workbook(encoding='utf-8')

        sheet =workBook.add_sheet('职位表')
        sheet.write(0,0,'职位名称')
        sheet.write(0,1,'公司名称')
        return sheet,workBook

    def get_content_with_code(self,code):
        pattern =re.compile(r'<p class="t1 ">.*?<a.*?title="(.*?)".*?>.*?</a>.*?<span class="t2">.*?<a.*?title="(.*?)".*?>.*?</a>',re.S)
        result =pattern.findall(code)
        # print(result)
        return result
        
    def get_total_page(self,code):
        pattern =re.compile(r'<span class="td">共(.*?)页.*?</span>',re.S)
        result =pattern.findall(code)
        self.total_page =int(result[0])

    def get_code_with_url(self):
        url = self.baseUrl + self.get_city_code_with_name(self.cityName)+ ',000000,0000,00,9,99,' + self.jobName + ',2,' + str(self.current_page) + '.html'
        request =Request(url,headers=self.headers)
        response =urlopen(request)
        # decode默认解码的方式utf-8
        # 如果网页不是utf-8的解码方式
        # 则需要在解码的时候指定解码方式为网页的编码方式
        code =response.read().decode('gbk')
        # print(code)
        return code

cityName =input('请输入你想去的城市')
jobName =input('请输入你要找的工作')
job =Job(cityName,jobName)
job.start_spider()
```
