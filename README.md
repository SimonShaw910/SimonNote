# SimonNote
Node For Learn

1. @RestController 和@Controller区别

   相同点：都用来标识Spring的这个类可以接受http请求;
   
   @RestController 相当于@Controller+@ResponseBody 只能返回json数据,配置的视图解析器 InternalResourceViewResolver不起作用;
   
   
   @Controller可以返回多种数据，包括jsp和html等,如果需要返回jsp,html等需要配合视图解析器InternalResourceViewResolver；如果需要返回json,xlm或者自定义mediaType等到页面，需要加上@ReponseBody注解。

2. SQL执行很慢可能原因整理

   1）一般情况下很正常，偶尔慢；
   
      a.数据库insert或者updat数据时，一般现在内存中更新，更新后会把更新记录写入redo log日记中，等待空闲时通过日记同步持久化到磁盘。
      
      如果数据库一直很忙，更新也比较频繁，redo log会很快被写满，数据库只能暂停其他操作将数据同步到磁盘，导致sql执行很慢
      
      b.执行的语句被其他地方占用加锁，需要等待锁释放（可能是整张表，可能是某一行某一列）
      
   2）一直很慢
   
      a.没有索引
      
      b.字段有索引但是没有用到
      
  #关于事务
   
      
@Service
class A{
    @Transactinal
    method b(){...}
    
    method a(){    //标记1
        b();
    }
}
 
//Spring扫描注解后，创建了另外一个代理类，并为有注解的方法插入一个startTransaction()方法：
class proxy$A{
    A objectA = new A();
    method b(){    //标记2
        startTransaction();
        objectA.b();
    }
 
    method a(){    //标记3
        objectA.a();    //由于a()没有注解，所以不会启动transaction，而是直接调用A的实例的a()方法
    }

如果调用方法a，a没有注解，所以无法启动事务；（spring 在扫描bean的时候会扫描方法上是否包含@Transactional注解，如果包含，spring会为这个bean动态地生成一个子类（即代理类，proxy），代理类是继承原来那个bean的。此时，当这个有注解的方法被调用的时候，实际上是由代理类来调用的，代理类在调用之前就会启动transaction。然而，如果这个有注解的方法是被同一个类中的其他方法调用的，那么该方法的调用并没有通过代理类，而是直接通过原来的那个bean，所以就不会启动transaction，我们看到的现象就是@Transactional注解无效。）
解决办法：
1.在类上加事务注解
2.将需要事务的方法写入另外一个类来调用

事务分为声明式事务和注解类事务
