<img width="1327" height="299" alt="image" src="https://github.com/user-attachments/assets/c76f4a27-9509-4843-a798-c1fd36df6f8f" /># MySQL
<img width="936" height="428" alt="image" src="https://github.com/user-attachments/assets/13c0f9d4-5d12-47fd-b645-0dd0b186b835" />
<img width="972" height="670" alt="image" src="https://github.com/user-attachments/assets/a19e6959-7d6d-4812-93c5-52131a01dd04" />
- MySQL是基于关系的数据库，通过表格实现，Redis是非关系数据库，支持多种数据结构（如字符串、哈希等），通过Key-Value实现。

- 数据库的本质是 增create(INSERT INTO users (name, age) VALUES ('张三', 25);) 删delete(DELETE FROM users WHERE id = 1;) 改update(UPDATE users SET age = 26 WHERE name = '张三';) 查read(SELECT * FROM users WHERE age > 20;)

- 库的本质是文件夹，表的本质是文件

- 数据库的存储方式有很多，但是四种最常用innodb、myisam、memory、blackhole。Innodb支持事务性，行级锁，外键约束等，是5.5后版本主用的存储方式，mysiam只能支持到表锁，并发性没有那么高，是5.5之前的主流。
<img width="1190" height="591" alt="image" src="https://github.com/user-attachments/assets/f928a3c5-c8a7-4464-82a8-5f1e3b40936b" />

- innodb偏向于使用自增id作为主键主要是方便查询和管理(如果主键是自增 ID​​：新插入的数据会​​顺序追加​​到 B+树的末尾，写入效率高（减少页分裂和随机 I/O）)，如果使用其他的那么使得排序过于随机(如果主键是无序的（如 UUID）​​：新数据可能插入到 B+树的中间位置，导致频繁的​​页分裂​​和​​磁盘碎片​​，降低写入性能)

- 在B+树中的非叶子节点一般是存储的数字和指向下一个的指针，数字其实本质就是Key -- 自增主键的值

- MyISAM是5.5之前的主要存储方法，Innodb是5.5之后的主要存储方法，仅Innodb是支持外键的，MyISAM采用的是索引和数据分离，而Innodb是一起存储的
<img width="1205" height="497" alt="image" src="https://github.com/user-attachments/assets/60be81de-ea13-48c6-9c03-f461b0feef96" />

- 数据库的宽度是数据的显示宽度，而不是存储宽度，存储宽度在指定存储类型的时候就指定了，比如这个代码，定义的11其实是会用0补齐，本质其实就是占用四个字节的，而varchar是 一个可变的，50表示最多占用50，其具体的占用是根据实际占用而改变的
CREATE TABLE example (
    id INT(11),
    name VARCHAR(50)
);
  
- float、 double、 decimal 中虽然最后一个最精确，但是它的范围最小，​​decimal 是定点数，通常是按​​ ​​十进制形式存储（比如用数组存每一位的数字）; 在 ​​MySQL 的 InnoDB 存储引擎中，一个表中所有 VARCHAR 字段的总长度（加上其他字段）是有上限的，这个上限跟表的行最大存储容量有关

- ​​ENUM（枚举）用于实现单选​​，而 ​​SET（集合）用于实现多选​​

- MySQL的内部结构可以分为服务层和存储层，服务层包含连接器，查询缓存，分析器，优化器，执行器等
<img width="804" height="286" alt="image" src="https://github.com/user-attachments/assets/ba27f36e-8141-48fe-9ae7-8efd6b6a39d7" />


- SQL语言分为四大类，DDL(用于定义或修改数据库对象（如表、索引等）的结构，常见语句包括 CREATE、ALTER、DROP、TRUNCATE 等。​​DDL 通常执行后会自动提交事务，且大多不可回滚)，DML(用于操作表中的数据，包括增删改查，常见语句如 INSERT、UPDATE、DELETE，有时也包括 SELECT（查询）。​​DML 不会自动提交，必须通过 COMMIT 提交或 ROLLBACK 回滚，支持事务)，DCL(用于管理用户权限（如 GRANT、REVOKE）和部分事务控制（如 COMMIT、ROLLBACK、SAVEPOINT）。​​权限语句通常自动提交，事务控制语句用于显式管理事务状态)
<img width="1327" height="299" alt="image" src="https://github.com/user-attachments/assets/3bf097d1-e3f1-47ee-9a23-d363fc5fd5e0" />

