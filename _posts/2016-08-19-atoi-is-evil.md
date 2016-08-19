---
layout:     post
title:      "atoi() is evil!"
date:       2016-08-19
author:     "Michael"
header-img: "img/post-bg-2015.jpg"
tags:
    - C
---

在做LeetCode上面的 "8. String to Integer (atoi)" 这道题目的时候，发现atoi()在处理溢出的时候，行为是不可知的

```
#include <stdio.h>
#include <stdlib.h>

int main(int argc, const char * argv[]) {
    char *str = "-2147483649";
    printf("%d\n", atoi(str));
    return 0;
}
```

本以为会输出INT_MIN，结果输出结果是INT_MAX。

后来Google一下发现：http://www.eeng.dcu.ie/~ee105/2005-2006/manual/manual/node16.html

> int atoi(char *s)  
> ...  
> The behaviour where the answer would overflow (i.e., be greater than INT_MAX or less than INT_MIN) is generally **unpredictable** or **indeterminate**--so it is your responsibility to make sure this never arises; if the conversion simply cannot be done (e.g., atoi("xyz") the > return value will be 0.
