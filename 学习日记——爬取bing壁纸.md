### 日拱一卒无有尽，功不唐捐终入海

---
**代码如下**
```javascript
    import time
from urllib.request import Request,urlopen

headers={
    'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:61.0) Gecko/20100101 Firefox/61.0'
}
# 创建一个循环
x=1
while True:
    base_url='https://bing.ioliu.cn/v1/?d={}&w=1280'.format(x)
    print(base_url)
    resquest =Request(base_url,headers=headers)
    res =urlopen(resquest).read()
    # 创建一个存储文件夹
    with open('D:/壁纸/必应壁纸/{}.jpg'.format(x),'wb')as f:
        f.write(res)
        f.close()
    x +=1
    time.sleep(0.3)
```
