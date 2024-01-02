# Mybatis的多表模型


## 4.2 多表模型一对一操作

- 一对一模型： 人和身份证，一个人只有一个身份证

  步骤：

  1. 在Card类要有person属性，得到数据好封装。

2. 返回结果集天然不带有封装类的关系。我们要重新声明结果集。
  3. 测试。

  

  **代码实现**

- 步骤一: sql语句准备

~~~sql
create database db2;
use db2;

CREATE TABLE person(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20),
	age INT
);
INSERT INTO person VALUES (NULL,'张三',23);
INSERT INTO person VALUES (NULL,'李四',24);
INSERT INTO person VALUES (NULL,'王五',25);

CREATE TABLE card(
	id INT PRIMARY KEY AUTO_INCREMENT,
	number VARCHAR(30),
	pid INT,
	CONSTRAINT cp_fk FOREIGN KEY (pid) REFERENCES person(id)
);
INSERT INTO card VALUES (NULL,'12345',1);
INSERT INTO card VALUES (NULL,'23456',2);
INSERT INTO card VALUES (NULL,'34567',3);

~~~

相关实体类：

```java
public class Person {
    private int id;
    private String name;
    private int age;
    //getter & setter略
}

public class Card {
    private int id;
    private String number;
    private int pid;
    private Person p;
     //getter & setter略
}
```

  //一对一查询
     select * from card c,person p where c.pid = p.id

- 实体关联属性
- 配置结果集
- 测试

- 步骤二:配置文件

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
             PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.it ghd.table01.OneToOneMapper">
    <!--配置字段和实体对象属性的映射关系-->
    <resultMap id="oneToOne" type="card">
        <!--
       column   数据库列名
      property  对象属性名
     -->
        <id column="cid" property="id" />
        <result column="number" property="number" />
        <!--
                 association：配置被包含对象的映射关系
                 property：被包含对象的变量名
                 javaType：被包含对象的数据类型
             -->
        <association property="p" javaType="person">
            <id column="pid" property="id" />
            <result column="name" property="name" />
            <result column="age" property="age" />
        </association>
    </resultMap>
    <select id="selectAll" resultMap="oneToOne">
        SELECT c.id cid,number,pid,NAME,age FROM card c,person p WHERE c.pid=p.id
    </select>
</mapper>
~~~

   * ​	步骤三：测试类

~~~java
 @Test
    public void selectAll() throws Exception{
        //1.加载核心配置文件
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
        //2.获取SqlSession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        //3.通过工厂对象获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        //4.获取OneToOneMapper接口的实现类对象
        OneToOneMapper mapper = sqlSession.getMapper(OneToOneMapper.class);
        //5.调用实现类的方法，接收结果
        List<Card> list = mapper.selectAll();
        //6.处理结果
        for (Card c : list) {
            System.out.println(c);
        }
        //7.释放资源
        sqlSession.close();
     is.close();
    }
~~~

 **一对一配置总结:**
   <resultMap>：配置字段和对象属性的映射关系标签。
       id 属性：唯一标识
       type 属性：实体对象类型
       <id>：配置主键映射关系标签。
   <result>：配置非主键映射关系标签。
       column 属性：表中字段名称     (有别名就是别名的名字)
       property 属性： 实体对象变量名称
   <association>：配置被包含对象的映射关系标签。
       property 属性：被包含对象的变量名
       javaType 属性：被包含对象的数据类型





## 4.3 多表模型一对多操作

1. 一对多模型： 一对多模型：班级和学生，一个班级可以有多个学生。  

   ![image-20210330154023544](D:\Wechat\WeChat Files\wxid_d97ib7s1lsxa22\FileStorage\File\2023-09\img\image-20210330154023544.png)  

**准备资料和步骤：**

1. 数据创建两张表：student  classes 。创建两个bean实体类。
2. 在班级表中设置学生列表属性。
3. 映射配置文件配置结果集。
4. 测试。

**代码实现**

1. sql语句准备

