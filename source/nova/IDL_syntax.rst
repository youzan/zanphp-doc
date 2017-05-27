================
Thrift IDL 语法
================

1.类型
-------

- bool: 布尔值 (true or false) 
- byte: 有符号字节 
- i16: 16位有符号整型 
- i32: 32位有符号整型 
- i64: 64位有符号整型 
- double: 64位浮点型 
- string: Encoding agnostic text or binary string 
- binary: Blob (byte array) a sequence of unencoded bytes，这是string类型的一种变形，主要是为.java使用

2.结构体（Struct）
------------------

thrift中struct是定义为一种对象，和面向对象语言的class差不多. 但是struct有以下一些约束：

1. struct不能继承，但是可以嵌套，不能嵌套自己。
2. 其成员都是有明确类型
3. 成员是被正整数编号过的，其中的编号使不能重复的，这个是为了在传输过程中编码使用。
4. 成员分割符可以是逗号（,）或是分号（;），而且可以混用，但是为了清晰期间，建议在定义中只使用一种。
5. 字段会有optional和required之分和protobuf一样，但是如果不指定则为无类型—可以不填充该值，但是在序列化传输的时候也会序列化进去，optional是不填充则部序列化，required是必须填充也必须序列化。
6. 每个字段可以设置默认值
7. 同一文件可以定义多个struct，也可以定义在不同的文件，进行include引入

数字标签作用非常大，但是随着项目开发的不断发展，也许字段会有变化，但是建议不要轻易修改这些数字标签，修改之后如果没有同步客户端和服务器端会让一方解析出问题。

.. code:: thrift

         struct Report
         {
           1: required string msg, //序列化中，必须有该字段
           2: optional i32 type = 0; //序列化中可以没有该字段
           3: i32 time //对于基本类型，会序列化到字节数组中，解析的时候不会校验。对于对象类型，如果不为空，则序列化到字节数组中
         }

规范的struct定义中的每个域均会使用required或者optional关键字进行标识。如果required标识的域没有该字段，Thrift将给予提示；如果optional标识的域没有赋值，该域将不会被序列化传输；如果某个optional标识域有缺省值而用户没有重新赋值，则该域的值一直为缺省值；如果某个optional标识域有缺省值或者用户已经重新赋值，而不设置它的\_\_isset为true，也不会被序列化传输。

3.容器（Containers）
--------------------

Thrift容器与目前流行编程语言的容器类型相对应，有3种可用容器类型：

1. list: 元素类型为t的有序表，容许元素重复。对应c++的vector，Java的ArrayList或者其他语言的数组
2. set: 元素类型为t的无序表，不容许元素重复。对应c++中的set，Java中的HashSet,python中的set，php中没有set，则转换为list类型了
3. map: 键类型为t，值类型为t的kv对，键不容许重复。对用c++中的map, Java的HashMap, PHP 对应 array, Python/Ruby 的dictionary。

容器中元素类型可以是除了service外的任何合法Thrift类型（包括结构体和异常）。为了最大的兼容性，map的key最好是thrift的基本类型，有些语言不支持复杂类型的key，JSON协议只支
持那些基本类型的key。

容器都是同构容器，不失异构容器。

例子:

.. code:: thrift

    struct Test {
      1: map<Number, UserId> user_map,
      2: set<Number> num_sets,
      3: list<Stusers> users
    }

4.枚举（enum）
--------------

很多语言都有枚举，意义都一样。比如，当定义一个消息类型时，它只能是预定义的值列表中的一个，可以用枚举实现。说明：

1. 编译器默认从0开始赋值 
2. 可以赋予某个常量某个整数 
3. 允许常量是十六进制整数 
4. 末尾没有分号 
5. 给常量赋缺省值时，使用常量的全称

注意，不同于protocal buffer，thrift不支持枚举类嵌套，枚举常量必须是32位的正整数

.. code:: thrift

    enum EnOpType {
      CMD_OK = 0, // (0) 　　
      CMD_NOT_EXIT = 2000, // (2000) 
      CMD_EXIT = 2001, // (2001)  　　
      CMD_ADD = 2002 // (2002)
    }

::

    struct StUser {
      1: required i32 userId;
      2: required string userName;
      3: optional EnOpType cmd_code = EnOpType.CMD_OK; 
      4: optional string language = “english”
    }

5.常量定义和类型定义
--------------------

Thrift允许定义跨语言使用的常量，复杂的类型和结构体可使用JSON形式表示。

.. code:: thrift

    const i32 INT_CONST = 1234; 
    const EnOpType myEnOpType = EnOpType.CMD_EXIT; //2001

6.异常（Exceptions）
--------------------

