Zan Thrift 代码生成工具
========================


背景
----

因为thrift官方代码生成工具不符合我们nova项目的需求, 我们修改后创建了zan-thrift，用法与官方thrift一致，但是生成的代码格式与官方有一定差别，尤其是php差别很大。

仓库
----

zan-thrift 仓库 https://github.com/youzan/zan-thrift

::

    cd thrift-binary  
    sudo sh install.sh

或者直接使用编译好的二进制文件：

地址：

::

    Linux
    Mac

约定
-----

我们在原生thrift命令的基础上做了以下约定：

   1. yz-thrifts执行目录必须在thrifts目录下面。

      说明:这样做的目的是为了可以方便的查找所有的thrift文件，这样我们的工具就可以一次扫描所有的thrift文件，并进行代码的生成。
      如果不是在thrifts目录下执行yz-thrift命令，会报一条错误信息:
      ``[WARNING] parent path name mast be "thrifts"``.
      并终止代码的生成。

   2. 所有的thrift文件必须放在thrifts目录下。

      说明:原因同第一条。

   3. thrift文件的namespace必须以com.${company}.${module_name}打头，后面跟模块名，然后是你的命名空间。
      比如：\ ``com.youzan.module_name.xxx.xxx`` 这种形式。

   4. thrift文件所在目录必须与namespace相对应(去除namespace中的com.${company}.模块名)。

      说明:便捷直观， 如果没有对应，会报如下错误信息：
      ``[WARNING] path and namespace is not match.[path=xxx, namespace=xxx]         Failed:         错误文件的名字``,
      并终止代码的生成。

   5. 定义接口的thrift的namespace必须有且仅有一个service,定义数据结构的thrift无此约定。

      说明:在PHP代码生成的过程中，会查找service，并对其进行处理，针对接口会生成三个文件分别为:
      ``Interfaces``, ``Service``, ``Servicespecification``\ 。

   6、所有的thrift必须以".thrift"作为后缀名。

      说明:yz-thrift在生成的过程中会查找相关的thrift文件。

   7、定义namespace的语言必须为nova，即: `namespace nova com.youzan.modulename.xxx` 这种形式。

      说明:无

   例如:我有两个thrift文件，一个定义数据结构的Student.thrift以及定义接口的StudentService.thrift，其内容分别如下:

.. code:: thrift

    namespace nova com.youzan.platform.test
    /**
    * 传输对象*
    **/
    struct Student{
        /**
        * 变量注释*
        **/
        1:required i32 id, //布尔型
        2:required i16 age,//年龄
        3:required string name,//姓名
        4:optional byte sex,//性别
    }

.. code:: thrift

    namespace nova com.youzan.platform.service.student
    include "../../entity/student/Student.thrift"
    /**
    * 服务接口*
    **/
    service StudentService {
    /**
    * 查询列表
    **/
        void addStudent(1:Student.Student student);
        Student.Student getStudent(1:i16 age,2:string name, 3:byte sex);
    }

生成后的目录结构:

::

            ➜ [Downloads] tree platform
                platform
                ├── sdk
                │       ├── gen-java
                │       │      └── com
                │       │             └── youzan
                │       │                     └── platform
                │       │                             ├── entity
                │       │                             │      └── student
                │       │                             │              └── Student.java
                │       │                             └── service
                │       │                                    └── student
                │       │                                            ├── AvatarStudentService.java
                │       │                                            └── StudentService.java
                │       └── gen-php
                │               ├── Entity
                │               │     └── Student
                │               │             └── Student.php
                │               ├── Interfaces
                │               │     └── Student
                │               │            └── StudentService.php
                │               ├── Service
                │               │     └── Student
                │               │             └── StudentService.php
                │               └── Servicespecification
                │                       └── Student
                │                               └── StudentService.php
                └── thrifts
                        ├── entity
                        │     └── student
                        │             └── Student.thrift
                        └── service
                                └── student
                                        └── StudentService.thrift

使用方法
--------

::

    完成安装后，可以使用入下命令生成代码(PS:假如我想将生成的代码放到~/xxx/xxx目录.  

1. PHP(test.thrift):

::

    yz-thrift -gen php -out ~/xxx/xxx/ 

以上命令会在以上命令会在~/xxx/xxx目录里面生成php代码。

2. Java(test.thrift):

::

    yz-thrift -gen java -out ~/xxx/xxx/

以上命令会在~/xxx/xxx目录里面生成java代码。

如果想同时生成java 跟php代码，可以执行如下命令:

::

    yz-thrift

以上命令会在上级目录生成一个sdk目录，sdk目录里面会有gen-java跟gen-php两个目录，里面分别是java跟php的代码

thrift 保留关键字
-----------------

"BEGIN" "END" "**CLASS**" "**DIR**" "**FILE**" "**FUNCTION**" "**LINE**"
"**METHOD**" "**NAMESPACE**" "abstract" "alias" "and" "args" "as"
"assert" "begin" "break" "case" "catch" "class" "clone" "continue"
"declare" "def" "default" "del" "delete" "do" "dynamic" "elif" "else"
"elseif" "elsif" "end" "enddeclare" "endfor" "endforeach" "endif"
"endswitch" "endwhile" "ensure" "except" "exec" "finally" "float" "for"
"foreach" "function" "global" "goto" "if" "implements" "import" "in"
"inline" "instanceof" "interface" "is" "lambda" "module" "native" "new"
"next" "nil" "not" "or" "pass" "public" "print" "private" "protected"
"public" "raise" "redo" "rescue" "retry" "register" "return" "self"
"sizeof" "static" "super" "switch" "synchronized" "then" "this" "throw"
"transient" "try" "undef" "union" "unless" "unsigned" "until" "use"
"var" "virtual" "volatile" "when" "while" "with" "xor" "yield"
