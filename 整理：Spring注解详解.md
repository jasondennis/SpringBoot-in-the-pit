## 整理：Spring注解详解

+ **@Component**

  ---

  最朴素的注释，代表被注释的类是一个组件(不带其他含义的组件)，暨在Spring中被定义为一个Bean。

  例如：

  `@Component（“student”）`或`@Componet` 

  实际反映到xml配置文件中，相当于:

  ```xml
  <bean id="student" class="com.yellowdoge.hxl.model.Student">
      <property name="name" value="hxl"></property>
      <property name="age" value="19"></property>
  </bean>
  ```

  **解释**：`@Componet` 的参数 `value` 默认为 `""`, value参数指定了该类在 Spring 容器中的id, 在不传入此参数值时，则这个类在Spring 容器中的id为默认id(默认id为类名的首字母小写，`e.g:  Student -> student, BiBaBo -> biBaBo`)。

     

+ **@Repository**

  ---

  Spring 2.5 后，这个注释主要作为一个特化的 `@Component` 。应用于数据持久层(Dao)，常常代替`@Component`  注释。

  `@Repository` 能将所标注的类中抛出的数据访问异常封装为Spring 的数据访问异常类型，理解上是`@Component`的一种延伸。

   

+ **@Service**

  ---

  表示被注释的类是“Service”类, 用于Service层。相当于`@Component`, 别的没啥卵用。

  `@Service` 参数 Value 的使用方法和原理与 `@Component`一致。

​    

+ **@Controller**

  ---

  表示被注释的类是Controller类。**同样相当于**`@Component`, 别的没得什么卵用，主要的作用就是方便的给各个类做个层次上的分类。

  `@Controller`参数 Value 的使用方法和原理与 `@Component`一致。

  `@Controller`常常与`@RequestMapping` 结合使用。Spring 将会检测被`@Controller`注释的方法是否使用了`@RequestMapping` 注解。

  `@Controller`只是定义了一个控制器类，而使用`@RequestMapping` 注解的方法才是真正处理请求的处理器。

​     

+ **@RequestMapping**

  ---

  **作用**：使用自动的方法将Web请求映射到负责处理的方法类中。简而言之，根据Web请求调起相应的处理方法来处理请求。

  **简单用法**：`@RequestMapping(value:"student")`  访问路径：`http://yourip:port/student`

  **进阶**：

  1. **在控制器类的级别和/或其中的方法的级别上使用**

     下为同时在类和方法上应用了 @RequestMapping 注解的示例：

     ```java
     @RestController
     @RequestMapping("/home ")
     public class IndexController {
         @RequestMapping("/ ")
         String get() {
             //mapped to hostname:port/home/
             return "Hello from get ";
         }
         @RequestMapping("/index ")
         String index() {
             //mapped to hostname:port/home/index/
             return "Hello from index ";
         }
     }
     ```

     **解释：**

     如上图所示：`http://yourip:port/home`的请求会由 get() 方法来处理，而到 `http://yourip:port/home/index` 的请求会由 index() 来处理。

  2. `@RequestMapping `**来处理多个 URI**

     `@RequestMapping`允许将多个请求映射到一个方法上去，只需要添加一个带有请求路径值列表即可，如下：

     ```java
     @RestController
     @RequestMapping("/home ")
     public class IndexController {
     
         @RequestMapping(value = {
             " ",
             "/page ",
             "page* ",
             "view/*,**/msg "
         })
         String indexMultipleMapping() {
             return "Hello from index multiple mapping. ";
         }
     }
     ```

     上述代码使得：

     `http://yourip:port/home`

     `http://yourip:port/home/page`

     `http://yourip:port/home/pagebibabo`

     `http://yourip:port/home/view`

     `http://yourip:port/home/view/bibabo`

     均将路由至`indexMultipleMapping()`处理Web请求。

     

  3. Question：@RequestParam 和 @RequestMapping结合使用，与@GetMapping有什么区别呢？？？？

     **实测：**

     使用`@RequestMapping `与`@RequestParam`的代码块

     ```java
     @RequestMapping("/selectOne")
     public Student selectOne(@RequestParam("id") Integer id){
         return this.studentService.queryById(id);
     }
     ```

     与如下使用`@GetMapping` 的代码块具有一致的功能

     ```java
     @GetMapping("selectOne")
     public Student selectOne(Integer id){
         return this.studentService.queryById(id);
     }
     ```

     获得的效果为，同样的url：`http://localhost:8014/student/selectOne?id=2`可以同样的对接这两个接口。

     **个人理解：**

     `@RequestMapping` 可以视为@PostMapping、@PatchMapping、@PutMapping和@DeleteMapping

        

+ **@GetMapping**

  ---

  `@GetMapping`  注释用于将GET类型的请求映射到相应`GetMapping`注释的特定处理方法。

  效果相同于：`@RequestMapping(method=RequestMethod.Get)`。

  `@GetMapping` 是 `@RequestMapping` 的一种简化，可以理解为`@GetMapping `继承了`@RequestMapping` 

  

  

  