.. code:: thrift

    Thrift结构体在概念上类似于（similar to）C语言结构体类型—将相关属性封装在一起的简便方式。Thrift结构体将会被转换成面向对象语言的类。

异常在语法和功能上类似于结构体，差别是异常使用关键字exception，而且异常是继承每种语言的基础异常类。

.. code:: thrift

    exception UserException {
      1: i32 errorCode,
      2: string message,
      3: StUser userinfo
    }

7.服务（Services）
------------------

　　服务的定义方法在语义(semantically 上等同于面向对象语言中的接口。Thrift编译器会产生执行这些接口的client和server
stub。
Thrift编译器会根据选择的目标语言为server产生服务接口代码，为client产生stubs。

.. code:: thrift

    service SeTest {     
        void ping() throws (1:UserException e),     
        bool postTweet(1: StUser user);
        StUser searchTweets(1:string name);
    }

Service的约束
	
	1. 不支持多态 
	2. 方法无法重载

8.名字空间（Namespace）
-----------------------

.. code:: thrift

    Thrift中的命名空间类似于C++中的namespace和Java中的package，它们提供了一种组织（隔离）代码的简便方式。名字空间也可以用于解决类型定义中的名字冲突。 

由于每种语言均有自己的命名空间定义方式（如python中有module）,
thrift允许开发者针对特定语言定义namespace：

.. code:: thrift

    namespace cpp com.example.test
    namespace java com.example.test 
    namespace php com.example.test

注意：现在统一规定为

.. code:: thrift

    namespace nova com.youzan.xxx

9.注释（Comment）
-----------------

Thrift支持C多行风格和Java/C++单行风格。

.. code:: thrift

    /*
     * This is a multi-line comment.
     * Just like in C.
     */
    // C++/Java style single-line comments work just as well.

10.依赖(Includes)
-----------------

便于管理、重用和提高模块性/组织性，我们常常分割Thrift定义在不同的文件中。包含文件搜索方式与c++一样。Thrift允许文件包含其它thrift文件，用户需要使用thrift文件名作为前缀访问被包含的对象，如：

.. code:: thrift

    include "test.thrift" 
    ...
    struct StSearchResult {
     1: in32 uid; 
     ...
    }

thrift文件名要用双引号包含，末尾没有逗号或者分号

11.示例
-------

.. code:: thrift

    Teacher.thrift
    namespace nova com.youzan.nova.demo

    struct Teacher{
        1:string name,
    }

Student.thrift

.. code:: thrift

    namespace php kdt.api.nova.demo
    namespace java com.youzan.nova.demo

    include "Teacher.thrift"
    include "StudentEnum.thrift"

    const i32 INT_CONST = 1234;//定义常量
    struct Student{
     1:required bool health, //布尔型
     2:optional byte sex, //字节型
     3:i16 age, //short型
     4:i32 height = INT_CONST, //引用常量
     5:i64 admissionDate, //long型
     6:double dbValue,//double型
     7:string address,//字符串型
     8:Teacher.Teacher teacher,//复合型
     9:list<Teacher.Teacher> teachers,//list
     10:set<Teacher.Teacher> sTeachers,//set
     11:map<double,Teacher.Teacher> mTeachers,//map
     12:StudentEnum.StudentEnum type = StudentEnum.StudentEnum.STUDENT_TYPE_1//枚举
    }

StudentEnum.thrift

.. code:: thrift

    namespace php kdt.api.nova.demo
    namespace java com.youzan.nova.demo

    enum StudentEnum {
      STUDENT_TYPE_1 = 1,
      STUDENT_TYPE_2 = 2,
      STUDENT_TYPE_3 = 3,
      STUDENT_TYPE_4 = 4
    }

StudentException.thrift

.. code:: thrift

    namespace php kdt.api.nova.demo
    namespace java com.youzan.nova.demo

    include "Student.thrift"

    exception StudentException {
      1: i32 errorCode,
      2: string message,
      3: Student.Student student
    }

StudentService.thrift

.. code:: thrift

    namespace php kdt.api.nova.demo
    namespace java com.youzan.nova.demo

    include "Student.thrift"
    include "StudentException.thrift"
    service StudentService {
      list<Student.Student> listStudent() throws (1:StudentException.StudentException e),
      bool deleteStident(1:i32 id);
      void addStudent(1:Student.Student student);
      Student.Student getStudent(1:i32 id,2:string type);
    }


附录：

- 有赞nova协议Intellij IDEA Thrift插件 https://github.com/youzan/intellij-thrift
- 官方文档：http://thrift.apache.org/docs/idl
- 官方示例：https://git-wip-us.apache.org/repos/asf?p=thrift.git;a=blob\_plain;f=test/ThriftTest.thrift;hb=HEAD
