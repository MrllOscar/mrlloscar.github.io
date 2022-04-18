---
title: PHPExcel匯出
date: 2021-12-21 15:34:01
index_img: https://cdn2.techbang.com/system/excerpt_images/58973/special_headline/79ef95adf259045615d356d788c8eaf7.jpg?1528463664
categories: 
- [疑難雜症]
tags: [ PHP, 疑難雜症 , excel]
---

# PHPexcel

有幸開始接處新的東西，[PHPexcel](https://thecolormoon.blogspot.com/2015/11/2015111302.html)

作用是把資料丟給這個obj，讓他產生excel檔案

其實裡面內容都是依照表格去撰寫，利用A1、B3等等的欄位去對應自己要找的位置

其中平常有GUI頁面的功能都會變成指令，像是表格背景顏色就要
```php=
        $sheet->getStyle(sprintf('A%d', $current_row_index))
        ->getFill()->setFillType(PHPExcel_Style_Fill::FILL_SOLID)
        ->getStartColor()->setRGB('d9e2f3');
```
看起來會麻煩許多，其中框線這個功能讓我想破頭，從一開始laravel上方沒有導入物件讓border沒辦法使用，一直到最後用google到的函式測試都沒辦法成功刷出自己要的邊框，這點讓人非常苦惱

其他功能例如自動換行、垂直置中、向左對齊等等
```php=
        $sheet->getStyle(sprintf('A%d', $current_row_index))
        ->getAlignment()->setWrapText(TRUE)->setHorizontal(PHPExcel_Style_Alignment::VERTICAL_CENTER)->setHorizontal(PHPExcel_Style_Alignment::HORIZONTAL_LEFT);
```
都是用laravel的方式呈現

語法基本上google都有，除了那個我不知道為什麼一直呈現不出來的邊框

其他的組成像是換行`$current_row_index++`；找位置輸入值`setCellValue()`；合併儲存格`mergeCells`；等等

組起來就會變成自己想要的樣式

for迴圈也是藉由跑完一圈配上`$current_row_index++`繼續列印下一行

其他額外的設定像是sheet表格(下方)
```php=
        $sheet = $phpObj->createSheet(0);
        $sheet->setTitle('下方表格名稱');
```

最後欄位去設定整張資料表

```php=
        $max_row_seq = count($orders) + 10;
        $global_style = $sheet->getStyle(sprintf('A0:Y%d', $max_row_seq));
        $global_style->getFont()->setSize(22);
```

全部設定完在從他`outPut()`去產生檔案
```php=
    private function outPut($phpExcelObj, $filename)
    {
        $is_pdf = request('is_pdf',0);
        if($this->output != 'php://output') {
            if(!file_exists($this->output)) {
                mkdirs($this->output);
            }
            $this->output = sprintf('%s/%s.%s', $this->output, $filename, $is_pdf ? 'pdf' : 'xls');
            $objWriter = \PHPExcel_IOFactory::createWriter($phpExcelObj, $is_pdf ? 'PDF' : 'Excel5');
        }
        else {
            if ($is_pdf){
                $rendererName = \PHPExcel_Settings::PDF_RENDERER_MPDF;
                $rendererLibraryPath = base_path('vendor/mpdf/mpdf');
                if (!\PHPExcel_Settings::setPdfRenderer($rendererName, $rendererLibraryPath)) {
                    throw new \Exception(
                        'Please set the $rendererName and $rendererLibraryPath values' .
                        PHP_EOL .
                        ' as appropriate for your directory structure'
                    );
                }
                setup_download_excel_header($filename, true);
                $objWriter = \PHPExcel_IOFactory::createWriter($phpExcelObj, 'PDF');
            }else{
                setup_download_excel_header($filename);
                $objWriter = \PHPExcel_IOFactory::createWriter($phpExcelObj, 'Excel5');
            }
        }
        $objWriter->save($this->output);
    }
```

全部的設定設置完成，讓按鈕有觸發效果就可以產生excel檔案了，是不是很厲害阿(˘•ω•˘)

雖然這功能不常用，不過還是紀錄一下讓以後的自己找資料可以更方便一點