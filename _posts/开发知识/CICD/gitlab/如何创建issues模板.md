# 使用git的issue

​	Github为开发者提供了许多便于开发的功能，其中，issues功能被用来追踪各种想法，增强功能，任务，bug等。许多的项目通过centeral bug tracker收集用户的反馈，Github通过issues来实现同样的功能。同理，gitlab上也是相同的机制。

​        Issues经常被误解为一个报告软件bug的地方，其实不然。报告软件bug只是Issues众多功能中的一个。项目维护者可以通过Issues来组织需要完成的任务，例如增加新特性或者审计一个已经上线的功能。同时，还可以将Issues关联某些pull request，一旦合并了某个pull request，这个issue会被自动关闭。同时，你可以将issues添加到看板，也可以将看板的任务转化为issue。

## gitlab创建issue模板

​	找到项目的默认分支，一般是`master`，当然也有设置为`develop`，在此分支下建立 `.gitlab`目录，如果是github则建立`.github`目录，在此目录下，放文件夹`issue_templates` ，之后将模板文件放入，就可以创建issue模板了，当提交issue的时候就可以使用模板了。



