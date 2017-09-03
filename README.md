# dhf
爬取大护法网页
import requests,re,time
from lxml import etree

for i in range(0,2000,20):
    url='https://movie.douban.com/subject/26811587/comments?start='+str(i)+'&limit=20&sort=new_score&status=P'
    r = requests.get(url)
    try:
        a=r.text
        selector=etree.HTML(a)
        content=selector.xpath('//div[@class="comment"]/p/text()')
        for each in content:
            b=open('E:\大护法.txt','a')
            
            b.write(each)
            b.close()
        print('success')
        time.sleep(0.1)
    #print(r.status_code)
    #r.encoding
    except:
        print('fail')

统计大护法出现次数最多的词语
import xlrd
import jieba
import numpy
import codecs
import pandas

f=codecs.open(r"E:\dhf.txt",'r')
content=f.read()
f.close()
segments=[]
segs=jieba.cut(content)
for seg in segs:
    if len(seg)>1:
        segments.append(seg)
segmentDF=pandas.DataFrame({'segment':segments})
df=segmentDF.groupby("segment")["segment"].agg({"计数":numpy.size}).reset_index()
print(df.head(100))
df.to_excel(r"E:\dhf.xls",sheet_name='sheet1')

大护法腾讯文智分析
# -*- coding: utf-8 -*-

from src.QcloudApi.qcloudapi import QcloudApi
f=open("E:\\dhf.txt","r")
lines=f.readlines()
for i in range(1,10,1):
    i=1
    
module='wenzhi'
 
config = {
    'Region': 'gz',
    'secretId': 'AKIDtgWEk94PErMfmg4uDKPyMYYLwLXbq7Zq',
    'secretKey': 'CD5mjV5SdbNxSlsnW2MTP0QegW9apvoU',
    'method': 'post'
}

'''
action='LexicalAnalysis'
       'TextSentiment'
       'TextClassify'
       'TextKeywords'
       'TextDependency'
       'LexicalSynonym'
       'LexicalCheck'
       'ContentTranscode'
'''
action=  'TextSentiment'

#different action has differnet params
params={
   'content':i

                

   }
try:
    service = QcloudApi(module, config)
    print service.call(action, params).decode('unicode_escape')
except Exception, e:
    print 'exception:', e
