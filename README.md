# SimonNote
Node For Learn

1. @RestController 和@Controller区别

   @RestController 相当于@Controller+@ResponseBody 只能返回json数据,配置的视图解析器 InternalResourceViewResolver不起作用;
   
   
   @Controller可以返回多种数据，包括jsp和html等,如果需要返回jsp,html等需要配合视图解析器InternalResourceViewResolver；如果需要返回json,xlm或者自定义mediaType等到页面，需要加上@ReponseBody注解。
