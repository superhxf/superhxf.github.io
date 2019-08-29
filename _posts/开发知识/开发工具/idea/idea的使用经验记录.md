# idea的使用经验记录

## 1、maven插件报错

```
maven install报错：The packaging for this project did not assign a file to the build artifact
```

使用mvn clean install 就可以解决问题，或者先点lifecycle的clean再点install，不要用mvn install:install那个。

