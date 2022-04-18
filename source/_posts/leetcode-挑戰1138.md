---
title: leetcode 挑戰1138.
date: 2022-01-06 17:43:30
excerpt: 趁忙裡偷閒的時候來刷一下leetcode
index_img: /img/2022-01-06 174539.png
categories: 
- [疑難雜症]
tags: [ leetcode, 疑難雜症, 多重]
---

# 1138 alphabet-board-path

![](https://assets.leetcode.com/uploads/2019/07/28/azboard.png)

[alphabet-board-path題目](https://leetcode.com/problems/alphabet-board-path/)就如網址所寫的

是一個將26個英文單字拆解成一張表格，透過起始位置`[0][0]`去查找那個英文字所在的位置

並且根據上一個位置去找下一個英文單字

雖然我作品做出來還有些問題在裡面，執行速度跟判斷都還有一些問題

但第一次挑戰成功真的很開心！中間其實因為z的關係有卡關，還有前面怎麼篩選出幾個英文有卡關(最後還是選擇用for迴圈)

我相信code應該還有更好的、更快速的方法可以使用，希望有空能再繼續研究

附上結果及程式碼

![](/img/2022-01-06 174539.png)

```php
class Solution {

    /**
     * @param String $target
     * @return String
     */
    function alphabetBoardPath($target) {
        $result = ''; //回傳值
        $z=''; //給z特殊狀況使用(整列只有他)
        $board = ["abcde", "fghij", "klmno", "pqrst", "uvwxy", "z"]; 
        $targetWord = preg_split('//', $target, -1, PREG_SPLIT_NO_EMPTY);
        $row=0; //縱向(選列)
        $column=0; //橫向(選欄)
        foreach($targetWord as $word){ //進來的文字處理
            foreach($board as $rowKey => $value){ //文字表個建立($row)
                $brokeValue = preg_split('//',$value,-1,PREG_SPLIT_NO_EMPTY);
                $newBoard[$rowKey] = $brokeValue;
                foreach($brokeValue as $columnKey => $boardWord){ //文字表個建立($column)
                    if($boardWord === $word){ //判斷文字表內文字是否與當前文字相同
                        $row = $rowKey-$row;
                        $column = $columnKey-$column;
                        if($column!=0 && $rowKey===5){ //針對Z做處理
                            $row-=1;
                            $z='D';
                        }
                        if($row>0){
                            for($i=$row;$i>=1;$i--){
                                $result.='D';
                            }
                        }
                        if($row<0){
                            for($i=$row;$i<=-1;$i++){
                                $result.='U';
                            }
                        }
                        if($column>0){
                            for($j=$column;$j>=1;$j--){
                                $result.='R';
                            }
                        }
                        if($column<0){
                            for($j=$column;$j<=-1;$j++){
                                $result.='L';
                            }
                        }
                        if($z){ //針對Z做處理
                            $result.=$z;
                            $z='';
                        }
                        $result.='!';
                        $row = $rowKey;
                        $column = $columnKey;
                    }
                }
            }
        }
        return $result;
    }
}
```

努力努力(˘•ω•˘)