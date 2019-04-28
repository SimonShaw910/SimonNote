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
      
         
