### 需求和解决方案

#### 0. Python基础操作
##### 0.1 List
/#list #len 
```python
#和np.array不一样的是，list没有size这一属性
#获取list的长度
a = [1,2,3]
len(a)
```

##### 0.2 对列表去重

/#list  #去重

reference：  https://m.pythontab.com/article/1194

**不能保证顺序**:

```python
#方法1：利用set
original_list = [1,3,2,2,1,4]
formal_list = list(set(original_list))
#formal_list = [1, 2, 3, 4]
```

```python 
#方法2：利用keys()
original_list = [1,3,2,2,1,4]
formal_list = list({}.fromkeys(original_list).keys())
#formal_list = [1, 2, 3, 4]
```

**保证顺序：**

```python 
#方法1：遍历
original_list = [1,3,2,2,1,4]
formal_list = []
for obj in original_list:
    if obj not in formal_list:
    	formal_list.append(i)
#formal_list = [1, 3, 2, 4]
```

```python
#方法2：利用list.index排序
original_list = [1,3,2,2,1,4]
formal_list = list(set(original_list)) #得到[1, 2, 3, 4]
formal_list = formal_list.sort(key = original_list.index) #利用original_list.index进行重新排序，list.index(obj)返回list中obj第一个匹配项的索引位置
#formal_list = [1, 3, 2, 4]
```





#### 1. OpenCV

##### 1.1 OpenCV 图片文件路径中有中文字符，导致读写错误
   /# opencv #imread  #imwrite # 中文字符 
```python
#解决cv2.imread(path) path中若有中文字符则读取失败的问题
def cv_imread(image_path):
    cv_img = cv2.imdecode(np.fromfile(image_path,dtype=np.uint8),-1)
    return cv_img
 #####
#解决cv2.imwrite(path) path中若有中文字符则写入失败的问题
imwrite_path = os.path.join(images_path,os.path.splitext(os.path.split(j)[-1])[0]+'medianskelimage'+'.jpg')
cv2.imencode('.jpg',medianskelimage)[1].tofile(imwrite_path)
```

##### 1.2   OpenCV 读取某个文件夹里的所有图片
  /#opencv #glob #os #文件夹
```python
#将某个文件夹里的所有文件扩展名为'.bmp'图片的绝对路径存入images_list中
images_path = 'E:/SJTU2017S/胶条检测/胶条图片/远心镜头验证试验'
images_list = glob.glob(os.path.join(images_path,'*.bmp'))
for i,j in enumerate(images_list):
    srcImage = cv2.imdecode(np.fromfile(j,dtype=np.uint8),-1)
```

##### 1.3 保留图片窗口显示
/# waitkey() #显示
```python
    #一直显示图片直到按"q",则关闭全部窗口
    while True:
        if cv2.waitKey(20) & 0xFF == ord('q'):
            break

    cv2.destroyAllWindows()
```

#### 2. pandas
##### 2.1  pandas 将数据保存为Excel或者csv文件
/#pandas #excel #csv
```python
import pandas as pd

#index, result可以为list or np.array
result_df = pd.DataFrame({'id':index,'categories':result}) 
result_df.to_excel('test.xlsx'), index=False) #iDataFrame本身是带有index这一列的,从0开始，写入Excel时默认index为True,即将这一行也写入Excel中，修改index=False，则不写入该列
# result_df.to_csv('MLP_result_1_14_05.csv',index=False,sep=',')
```
#### 3. Numpy
##### 3.1 np.array
/#np.array #size #shape 
```python
import numpy as np
a = np.array([ [1,2,3], [4,5,6] ])
#获取np.array的元素个数
a.size #返回6
#获取np.array的形状，几行几列
row, column = a.shape #返回(2,3)
```

#####   3.2  np.flatnonzero()



#### 4.  os

##### 4.1  os
/#os #合并文件路径 
```python
import os

# 合并文件路径
images_path = 'E:/SJTU2017S/胶条检测/胶条图片/角度60黑色胶条'
images_list = os.path.join(images_path,'2019-01-28_14_30_34_666.bmp')

#获取当前工作目录
cwd = os.getcwd() #current working directory

#将文件路径中的文件夹路径和文件名分开
dirname,filename = os.path.split(Image_path)
```

#### 5. time

##### 5.1  获取当前系统时间

/#time #当前时间 
```python
import time

time.strftime('%Y-%m-%d_%H_%M_%S',time.localtime(time.time())) 
#返回str：'2019-02-24_15_08_44' 
```

  

####  6. 跟txt文件有关的操作

##### 6.1 读取txt(英文段落)，返回一个list

/#txt #re

```python
import string
import re

#读取txt,返回单词list
def read_txt(filename='studentversion.txt'):
###################################################
    #将每行数据存入单独的list中
    # f = open(filename,"r", encoding='utf-8')
     
    # lines = f.readlines()#读取全部内容
     
    # wordlist = []  ## 空列表, 将第i行数据存入list中

    # for i in range(0,lines.__len__(),1): #(开始/左边界, 结束/右边界, 步长) 
    #     for word in lines[i].split():
    #          word=word.strip(string.whitespace)
    #          wordlist.append(word)
######################################################
    #将所有数据存入一个list中
    f = open(filename)

    tmp = f.read() #tmp为string类型
    # wordlist = tmp.split(' ')
    wordlist = re.split(r'[\s\!\?\,\.\"\“]',tmp)#按照空格\!\?\,\.\"将英文段落分词

    print(filename)
    print(wordlist)
    return wordlist
```

##### 6.2  读取两个txt文件，并比较得出相同的地方，连续重复单词数超过6个就报警

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
#By Jack_Fu

import string
import re

#读取txt,返回单词list
def readtxt(filename='studentversion.txt'):
    #将所有数据存入一个list中
    f = open(filename)

    tmp = f.read()
    # wordlist = tmp.split(' ')
    wordlist = re.split(r'[\s\!\?\,\.\"\“]',tmp)

    print(filename)
    print(wordlist)
    return wordlist


def main():
    curfile = readtxt('student_version.txt')
    orifile = readtxt('original_version.txt')

    for i,j in enumerate(curfile):
        print(i,j)
        for m,n in enumerate(orifile):
            num = 0
            copyword = []
            if j == n:
                while curfile[i:i+num+1] == orifile[m:m+num+1] and i+num+1 < len(curfile) and m+num+1 < len(orifile):
                    num += 1
                    # if i+num+1 == len(curfile) or m+num+1 == len(orifile):
                    #     break
                if num > 6:
                    print("这里有抄袭，连续重复字数为：",num)
                    print(curfile[i:i+num])
                    break

if __name__ == '__main__':
    main()
```



