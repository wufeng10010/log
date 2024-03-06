# 2.27
主要学习了maven的概念，以及在idea中的配置和使用,并成功在idea中配置完成

# 2.28
学习网络编程的相关概念，主要学习了UDP，TCP协议，以及在java代码中实现了相关小demo

# 2.29
在idea中创建springboot项目，并运行了一个web应用小demo，之后学习请求响应相关知识

# 3.4
使用分层解耦的方式来搭建一个springboot的小demo，了解了IOC（控制反转）和DI（依赖注入）的设计思想

    * IOC:将设计好的对象交给容器控制,由容器帮我们查找及注入依赖对象
    * DI:由容器动态的将某个依赖关系注入到组件之中   

要把某个对象交给IOC容器管理，需要在对应的类加上如下注解之一：

|注解|说明|位置|
|:---|:---|:---|
|@Component|声明bean的基础类|不属于一下三类时，用此注解|
|@Controller|@Component的衍生注解|标注在控制器类上|
|@Service|@Component的衍生注解|标注在业务类上|
|@Repository|@Component的衍生注解|标注在数据访问类上|

注意@SpringBootApplication具有包扫描作用，默认扫描当前包及其子包。

依赖注入的注解

* @Autowired:默认按类型自动注入，这是Spring框架提供的注解，与@Resource有所区分
* 如果同类型的bean有多个：
  * 使用@Primary注解，放在类上方
  * @Autowired + @Qualifier("bean的名称")
  * @Resource(name = "bean的名称")，这是JDK提供的注解，按名称注入

controller层：控制层，接收前端发送的请求，对请求进行处理，并响应数据。
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
service层：业务逻辑层，处理具体的业务逻辑。
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
dao层：数据访问层，负责数据的访问操作
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
启动spring后在页面的展示效果
<img width="1245" alt="image" src="https://github.com/wufeng10010/log/assets/131955051/73e6c656-3fdc-4b34-a62b-978b22e1dd6d">

# 3.6 mybatis基础操作(预编译SQL)
预编译是指把要执行的sql语句先进行一个解析,解析语法以及确定查询范围还有查找的返回结果类型，就是确定了查询的方式，把命令和参数进行了分离，使用预编译的sql语句来进行查询直接进行执行计划，只需要替换掉参数部分.

### 好处：
* 1.性能更高，通过预编译，可以在执行相似的sql指令时减少对sql的编译，如下图
<img width="1013" alt="image" src="https://github.com/wufeng10010/log/assets/131955051/90e9a2c1-1e61-42c3-9546-ef0089695679">
* 2.更安全，主要是防止SQL注入