- Drop（DDL）是删除整个表，Delete（DML）是删除部分行，并会记录删除操作在日志里面，方便回滚。Truncate（DDL）是保留表删除所有数据，不可回滚
<img width="1057" height="427" alt="image" src="https://github.com/user-attachments/assets/2dcbcea6-4932-488d-95f5-705ac60c53f5" />

- 脏读，不可重读，幻读都是因为并发程度高引起的问题。隔离程度高，那么并发性就会低
  
- 脏读--修改了未提交，然后又发生了回滚，读取了本就不存在的数据。不可重读--数据发生了更改，导致第一次和第二次读取结果不同。幻读--也是指读取的结果不同，但更偏向于数据的增加或者删除。丢弃更改--两人同时对同一个数据进行更改，导致其中一个人的更改丢弃
  
- RR（可重复读）可以防止不可重复读，但是幻读依然存在。RC（读已提交）能防止脏读，RR和RC都是利用了锁实现的。RC利用了共享锁和行级锁，RR利用了行级锁和间隙锁
  
- MySQL使用的是B+ 树，B+ 树的值最后都是放在叶子节点，这样就使得构造的树更加宽，从而索引更加快，且叶子节点之间会有双向链表，支持范围查询
  
- MySQL中的事务回滚主要是依靠undo log实现的，所有对事物的修改都会加入到事务回滚日志中，若提交了修改则无法进行回滚
<img width="1021" height="281" alt="image" src="https://github.com/user-attachments/assets/591a702e-2f1b-4e45-9c25-cae06989b3f4" />
<img width="749" height="360" alt="image" src="https://github.com/user-attachments/assets/64a7e1da-9a6c-4247-9369-4e7cb261435d" />

- 共享锁SHARE 是可以让大家一起读，但是无法修改，可以多个共享锁。排他锁UPDATE是不允许其他人进行读写

- 间隙锁是锁定表格间隙，可以防止幻读，除非事务提交结束了否则是不会进行进程的

- 事务性保证了数据的一致性，ACID。MyISAM是不支持事务性。Innodb能够支持到行级锁，而MyISAM只能支持到表级锁
  
- 锁可以分成三类--总体、类型和粒度，总体就包含乐观锁和悲观锁，类型分为共享锁和排他锁，粒度分为表锁，行锁，页锁，间隙锁，临键锁
  
- 悲观锁是真锁，乐观锁不是真的上锁，只是会在数据更新的时候进行版本比对
  
- 视图是虚拟的表，视图本身就是一个存储的查询，视图可以将复杂的查询简化
  
- 索引查询和非索引查询本质就是在于是否是主键索引，如果是主键索引，那么就可以查询出所有的，但是如果不是主键那就回查询到主键再回表
<img width="1145" height="294" alt="image" src="https://github.com/user-attachments/assets/0d9937ff-9533-4583-b1a6-30719e27fac5" />

- 数据库的索引应该建立在最频繁使用的、用以缩小查询范围的字段上，索引会加大内存损耗

- 第一范式--字段不可分，体现原子性。第二范式--有主键，非主键依赖主键，体现唯一性。第三范式--非主键不能相互依赖，每列有与主键存在关系，不存在传递依赖

- 原子性--事务要么全部成功要么全部回滚。一致性--事务发生后，数据库完整性不会遭到破坏。隔离性--多并发时，多个事务之间相互隔离。 持久性--事务一旦提交那就是永久改变

- vchar是存多少就是多少，存取慢。char是固定长度，不够就空格补齐，存取快

- 想要实现行锁，则必须加上索引，否则就是表锁

- B树的存储是分散的，因此对于磁盘的IO就是多次，因为磁盘的基础单位是页，而B+树是连续存储，因此IO较少

- 虽然哈希比B+快，但是由于哈希只是单个值，而B+树可以有连续存储，因此B+ 是不二之选

- 原子性是通过undo log 实现的。 持久性是通过redo log实现的，redo log会写入磁盘

- MyISAM在使用insert的时候会添加表锁

- 在 MySQL 中，死锁通常发生在 InnoDB 存储引擎使用 行级锁 或 表级锁 时，特别是在事务并发执行时。如果两个事务需要相同的资源，但持有了彼此所需的资源，就可能发生死锁。

- InnoDB 存储引擎会定期检查系统中的事务是否存在死锁。如果检测到死锁，它会选择一个事务作为牺牲者，回滚该事务，使其他事务能够继续执行。
