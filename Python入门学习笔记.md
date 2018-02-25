# Python入门学习笔记

## Python字符串
```python
print 'ABC';
print r'\(~_~)/ \(~_~)/' # raw 字符串，里面的字符就不需要转义了。
print '''Line 1
      Line 2
      Line 3'''     # 多行字符串
print r'''Python is created by "Guido".
      It is free and easy to learn.
      Let's start learn Python in imooc!''' # 多行raw字符串
print ur'''Python的Unicode字符串支持"中文",
      "日文",
      "韩文"等多种语言''' # raw+多行 Unicode字符串
```

如果中文字符串在Python环境下遇到 UnicodeDecodeError，这是因为.py文件保存的格式有问题。可以在第一行添加注释
```python
# -*- coding: utf-8 -*-
```