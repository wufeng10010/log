# 2.27
主要学习了maven的概念，以及在idea中的配置和使用,并成功在idea中配置完成

# 2.28
学习网络编程的相关概念，主要学习了UDP，TCP协议，以及在java代码中实现了相关小demo

# 2.29
在idea中创建springboot项目，并运行了一个web应用小demo，之后学习请求响应相关知识

# 3.4
···java
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
