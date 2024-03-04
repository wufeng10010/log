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

controller层
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
service层
```java
//@Primary //想让哪个bean生效
//@Component //将当前类交给IOC容器管理，成为IOC容器中的bean--控制反转
@Service
public class EmpServiceA implements EmpService {

    @Autowired //运行时，IOC容器会提供该类型的bean对象，并赋值给该变量--依赖注入
    private EmpDao empDao;
    @Override
    public List<Emp> listEmp(){
        List<Emp> empList = empDao.listEmp();
        //2.对数据进行转换处理
        empList.stream().forEach(emp -> {
//          2.1处理gender 1.男 2.女
            String gender = emp.getGender();
            if ("1".equals(gender)) {
                emp.setGender("男");
            } else if ("2".equals(gender)) {
                emp.setGender("女");

            }
//          2.1处理job 1.讲师 2.班主任 3.就业指导 4.
            String job = emp.getJob();
            if ("1".equals(job)) {
                emp.setJob("讲师");
            } else if ("2".equals(job)) {
                emp.setJob("班主任");
            } else if ("3".equals(job)) {
                emp.setJob("就业指导");
            }

        });
        return empList;
    }
}

```
dao层
```java
//@Component //将当前类交给IOC容器管理，成为IOC容器中的bean --控制反转
@Repository
public class EmpDaoA implements EmpDao {
    @Override
    public List<Emp> listEmp(){
        //加载并解析emp.xml
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        System.out.println(file);
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);

        return empList;
    }
}
```
