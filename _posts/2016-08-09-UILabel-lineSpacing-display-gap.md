---
layout:     post
title:      "UILabel设置行间距只有一行的时候显示不正常"
date:       2016-08-09
author:     "Michael"
header-img: "img/post-bg-2015.jpg"
tags:
    - iOS 
    - Obj-C
---

最近遇到一个问题，UILabel设置了行间距之后，当只有一行的时候，底部也会多出行间距的空白，代码如下：

```
- (void)viewDidLoad {
    [super viewDidLoad];
    
    NSMutableParagraphStyle *paragraph = [[NSMutableParagraphStyle alloc] init];
    paragraph.lineSpacing = 100;
    paragraph.lineBreakMode = NSLineBreakByWordWrapping;
    
    NSDictionary *attributes = @{NSFontAttributeName : [UIFont systemFontOfSize:15], NSParagraphStyleAttributeName: paragraph};
    
    NSString *str = @"我是一行中文字啊";// @"I'm an English sentence.";
    
    CGFloat textWidth = 260;
    CGFloat height = [str boundingRectWithSize:CGSizeMake(textWidth, CGFLOAT_MAX) options:NSStringDrawingUsesLineFragmentOrigin | NSStringDrawingUsesFontLeading attributes:attributes context:nil].size.height;
    
    UILabel *lb = [[UILabel alloc] initWithFrame:CGRectMake(0, 0, textWidth, height)];
    lb.numberOfLines = 0;
    lb.attributedText = [[NSAttributedString alloc] initWithString:str attributes:attributes];
    
    UIView *bgView = [[UIView alloc] initWithFrame:CGRectMake(0, 100, textWidth, height)];
    [bgView addSubview:lb];
    bgView.backgroundColor = [UIColor lightGrayColor];
    [self.view addSubview:bgView];
    NSLog(@"%.2f", height);
}
```

从StackOverflow的一篇[文章](http://stackoverflow.com/a/33661380/1391851)上看到一个使用了不同字体导致baseline显示不对的问题，估计这里也是中文字体渲染导致baseline不对的问题，因为试了一下纯英文，是显示正常的。

---

### Workaround

计算一下当前是否是一行，如果是，就重新设置ParagraphStyle，去掉lineSpacing，没有找到更好的解决方法~
