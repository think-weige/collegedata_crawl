# collegedata_crawl
You can craw any chinese college datas by this py code,and we can learn from each other.
import requests
import re
def getHTMLText(url):
#向网页发送请求，读取网页内容
    try:
        r=requests.get(url,timeout=30)
        r.raise_for_status()
        r.encoding=r.apparent_encoding
        return r.text
异常处理
    except:
        return "产生异常"
def parse_one_page(html):
#观察网页结构，可以发现高校录取数据都封装在<td> </td>标签之中，但是还有录取数据却也在<td><a> </td></a>标签之中，
#所以首先利用正则表达式中的sub替换掉后者标签，使其成为与前者一样的标签后，利用正则表达式，通配符，即可提取出网页内容
    res='<td.*?>(.*?)<.*?/td>'
    page1=re.sub('<a.*?=".*?">','',html)
    texts=re.findall(res,page1,re.S|re.M)
    for m in texts:
        print(m)
def main(p):
#main函数内容为获取网页url，并将其输出
    url="http://college.gaokao.com/spepoint/y2017/o郑州大学/p"+str(p)+'/'
#此处为爬取网页的url
    html=getHTMLText(url)
    parse_one_page(html)
#遍历，获取第一页到第33页的内容，并打印输出
for i in range(1,33):
    main(i)
