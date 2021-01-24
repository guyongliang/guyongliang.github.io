## Spring

1.三种装配机制

- xml

  - ```
    <beans>
    	<bean id="" class="">
    		<constructor-arg ref="" />
    		<constructor-arg value="">
    			<list>
    				<ref bean="" />
    				<ref bean="" />
    				<ref bean="" />
    			</list>
             </>
    		<constructor-arg ref="" />
    	</bean>
    <beans>
    可以使用命名空间来代替标签
    - c命名空间代替<constructor-arg>
    - p命名空间代替<property>
    - util命名空间代替list、map、set等
    ```

  - 

- java config

  - @Bean

- 隐式的bean发现机制和自动装配

  - 两方面：组件扫描+自动装配
  - @ComponentScan -- <context:component-scan>
  - @Component
  - @Autowired

2.profile

- 用在类或‘Setter’方法上 @Profile

- xml中：<beans profile="">标签中profile属性指定

- 激活：
  - @ActiveProfiles("")用于测试类
  - spring.profiles.active
    - spring.profiles.default < spring.profiles.active
    - 注意profiles是复数：可以指定多个

3.条件化的bean

- @Condition

  - 用在@Bean上，例如

  - ```
    @Bean
    @Condition(Xxx.class)
    public A b() {
    	return new A();
    }
    ```

    Xxx：需要实现Condition接口，重写matches方法，返回true才创建bean，否则忽略该bean

  - @Profile注解的实现借助了@Condition，判定是否profile处于激活状态

4.歧义性

解决：

- @Primary("")：标识一个首选bean
- @Qualifier("")：指定注入的bean
- 还可以自定义注解

5.bean的作用域

- @Scope + @Bean或@Component
  - singleton：单例（默认）
  - prototype：原型，每次注入或者通过Spring应用上下文获取的时候，都会创建一个新的bean实例
  - session：会话，web应用中，每个会话一个bean实例
  - request：web应用中，每个请求一个bean实例

- xml中 <bean scope="">

- 请求和会话要借助域代理的方式进行注入，由域代理将多个请求或会话bean注入到单例的bean，对于接口要指定proxyMode=INTERFACES，对于类要指定proxyMode=TARGET_CLASS，这样保证了每次处理业务逻辑的那个bean就是当前的那个用户对应的bean。

6.运行时求值的方式

- 属性占位符$
  - @PropertySource("路径")
    - @PropertySource("classpath:/com/xxx/xxx/aaa.properties")
  - 在xml中 <bean c:_title="${aa.bb}" />
  - 在java代码中：@Value("${aa.bb}") String title

- SpEL：${}
  - 可以使用bean的ID来引用bean
  - 调用方法和访问对象的属性
  - 算数、关系、逻辑运算
  - 正则表达式匹配
  - 集合操作

7.aop

- @Aspect
- 五种通知
  - 前置：@Before方法调用之前
  - 后置：@After 方法返回或者抛异常
  - 返回：@AfterReturning方法返回
  - 异常：@AfterThrowing抛异常之后
  - 环绕：@Around可以实现上述四种通知

- AspectJ
  - 比Spring AOP更强大的面向切面实现
  - 由于spring AOP是基于代理实现的，所以无法把通知应用于对象的创建过程，而AspectJ可以

8.