```sql
CREATE TABLE classes(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20)
);
INSERT INTO classes VALUES (NULL,' 一班');
INSERT INTO classes VALUES (NULL,' 二班');


CREATE TABLE student(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(30),
	age INT,
	cid INT,
	CONSTRAINT cs_fk FOREIGN KEY (cid) REFERENCES classes(id)
);
INSERT INTO student VALUES (NULL,'张三',23,1);
INSERT INTO student VALUES (NULL,'李四',24,1);
INSERT INTO student VALUES (NULL,'王五',25,2);
INSERT INTO student VALUES (NULL,'赵六',26,2);


--1对多查询方法
SELECT c.id cid,c.name cname,s.id sid,s.name sname,s.age sage FROM classes c,student s WHERE c.id=s.cid
```

2. 创建bean文件。

```java
//班级
public class Classes {
    private Integer id;
    private String  name;
    private List<Student> students;
    //getter/setter略
}
//学生
public class Student {
    private Integer id;
    private String name;
    private Integer age;
    private Integer cid;
    //getter/setter略
}
```



3. 创建一个一对多mapper文件，创建一个同名mapper.xml映射文件。

   ```java
   public interface OneToManyMapper {
       //查询全部
       public abstract List<Classes> selectAll();
   }
   ```

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.itgaohe.mapper.ClassesMapper">
   
       <resultMap id="oneToMany" type="com.itgaohe.pojo.Classes">
           <id property="id" column="cid"/>
           <result property="name" column="cname"/>
   
           <!--
               collection :集合映射标签
                   property：实体类中属性名
                   ofType：集合属性的每一个元素的类型
           -->
           <collection property="students" ofType="com.itgaohe.pojo.Student">
               <id property="id" column="sid"/>
               <result property="name" column="sname"/>
               <result property="age" column="sage"/>
           </collection>
   
       </resultMap>
   
       <select id="selectAll" resultMap="oneToMany">
   
       SELECT c.id cid,c.name cname,s.id sid,s.name sname,s.age sage
       FROM classes c,student s
       WHERE c.id=s.cid
   
       </select>
   </mapper>
   ```

   </mapper>

4. 在主配置文件中进行别名、mapper配置。

   ![image-20210329153710595](D:\Wechat\WeChat Files\wxid_d97ib7s1lsxa22\FileStorage\File\2023-09\img\image-20210329153710595.png)



5. 测试类

```java
    @Test
    public void selectAll() throws Exception{
        //1.加载核心配置文件
        InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
        //2.获取SqlSession工厂对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        //3.通过工厂对象获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession(true);
        //4.获取OneToManyMapper接口的实现类对象
        OneToManyMapper mapper = sqlSession.getMapper(OneToManyMapper.class);
        //5.调用实现类的方法，接收结果
        List<Classes> classes = mapper.selectAll();
        //6.处理结果
        for (Classes cls : classes) {
            System.out.println(cls.getId() + "," + cls.getName());
            List<Student> students = cls.getStudents();
            for (Student student : students) {
                System.out.println("\t" + student);
            }
        }
        //7.释放资源
        sqlSession.close();
        is.close();
    }
```

运行结果：

**运行结果为如下：**

```
1，高合一班
	Student{sid=1,name='zhangsna',age=15}
	Student{sid=3,name='赵四',age=34}
	Student{sid=5,name='小二',age=23}
	Student{sid=7,name='小七',age=32}
2，高合二班
	Student{sid=2,name='王五',age=32}
	Student{sid=4,name='张三',age=43}
	Student{sid=6,name='周七2',age=37}
