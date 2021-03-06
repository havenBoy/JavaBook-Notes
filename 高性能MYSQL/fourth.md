# schema与数据类型设计
> MYSQL数据库与其他关系数据库管理系统的区别，充分体会数据库基础设计的基本知识

* 选择优化的数据类型，原则如下：
  - 更小的通常更好  一般占用较小的磁盘 内存与CPU缓存，但是需要确保存储值的范围
  - 简单就好   简单类型数据的操作需要更少的CPU周期
  - 避免使用NULL  存储null的值使得索引 索引统计值更加复杂 需要占用到更多的空间，需要特殊处理，当为null列值被索引时，每个索引记录需要一个额外的字节，一般的做法是把null变成not null，但是性能提升较小，一般的原则是假如这个列会有null值出现，最好不要再这个列上创建索引
  - 在选择数据类型时一般是三大类：数字，字符串，时间，再下一步才是具体类型的选择；
* 具体数据类型介绍
  - 整数类型 
    * 分为整数与实数，tinyInt,smallInt,mediumInt,int,bigInt,类型可选unsigned，表示不允许负数；
    * 选择不同的类型决定了数据在数据库中是如何存储数据的
  - 实数类型
    * 指的是带有小数部分的数字，mysql支持精确的数据类型
    * demical在精确计算的时候才使用，否则考虑bigInt代替demical
  - 字符串类型
    * varchar 存储可变长字符串，比定长节省空间；使用1-2个自己存储字符串长度；
    * char 是定长的，在存储时会删除末尾的空格，适合存储很短的字符串；
    * binary binary 存储二进制字符串，存储的字节码不是字符，二进制比字符要简单的多，也更快
    * blob text 存储很大的数据的字符串数据类型 分别采用二进制与字符串存储，存储使用指针，真正的数据存放在存储区域
    * 使用枚举代替字符串 把不重复的字符串存储在一个预定义的集合 
    * 日期与时间类型  对于datatime与timestamp对比：
      - datetime 从1001到9999年，精度为秒，使用8个字节存储，与时区无关
      - timestamp 保存从1970到现在的秒数，与Unix的时间戳相同，只能表示1970-2038年
      - 建议：尽量使用时间戳，因为其空间效率高
  - 位数据类型
    * bit 用来存储一个或者多个true/false值  避免使用
    * set 如果需要保存许多的true/false值，优点是有效利用空间，但缺点是定义的代价较高，无法在列上进行索引查询
  - 选择标识符
    * 选择标识符时，不仅需要考虑存储类型，还要考虑MySQL是如何对这种数据类型进行处理的
    * 在可以满足值的范围需求，并且预留未来增长空间的前提下，选择最小的数据类型
    * 特殊类型数据 使用无符号整数来存储IP地址， IP地址本身是32位无符号整数
* MySQL schema设计中的陷阱
  - 太多的列：存储引擎需要在服务器层与存储引擎层之间进行行缓冲格式数数据拷贝数据，之后是在服务器层把缓冲内容解码成各个列，这个操作代价是比较高的
  - 太多的关联： 单个查询的表之间的关联最好少一些
  - 全能的枚举： 防止过度的使用枚举
  - 变相的枚举： 允许在列中存储一组定义值的单个值，集合允许在列中定义一个或者多个值
  - 非此发明的NULL： 