来源:https://blog.csdn.net/gentelyang/article/details/80372045

##一：数据库引擎的定义

数据库引擎简单来说就是一个"数据库发动机"。当你访问数据库时，不管是手工访问，还是程序访问，都不是直接读写数据库文件，而是 通过数据库引擎去访问数据库文件。以关系型数据库为例，你发SQL语句给数据库引擎， 数据库引擎解释SQL语句，提取出你需要的数据返回给你。因此，对访问者来说，数据库引擎就是SQL语句的解释器。
  正式来说，数据库引擎是用于 存储、处理和保护数据的核心服务。利用数据库引擎可以 控制访问权限并快速处理事务，从而满足企业内大多数需要 处理大量数据的应用程序的要求，这包括 创建用于存储数据的表和用于查看、管理和保护数据安全的数据库对象(如索引、视图和存储过程)。
##二：数据库引擎的任务
1：设计并创建数据库以保存系统所需要的关系或xml文档

2：实现系统以访问或更改数据库中存储的数据，实现网站或使用数据的应用程序，包括使用SOL Server工具和使用工具已使用数据的过程。

3：为单位或用户部署实现的系统

4：提供日常管理支持优化数据库的性能。

##三：mysql引擎的类别

你能用的数据库引擎取决于mysql在安装的时候是如何被编译的。要添加一个新的引擎，就必须重新编译MYSQL。在 缺省情况下,MYSQL支持三个引擎:ISAM、MYISAM和HEAP。另外两种类型INNODB和BERKLEY（BDB），也常常可以使用。如果技术高超，还可以使用MySQL+API自己做一个引擎。