```



3.一对多配置文件总结：

~~~xml-dtd
<resultMap>：配置字段和对象属性的映射关系标签。
    id 属性：唯一标识
    type 属性：实体对象类型
<id>：配置主键映射关系标签。
<result>：配置非主键映射关系标签。
    column 属性：表中字段名称
    property 属性： 实体对象变量名称
<collection>：配置被包含集合对象的映射关系标签。
    property 属性：被包含集合对象的变量名
    ofType 属性：集合中保存的对象数据类型
~~~



> tips: 常见问题：属性值问题
>
> 数据库名称、bean、映射配置名称在拷贝代码的时候容易不一致



## 4.4 多表模型多对多操作

1. 多对多模型：学生和课程，一个学生可以选择多门课程、一个课程也可以被多个学生所选择。

   ![image-20210330160220739](D:\Wechat\WeChat Files\wxid_d97ib7s1lsxa22\FileStorage\File\2023-09\img\image-20210330160220739.png)

   **准备资料和步骤：**

   1. 数据创建三张表：student\course\stu_cr。

   2. 创建两个bean文件。

   3. 创建一个一对多mapper文件，创建一个同名mapper.xml映射文件。

   4. 在主配置文件中进行别名、mapper配置。

   5. 开始编写mapper代码及相关功能功能。测试。

      ****

2. 代码实现

   - 步骤一: sql语句准备

     ```sql
     CREATE TABLE course(
     	id INT PRIMARY KEY AUTO_INCREMENT,
     	NAME VARCHAR(20)
     );
     INSERT INTO course VALUES (NULL,'语文');
     INSERT INTO course VALUES (NULL,'数学');
     
     
     CREATE TABLE stu_cr(
     	id INT PRIMARY KEY AUTO_INCREMENT,
     	sid INT,
     	cid INT,
     	CONSTRAINT sc_fk1 FOREIGN KEY (sid) REFERENCES student(id),
     	CONSTRAINT sc_fk2 FOREIGN KEY (cid) REFERENCES course(id)
     );
     INSERT INTO stu_cr VALUES (NULL,1,1);
     INSERT INTO stu_cr VALUES (NULL,1,2);
     INSERT INTO stu_cr VALUES (NULL,2,1);
     INSERT INTO stu_cr VALUES (NULL,2,2);
     
     
     SELECT s.id sid,s.`NAME` sname,c.`NAME` cname FROM student s,stu_cr cr,course c WHERE s.`id` = cr.sid AND cr.cid = c.id
     
     ```

   - 接口方法

   - 步骤二:配置文件

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <!DOCTYPE mapper
             PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     <mapper namespace="com.itgaohe.mapper.StudentMapper">
         
         <resultMap id="manyToMany" type="com.itgaohe.pojo.Student">
         <id column="sid" property="id"/>
         <result column="sname" property="name"/>
         <result column="sage" property="age"/>
     
         <collection property="courses" ofType="com.itgaohe.pojo.Course">
             <id column="cid" property="id"/>
             <result column="cname" property="name"/>
         </collection>
     </resultMap>
         
     <!--    public List<StudentMapper> selectAll();-->
         <select id="selectAll" resultMap="manyToMany">
          SELECT sc.sid,s.name sname,s.age sage,sc.cid,c.name cname
             FROM student s,course c,stu_cr sc
          WHERE sc.sid=s.id
             AND sc.cid=c.id
     
         </select>
     </mapper>
     ```

   - 步骤三：测试类

     ```java
      @Test
         public void selectAll() throws Exception{
             //1.加载核心配置文件
             InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
             //2.获取SqlSession工厂对象
             SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
             //3.通过工厂对象获取SqlSession对象
             SqlSession sqlSession = sqlSessionFactory.openSession(true);
             //4.获取ManyToManyMapper接口的实现类对象
             ManyToManyMapper mapper = sqlSession.getMapper(ManyToManyMapper.class);
             //5.调用实现类的方法，接收结果
             List<Student> students = mapper.selectAll();
             //6.处理结果
             for (Student student : students) {
                 System.out.println(student.getId() + "," + student.getName() + "," + student.getAge());
                 List<Course> courses = student.getCourses();
                 for (Course cours : courses) {
                     System.out.println("\t" + cours);
                 }
          }
          //7.释放资源
             sqlSession.close();
       is.close();
         }
     ```

   

   运行效果：

   ![image-20210329212531848](D:\Wechat\WeChat Files\wxid_d97ib7s1lsxa22\FileStorage\File\2023-09\img\image-20210329212531848.png)

   

3.多对多配置文件总结：

   ```xml-dtd
   <resultMap>：配置字段和对象属性的映射关系标签。
   	id 属性：唯一标识
   	type 属性：实体对象类型
    <id>：配置主键映射关系标签。
    <result>：配置非主键映射关系标签。
   	column 属性：表中字段名称
   	property 属性： 实体对象变量名称
   <collection>：配置被包含集合对象的映射关系标签。
   	property 属性：被包含集合对象的变量名
   	ofType 属性：集合中保存的对象数据类型
   ```

​    

## 4.5 多表模型操作总结

~~~xml-dtd
 <resultMap>：配置字段和对象属性的映射关系标签。
    id 属性：唯一标识
    type 属性：实体对象类型
<id>：配置主键映射关系标签。
<result>：配置非主键映射关系标签。
	column 属性：表中字段名称
	property 属性： 实体对象变量名称
