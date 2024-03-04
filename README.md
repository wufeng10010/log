# 2.27
主要学习了maven的概念，以及在idea中的配置和使用,并成功在idea中配置完成

# 2.28
学习网络编程的相关概念，主要学习了UDP，TCP协议，以及在java代码中实现了相关小demo

# 2.29
在idea中创建springboot项目，并运行了一个web应用小demo，之后学习请求响应相关知识

# 3.4
使用分层解耦的方式来搭建一个springboot的小demo
了解了IOC（控制反转）和DI（依赖注入）的设计思想

    * IOC:将设计好的对象交给容器控制,由容器帮我们查找及注入依赖对象
    * DI:由容器动态的将某个依赖关系注入到组件之中   
```java
@RestController
public class EmpController {
    @Autowired //运行时，IOC容器会提供该类型的bean对象，并赋值给该变量--依赖注入
    private EmpService empService;

    @RequestMapping("/listEmp")
    public Result listEmp() {
        List<Emp> empList = empService.listEmp();

//      3.响应数据
        return Result.success(empList);
```