1：ISAM引擎
是一个定义明确且历经时间考验的数据表格管理方法，它在设计之时就考虑到 数据库被查询的次数要远大于更新的次数 。因此， ISAM执行读取操作的速度很快，而且不占用大量的内存和存储资源。
ISAM的结构如下图：
![image.png](https://upload-images.jianshu.io/upload_images/27340183-456fe0c3bf2ddbc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
ISAM的主要不足之处在于，它 不支持事务处理、不支持外来键、不能够容错、也不支持索引。因为ISAM不支持事务，所以如果你的硬盘崩溃了，那么数据文件就无法恢复了。所以如果你正在把ISAM用在关键任务应用程序里，那就必须经常备份你所有的实时数据，通过其复制特性，MySQL能够支持这样的备份应用程序。
2：MyISAM引擎
MyISAM是MySQL的ISAM扩展格式。除了提供ISAM里所没有的索引 (ISAM允许没有任何索引和主键的表存在，索引都是保存行的地址)和字段管理的大量功能， MyISAM还使用一种表格锁定的机制(表级锁)，来优化多个并发的读写操作，其代价是你需要经常运行OPTIMIZE TABLE命令，来恢复被更新机制所浪费的空间，否则碎片也会随之增加，最终影响数据访问性能。
MYISAM强调了快速读取操作 ，这可能就是为什么MySQL受到了WEB开发如此青睐的主要原因：在WEB开发中你所进行的大量数据操作都是读取操作。所以，大多数虚拟主机提供商和INTERNET平台提供商只允许使用MYISAM格式。
3：Heap引擎
Heap存储引擎就是将数据存储在内存中，由于没有磁盘I./O的等待，所以使用该种引擎的表拥有极高的插入、更新和查询效率。这种存储引擎默认使用哈希（HASH）索引，其速度比使用B-+Tree型要快，但也可以使用B树型索引。由于这种存储引擎所存储的数据保存在内存中，所以其保存的数据具有不稳定性，比如如果mysqld进程发生异常、重启或计算机关机等等都会造成这些数据的消失，所以这种存储引擎中的表的生命周期很短，一般只使用一次。
4：InnoDB引擎
In弄DB数据库引擎是早就Mysql灵活性的技术的直接产品，这项技术就是mysql+api，在使用mysql的时候，你所面对的每一个挑战几乎都源于isam和myisam数据库引擎不支持事务处理也不支持外来键。

InnoDB的特点

InnoDB要比isam和myisam引擎慢

innoDB为mysql表提供了acid事务支持，系统崩溃修复能力和多版本并发控制的行级锁，该引擎提供了行级锁和外键约束，所以InnoDB是事务型数据库首选的引擎。

采用B+树实现，索引与数据存储在同一文件中。

##四：InnoDB与MyISAM的对比

1：存储结构

innoDB使用共享表空间存储方式时，所有数据存在一个单独的表空间里面，而这个表空间是由很多个文件组成的，一个表可以跨越多个文件存在，InnoDB表空间的最大限制为64TB，innoDB的表限制基本上在64TB左右，当然这个大小是包括这个表所有索引和其它相关数据。使用单独表空间存储方式时，每个表的数据以一个单独的文件来存储，这个单独文件存放，单表限制了文件系统的大小。

MyISAM中每个表被存在分离的文件中，每个MyISAM中的表在磁盘上存储成三个文件，每一个文件的名字均以表的名字开始，扩展名指出文件类型：.frm文件存储表定义；MYD文件存储表的数据；MYI文件存储表的索引。

2：存储空间

InnoDB存储引擎为在主内存建立其专用的缓冲池来缓存数据和索引，所以需要更多的内存和存储。

MyISAM可被压缩，存储空间较小。支持三种不同的存储格式：静态表(默认，但是注意数据末尾不能有空格，会被去掉)、动态表、压缩表。

MyISAM引擎支持对表的压缩,压缩后的空间上比压缩前会减少60%-70%,但是压缩后的表是只读的,这个要注意,根据使用场景判断是否需要压缩。

3：索引与数据
索引（Index）是帮助MySQL高效获取数据的数据结构。MyIASM和Innodb都使用了树这种数据结构做为索引

Innodb引擎的索引结构是B+Tree，但是这棵树的叶节点data域保存了完整的数据记录，所以InnoDB的 数据文件本身就是索引文件 ，索引和数据是紧密捆绑的。data域的key是数据表的主键。没有使用压缩从而会造成Innodb比MyISAM体积庞大不小。
![image.png](https://upload-images.jianshu.io/upload_images/27340183-261981a0f07e2fa1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图是InnoDB主索引（同时也是数据文件）的示意图，可以看到叶节点包含了完整的数据记录。因为InnoDB的数据文件本身要 按主键聚集，所以InnoDB要求表必须有主键（MyISAM可以没有），如果没有显式指定，则MySQL系统会自动选择一个可以唯一标识数据记录的列作为主键，如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整形。
 InnoDB的 辅助索引data域 存储的也是 相应记录主键的值而不是地址 ，所以当以辅助索引查找时，会先根据辅助索引找到主键，再根据主键索引找到实际的数据。所以InnoDB的所有辅助索引都引用主键作为data域：
![image.png](https://upload-images.jianshu.io/upload_images/27340183-9b1c22e0e94b3224.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
MyISAM引擎的索引结构也是B+Tree,其中B+Tree的data域存储的内容为实际数据的地址，也就是说它的索引和实际的数据是分开的，只不过是用索引指向了实际的数据，所以 MyISAM的索引文件和数据文件是分开的，索引文件仅保存数据记录的地址。MyISAM索引是有压缩的，内存使用率就对应提高了不少。
![image.png](https://upload-images.jianshu.io/upload_images/27340183-bfd4f1b856dd4ca4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在MyISAM中，主索引和辅助索引（Secondary key）在结构上没有任何区别，只是主索引要求key是唯一的，而 辅助索引的key可以重复。与InnoDB不同的是MYISAM辅助索引存储的 data域存储的是地址而不是主键。假设我们以Col1为主键，然后我们在Col2上建立一个辅助索引，则此索引的结构如下图所示：
![image.png](https://upload-images.jianshu.io/upload_images/27340183-6b1e05c5f99753b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
4：是否保存行数
InnoDB中不保存表的具体行数，执行select count(*) from table 时，InnoDB要扫描一遍增高饿表来计算有多少行。

myISAM中存储了表的行数，于是select count(*) from table时只需要直接取已经保存好的值而不需要进行全表扫描。

5：锁

MyISAM支持的是表级锁，而InnoDB支持的是行级锁。但由于锁的粒度更小，写操作不会锁定全表，所以在并发较高时，使用Innodb引擎会提升效率。但是使用行级锁也不是绝对的，如果在执行一个SQL语句时MySQL不能确定要扫描的范围，InnoDB表同样会锁全表，例如update table set num=1 where name like “a%”

6：可移植性、备份及恢复
InnoDB免费的方案可以是拷贝数据文件、备份 binlog，或者用 mysqldump，在数据量达到几十G的时候就相对痛苦了。

MyISAM引擎中的数据是以文件的形式存储，所以在跨平台的数据转移中会很方便。在备份和恢复时可单独针对某个表进行操作

##五：mysql数据引擎更换方式

1：查看当前数据库支持的引擎和默认的数据库引擎

show engines;

2:更改数据库引擎
2.1：更改方式1：修改配置文件my.ini

将my-small。ini另存为my.ini，在default-storage-engine=InnoDB

,重启服务，数据库默认引擎更改为了InnoDB。

2.2更改方法2：在键表的时候指定

create table mytbl(id int primary key,name varchar(50)) type=MyISAM

2.3 :更改方式3：建表后更改

alter table mytbl2 type =InnoDB;

3:查看修改结果

show create table table_name;

show table status from table_name;
