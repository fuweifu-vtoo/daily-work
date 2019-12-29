##在服务器上搭建Django博客Project教程
---
假设已经根据[django博客教程]()成功搭建了自己的博客，并且已经将其发布到[github]()，但是还未发布到自己的私人服务器上。
现在需要在服务器上搭建django博客，方便他人访问，可以按照下面的步骤操作：

###在本地还原博客环境
参考之前写过的教程[在新环境搭建Django博客Project教程]()。

###提前注释会报错的代码
之前在本地访问博客时报错[IndexError Exception Value:list index out of range](),这是因为当前数据库中没有**home，about，Todolist，contact，blog**的访问次数的初始化表格数据，这里需要在后台手动创建表格中的这五个数据，在代码中不能初始化，所以要先在代码中把这5个部分的view.py中的[refresh_visitnumber(request,'home')]()代码注释，分别对应报错的那几句代码，便可以正常运行。

###在服务器搭建博客环境
参考教程[使用 Nginx 和 Gunicorn 部署 Django 博客](https://www.zmrenwu.com/courses/django-blog-tutorial/materials/15/)

------
准备在寒假重新使用django+服务器搭建自己的网站，到时候还会对比使用hexo+github.io搭建博客。

到时候会有很多版本问题，需要一个个解决。

除此之外，我还会更加系统的学习一下django，依旧是参考追梦人物的博客[HelloDjango - Django博客教程(第二版)](https://www.zmrenwu.com/courses/hellodjango-blog-tutorial/),这是一套Django全栈开发系列教程，现在还在连载中。