<association>：配置被包含对象的映射关系标签。
	property 属性：被包含对象的变量名
	javaType 属性：被包含对象的数据类型
<collection>：配置被包含集合对象的映射关系标签。
	property 属性：被包含集合对象的变量名
	ofType 属性：集合中保存的对象数据类型
~~~

# 注解开发略。

1. 注解实现crud。

   ```java
   /*查询*/
   @Select("select * from student where id = #{id} or name  = #{name}")
   List<Student> findbyId(@Param("id") Integer id, @Param("name")String name);
   
   /*新增*/
   @Options(useGeneratedKeys = true,keyColumn = "id" ,keyProperty = "id")
   @Insert("insert into student (id,name,age,cid) values(null,#{name},#{age},#{cid})")
   void add(Student student);
   
   
   /*修改*/
   @Update("update student set name = #{name},age= #{age},cid = #{cid} where id = #{id}")
   void update(Student student);
   
   /*删除*/
   @Delete("delete from student where id  = #{id}")
   void delete(Integer id);
   ```

2. 注解实现1对1。

   ```java
   @Results({
           @Result(property = "id",column = "id"),
           @Result(property = "number",column = "number"),
           @Result(
                   property = "p",
                   javaType = Person.class,
                   column = "pid",
                   one = @One(select = "com.itgaohe.mapper.PersonMapper.findById")
           )
   })
   @Select("select * from card where id = #{id}")
   public Card findById(Integer id);
   ```

   ```java
   @Select("select * from person where id = #{id}")
   public Person findById(Integer id);
   ```

3. 注解实现1对多。

   ```java
   @Results({
          @Result(property = "id",column = "id"),
          @Result(property = "name",column = "name"),
          @Result(
                   property = "students",/*实体对象属性名*/
                   javaType = List.class,/*查询结果封装类型*/
                   column = "id",/*查询参数*/
                   many = @Many(select = "com.itgaohe.mapper.StudentMapper.findByCourseId")
          )
   })
   @Select("select * from course")
   public List<Course> findById(Integer id);
   ```

   ```java
   @Select("SELECT s.id,s.name,s.age,s.cid FROM student s,stu_cr cr " +
               "WHERE cr.cid = #{id}   " +
               "AND cr.sid = s.id")
   List<Student> findByCourseId(Integer id);
   ```



# 分页查询

# 三. 分页插件

以前我们都是findAll()查询。

问题：数据量一旦大了，查询效率会非常慢。页面展示渲染更需要大量时间。

所以在企业应用中展示条形数据，几乎都是分页。

## 3.1 分页插件介绍

* 分页可以将很多条结果进行分页显示。 
* 如果当前在第一页，则没有上一页。如果当前在最后一页，则没有下一页。 
* 需要明确当前是第几页，这一页中显示多少条结果。    
* MyBatis分页插件总结
  1. 在企业级开发中，分页也是一种常见的技术。而目前使用的 MyBatis 是不带分页功能的，如果想实现分页的 功能，需要我们手动编写 LIMIT 语句。但是**不同的数据库实现分页的 SQL 语句也是不同的，所以手写分页 成本较高**。这个时候就可以借助分页插件来帮助我们实现分页功能。 
  2. PageHelper：第三方分页助手。将复杂的分页操作进行封装，从而让分页功能变得非常简单。    

## 3.2 分页插件的使用

MyBatis可以使用第三方的插件来对功能进行扩展，分页助手PageHelper是将分页的复杂操作进行封装，使用简单的方式即可获得分页的相关数据

**开发步骤：**

1. 导入与PageHelper的jar包
2. 在mybatis核心配置文件中配置PageHelper插件
3. 测试分页数据获取



代码演示：

①导入与PageHelper的jar包

```XML
<!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.2.0</version>
</dependency>
```

②在mybatis核心配置文件中配置PageHelper插件

~~~xml
<!-- 注意：分页助手的插件  配置在通用mapper之前 -->
<plugins>
    <!-- 分页助手的插件 -->
    <plugin interceptor="com.github.pagehelper.PageInterceptor">
    </plugin>
</plugins>
~~~

③测试分页数据获取

~~~java
@Test
public void testPageHelper(){
    //设置分页参数
    PageHelper.startPage(1,2);

    List<User> select = userMapper2.select(null);
    for(User user : select){
        System.out.println(user);
    }
}
~~~

