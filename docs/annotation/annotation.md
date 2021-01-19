# 元注解
@Target  
指定这个注解可以修饰什么，例如类，方法，字段  
@Retention  
保留多长时间，SOURCE源码中,CLASS默认，字节码文件中,RUNTIME会存在字节码文件，运行时可获取  
@Documented   
@Inherited  
注解类是否有继承性  