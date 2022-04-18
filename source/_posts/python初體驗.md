---
title: python初體驗
date: 2021-12-29 12:02:07
excerpt: 趁忙裡偷閒的時候來研究一下python
index_img: /img/2022-01-06 175749.png
categories: 
- [Python]
tags: [ Python, 初學 , 陣列 , 字典]
---

# python 初體驗

網路上看了很多**python跟php差別在哪**、**python跟php哪個好？**的文章

發現其實很多大神都是雙腳併用，畢竟領域不同

python出生於1991年、PHP出生於1994年，在web上PHP算是無可取代的，但python的多方面和簡易性也是數一數二

朋友剛好有一個便當需求，於是就想說來做個列表試試看

```php
$food=['排骨','雞腿','鯖魚'];
$amount=[80,100,120];
for($i=0;$i<3,$i++){
  print($food[$i].'便當'.$amount[$i].'元');
}
$sale=['排骨'=>80,'雞腿'=>100,'鯖魚'=>120];
foreach($sale as $key =>$value){
  print($key.'便當'.$value.'元');
}
```

```python
food=['排骨','雞腿','鯖魚']
amount=[80,100,120]
for i in range(3):
  print(food[i],'便當',amount[i],'元')
sale={'排骨':80,'雞腿':100,'鯖魚':120}
for i in sale:
  print(i,'便當',sale[i],'元')
```

修正版
---

```php
$sale=['排骨'=>80,'雞腿'=>100,'鯖魚'=>120];
$buy=['排骨'=>3,'雞腿'=>1];
$total=0;
foreach ($sale as $keys => $values){
  if(isset($buy[$keys])){
    $total += $sale[$keys] * $buy[$keys];
  }
  print("$keys 便當$values 元<br>");
}
print("共買了<br>");
foreach($buy as $keyb => $valueb){
  print("$valueb 個$keyb 便當<br>");
}
print("總共 $total 元");
```

```python
sale={'排骨':80,'雞腿':100,'鯖魚':120}
buy={'排骨':3,'雞腿':1}
total=0
for i in sale:
  if i in list(buy.keys()):
    total+=sale[i]*buy[i]
  print(i,'便當',sale[i],'元')
print('共買了',end='')
for j in buy:
  print('',buy[j],'個',j,'便當',end='，')
print('總共',total,'元')
```

赫然發現兩者的陣列跟字典的使用方式就不同了

key的表達方式來說，兩者混用有時候會不知道生出來的東西是什麼

2022.1.6------------

後來沒事跑去玩了以前老師寫了很多的[星星迴圈](https://hackmd.io/@mackliu/HkItZrwDH)去試試看迴圈邏輯

* 直角三角形

```
*    
**   
***  
**** 
*****
```

```python
for i in range(5):
  for j in range(5):
    if i!=j and i<j:
      print(' ',end='')
    else:
      print('*',end='')
  print()
```

* 反向

```python
for i in range(5):
  for j in range(5):
    if i+j<4:
      print(' ',end='')
    else:
      print('*',end='')
  print()
```

```
    *
   **
  ***
 ****
*****
```

* 倒立

```python
for i in range(5):
  for j in range(5):
    if i+j>=5:
      print(' ',end='')
    else:
      print('*',end='')
  print()
```

```
*****
**** 
***  
**   
*    
```

* 金字塔

```python
for i in range(5):
  for j in range(9):
    if i+j<4 or j-i>=5:
      print(' ',end='')
    else:
      print('*',end='')
  print()
```

```
    *    
   ***   
  *****  
 ******* 
*********
```

* 倒立金字塔

```python
for i in range(5):
  for j in range(9):
    if i<j+1 and i+j<9:
      print('*',end='')
    else:
      print(' ',end='')
  print()
```

```
*********
 ******* 
  *****  
   ***   
    *    
```

* 稜形

```python
for i in range(9):
  for j in range(5):
    if i+j<4 or i-j>=5:
      print(' ',end='')
    else:
      print('*',end='')
  for k in range (4):
    if k+i>=4 and i+k<8:
      print('*',end='')
  print()
```

```
    *
   ***
  *****
 *******
*********
 *******
  *****
   ***
    *
```

* 方形

```python
for i in range(9):
  for j in range(9):
    if j>0 and j<8 and i>0 and i<8:
      print(' ',end='')
    else:
      print('*',end='')
  print()
```

```
*********
*       *
*       *
*       *
*       *
*       *
*       *
*       *
*********
```

* 方形帶交叉線

```python
for i in range(9):
  for j in range(9):
    if j>0 and j<8 and i>0 and i<8 and i!=j and i+j!=8:
      print(' ',end='')
    else:
      print('*',end='')
  print()
```

```
*********
**     **
* *   * *
*  * *  *
*   *   *
*  * *  *
* *   * *
**     **
*********
```
其中有一些寫法不同，是因為都是到新的重新再去思考，但都不是完美解答

想考驗自己邏輯的可以來挑戰看看

* 大樂透開獎機

```python
import random
a=random.sample(range(1,50),7)
print('本期大樂透開獎號碼：',a[0:6])
print('特別號：',a[6])
```

其中印出陣列是`a[0:6]`，意思是說從第幾個開始取到第幾個，而`a[6]`則是取出第七位(包含0)

看來今天又比昨天更進步了一點呢(˘•ω•˘)