## 3.3 分页插件的参数获取

**获得分页相关的其他参数**：

```java
//其他分页的数据
PageInfo<User> pageInfo = new PageInfo<User>(select);
System.out.println("总条数："+pageInfo.getTotal());
System.out.println("总页数："+pageInfo.getPages());
System.out.println("当前页："+pageInfo.getPageNum());
System.out.println("每页显示长度："+pageInfo.getPageSize());
System.out.println("是否第一页："+pageInfo.isIsFirstPage());
System.out.println("是否最后一页："+pageInfo.isIsLastPage());

```

3.2-3.3演示：

```java
@Test
public void selectPaging() throws Exception{
    //1.加载核心配置文件
    InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
    //2.获取SqlSession工厂对象
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
    //3.通过工厂对象获取SqlSession对象
    SqlSession sqlSession = sqlSessionFactory.openSession(true);
    //4.获取StudentMapper接口的实现类对象
    StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    //通过分页助手来实现分页功能
    // 第一页：显示3条数据
    //        PageHelper.startPage(1,3);
    // 第三页：显示3条数据
    PageHelper.startPage(3,3);
    //5.调用实现类的方法，接收结果
    List<Student> list = mapper.selectAll();
    //6.处理结果
    for (Student student : list) {
        System.out.println(student);
    }
    //        //获取分页相关参数
    PageInfo<Student> info = new PageInfo<>(list);
    System.out.println("总条数：" + info.getTotal());
    System.out.println("总页数：" + info.getPages());
    System.out.println("当前页：" + info.getPageNum());
    System.out.println("每页显示条数：" + info.getPageSize());
    System.out.println("上一页：" + info.getPrePage());
    System.out.println("下一页：" + info.getNextPage());
    System.out.println("是否是第一页：" + info.isIsFirstPage());
    System.out.println("是否是最后一页：" + info.isIsLastPage());
    //7.释放资源
    sqlSession.close();
    is.close();
}
```

演示效果；

![image-20210329140304948](D:\Wechat\WeChat Files\wxid_d97ib7s1lsxa22\FileStorage\File\2023-09\img\image-20210329140304948.png)



## 3.4  分页插件知识小结

​    分页：可以将很多条结果进行分页显示。 

* 分页插件 jar 包： pagehelper-5.1.10.jar jsqlparser-3.1.jar 

* <plugins>：集成插件标签。 

* 分页助手相关 API 

  ​    1.PageHelper：分页助手功能类。

  2. startPage()：设置分页参数 
  3. PageInfo：分页相关参数功能类。 
  4. getTotal()：获取总条数 
  5. getPages()：获取总页数
  6. getPageNum()：获取当前页
  7. getPageSize()：获取每页显示条数
  8. getPrePage()：获取上一页 
  9. getNextPage()：获取下一页 
  10. isIsFirstPage()：获取是否是第一页 
  11. isIsLastPage()：获取是否是最后一页    

# 二级缓存

一级缓存:

​	一级缓存基于sqlSession默认开启,在操作数据库时需要构造SqlSession对象，在对象中有一个HashMap用于存储缓存数据。不同的SqlSession之间的缓存数据区域是互相不影响的。

- 如果SqlSession执行了DML操作（增删改），并且提交到数据库，MyBatis则会清空SqlSession中的一级缓存，这样做的目的是为了保证缓存中存储的是最新的信息，避免出现脏读现象。

- sqlSession关闭。

二级缓存

  mapper级别的缓存，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession去操作数据库得到数据会存在二级缓存区域，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。

- 手动开启
- 面向所有的sqlSession
- 性能提升（弊大于利）

的区别

二级缓存需要手动打开
打开总开关：
在核心配置文件SqlMapConfig.xml中加入

< !–注意顺序–>
< setting name=“cacheEnabled” value=“true”/>

```java
@Test
public void findAll() {
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    StuMapper mapper = sqlSession.getMapper(StuMapper.class);
    long start = System.currentTimeMillis();
    List<Stu> all = mapper.findAll();
    long end = System.currentTimeMillis();
    System.out.println("执行效率："+(end-start));//time=2000

    long start2 = System.currentTimeMillis();
    List<Stu> all2 = mapper.findAll();
    long end2 = System.currentTimeMillis();
    System.out.println("执行效率2："+(end2-start2));//time=0

    sqlSession.close();
}
```


