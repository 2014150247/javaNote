# 单例
反射会破坏单例
序列化也会破坏单例
只要在Singleton类中定义readResolve就可以解决该问题
![](../pic/Image7.png)