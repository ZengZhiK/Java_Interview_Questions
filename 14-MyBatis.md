## 1.概念/使用方法向的问题

### 1.1 什么是Mybatis?

（1）Mybatis是一个半ORM框架，它内部封装了JDBC，开发时只需要关注SQL语句本身，不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。

（2）MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

（3）通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。

### 1.2 为什么说Mybatis是半ORM框架?与Hibernate有哪些不同?

ORM是对象和关系之间的映射，包括对象->关系和关系->对象两方面。Hibernate是个完整的ORM框架，而MyBatis只完成了关系->对象，准确地说MyBatis是SQL映射框架而不是ORM框架，因为其仅有字段映射，对象数据以及对象实际关系仍然需要通过手写SQL来实现和管理。

（1）Hibernate为完整的ORM框架，Mybatis为半ORM框架。

（2）Mybatis程序员直接编写原生sql，可严格控制sql执行性能，灵活度高，适用于对关系数据模型要求不高的软件开发，例如互联网软件、企业运营类软件等；Hibernate只能通过编写hql实现数据库查询（hql好难用哦）。

（3）Hibernate对象/关系映射能力强，数据库无关性好，适用于对关系模型要求高的软件； Mybatis的数据库无关性较差，如果需要实现支持多种数据库的软件则需要自定义多套sql映射文件。

### 1.3 Mybaits的优点?

（1）基于SQL语句编程，不会对应用程序或者数据库的现有设计造成任何影响，解除sql与程序代码的耦合，便于统一管理；提供XML标签，支持编写动态SQL语句，重用性高。

（2）与JDBC相比，减少了50%以上的代码量，消除了JDBC大量冗余的代码，不需要手动开关连接；

（3）很好的与各种数据库兼容（因为MyBatis使用JDBC来连接数据库，所以只要JDBC支持的数据库MyBatis都支持）。

（4）能够与Spring很好的集成；

（5）提供映射标签，支持对象与数据库的ORM字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

### 1.4 MyBatis框架的缺点?

（1）SQL语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写SQL语句的功底有一定要求。

（2）SQL语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。

### 1.5 #{}和${}的区别?

（1）${}是properties文件中的变量占位符，它可以用于标签属性值和sql内部，属于静态文本替换。

（2）#{}是sql的参数占位符，Mybatis会将sql中的#{}替换为?号，在sql执行前会使用PreparedStatement的参数设置方法，按序给sql的?号占位符设置参数值。使用#{}可以有效的防止 SQL 注入，提高系统安全性。

```sql
${param}传递的参数会被当成sql语句中的一部分，举例：
order by ${param}，则解析成的sql为：
order by    id
 
#{parm}传入的数据都当成一个字符串，会对自动传入的数据加一个双引号，举例：
select * from table where name = #{param}，则解析成的sql为：
select * from table where name =   "id"
```

### 1.6 怎么解决实体类中的属性名和表中的字段名不一样的问题?

（1）通过在查询的sql语句中定义字段名的别名，使字段名的别名和实体类的属性名一致

```sql 
<select id="selectUserById" parameterType="java.lang.Integer" resultetype="com.en.entity.user">
       select user_id as id, user_no as no from test where user_id = #{id};
</select>
```

（2）Mybatis和hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句。

```xml
   <resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
        <!–用id标签来映射主键字段–>
        <id property="id" column="user_id">
        <!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–>
        <result property="no" column="user_no"/>
   </reslutMap>
```

### 1.7 如何在mapper中传递多个参数?

（1）使用 @param 注解：

```
user selectUser(@param("username") string username,@param("password") string password);
```

（2）Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同；

```
    Map<String, Object> map = new HashMap();
    map.put("start", start);
    map.put("end", end);
    sqlSession.selectList("student.selectUser", map);
```

### 1.8 MyBatis的接口绑定有哪些实现方式？

（1）Mapper接口方法名和mapper.xml中定义的每个sql的id相同；  
（2）Mapper接口方法的输入参数类型和mapper.xml中定义的每个sql 的parameterType的类型相同；  
（3）Mapper接口方法的输出参数类型和mapper.xml中定义的每个sql的resultType的类型相同；  
（4）Mapper.xml文件中的namespace即是mapper接口的类路径;















