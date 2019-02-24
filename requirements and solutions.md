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


​    