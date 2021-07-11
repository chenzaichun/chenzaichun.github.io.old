+++
title = "python获取中文拼音首字母以进行检索"
date = "2012-05-24T14:11:00+08:00"
comments = true
categories = ["programming"]
tags = ["python"]
description = ""
+++


主要的原理是GBK汉字是按拼音顺序编码的。

源代码如下:

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

def multi_get_letter(str_input):
    if isinstance(str_input, unicode):
        unicode_str = str_input
    else:
        try:
            unicode_str = str_input.decode('utf8')
        except:
            try:
                unicode_str = str_input.decode('gbk')
            except:
                print 'unknown coding'
                return

    return_list = []
    for one_unicode in unicode_str:
        #print single_get_first(one_unicode)
        return_list.append(single_get_first(one_unicode))
    return "".join(return_list)    

def single_get_first(unicode1):
    str1 = unicode1.encode('gbk')
    try:        
        ord(str1)
        return str1
    except:
        asc = ord(str1[0]) * 256 + ord(str1[1]) - 65536
        if asc >= -20319 and asc <= -20284:
            return 'a'
        if asc >= -20283 and asc <= -19776:
            return 'b'
        if asc >= -19775 and asc <= -19219:
            return 'c'
        if asc >= -19218 and asc <= -18711:
            return 'd'
        if asc >= -18710 and asc <= -18527:
            return 'e'
        if asc >= -18526 and asc <= -18240:
            return 'f'
        if asc >= -18239 and asc <= -17923:
            return 'g'
        if asc >= -17922 and asc <= -17418:
            return 'h'
        if asc >= -17417 and asc <= -16475:
            return 'j'
        if asc >= -16474 and asc <= -16213:
            return 'k'
        if asc >= -16212 and asc <= -15641:
            return 'l'
        if asc >= -15640 and asc <= -15166:
            return 'm'
        if asc >= -15165 and asc <= -14923:
            return 'n'
        if asc >= -14922 and asc <= -14915:
            return 'o'
        if asc >= -14914 and asc <= -14631:
            return 'p'
        if asc >= -14630 and asc <= -14150:
            return 'q'
        if asc >= -14149 and asc <= -14091:
            return 'r'
        if asc >= -14090 and asc <= -13119:
            return 's'
        if asc >= -13118 and asc <= -12839:
            return 't'
        if asc >= -12838 and asc <= -12557:
            return 'w'
        if asc >= -12556 and asc <= -11848:
            return 'x'
        if asc >= -11847 and asc <= -11056:
            return 'y'
        if asc >= -11055 and asc <= -10247:
            return 'z'
        return ''

def printresult(str):
    print('中文: "%s" --> 首字母拼音: "%s"' % (str, multi_get_letter(str)))
    
if __name__ == '__main__':
    printresult('木哈哈')
    printresult('小李')
    printresult('大王')
    printresult('大d王m')
```

我的修改版本:

<!--more-->

```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

def multi_get_letter(str_input):
    if isinstance(str_input, unicode):
        unicode_str = str_input
    else:
        try:
            unicode_str = str_input.decode('utf8')
        except:
            try:
                unicode_str = str_input.decode('gbk')
            except:
                print 'unknown coding'
                return

    return_list = []
    for one_unicode in unicode_str:
        #print single_get_first(one_unicode)
        return_list.append(single_get_first2(one_unicode))
    return "".join(return_list)

def single_get_first2(unicode1):
    str1 = unicode1.encode('gbk')
    try:        
        ord(str1)
        return str1
    except:
        asc = ord(str1[0]) * 256 + ord(str1[1]) - 65536

        asc_list = (-20320, -20284, -19776, -19219, 
                    -18711, -18527, -18240, -17923, 
                    -17418, -16475, -16213, -15641, 
                    -15166, -14923, -14915, -14631, 
                    -14150, -14091, -13119, -12839, 
                    -12557, -11848, -11056, -10247)
    
        letter_list = ('a', 'b', 'c', 'd',
                       'e', 'f', 'g', 'h',
                       'j', 'k', 'l', 'm',
                       'n', 'o', 'p', 'q',
                       'r', 's', 't', 'w',
                       'x', 'y', 'z')

        for i in range(0, len(letter_list)):
            if asc >= (asc_list[i]+1) and asc <= asc_list[i+1]:
                return letter_list[i]

        return ''
    
def printresult(str):
    print('中文: "%s" --> 首字母拼音: "%s"' % (str, multi_get_letter(str)))
	    
if __name__ == '__main__':
    printresult('木哈哈')
    printresult('小李')
    printresult('大王')
    printresult('大d王m')
    printresult('哦i哎v')
    printresult('啊吧才的恶发跟好就看了卖你哦怕去染色体我小样猪')
```

我们可以用 =二分查找= 优化一下：

```python
def single_get_first3(unicode1):
    str1 = unicode1.encode('gbk')
    try: 
        ord(str1)
        return str1
    except:
        asc = ord(str1[0]) * 256 + ord(str1[1]) - 65536

        asc_list = (-20320, -20284, -19776, -19219, 
                    -18711, -18527, -18240, -17923, 
                    -17418, -16475, -16213, -15641, 
                    -15166, -14923, -14915, -14631, 
                    -14150, -14091, -13119, -12839, 
                    -12557, -11848, -11056, -10247)
    
        letter_list = ('a', 'b', 'c', 'd',
                       'e', 'f', 'g', 'h',
                       'j', 'k', 'l', 'm',
                       'n', 'o', 'p', 'q',
                       'r', 's', 't', 'w',
                       'x', 'y', 'z')

        left, right = 0, len(letter_list)-1
        while left <= right:
            middle = (left+right)/2
            if asc >= (asc_list[middle]+1) and asc <= asc_list[middle+1]:
                return letter_list[middle]
            
            if asc_list[middle+1] > asc:
                right = middle - 1
            elif asc_list[middle]+1 < asc:
                left = middle + 1

        return ''
```

运行结果如下:

```sh
# python py_pinyin.py
中文: "木哈哈" --> 首字母拼音: "mhh"
中文: "小李" --> 首字母拼音: "xl"
中文: "大王" --> 首字母拼音: "dw"
中文: "大d王m" --> 首字母拼音: "ddwm"
中文: "啊吧才的恶发跟好就看了卖你哦怕去染色体我小样猪" --> 首字母拼音: "abcdefghjklmnopqrstwxyz"
```

[这里]: http://blog.csdn.net/liveguanquan/article/details/6037966
