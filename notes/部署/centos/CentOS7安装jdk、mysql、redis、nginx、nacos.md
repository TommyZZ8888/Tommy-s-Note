### jdk1.8

**卸载系统自带的jdk**

xml

 体验AI代码助手

 代码解读

复制代码

`--- 查询系统是否安装jdk rpm -qa|grep java rpm -qa|grep jdk rpm -qa|grep gcj --- 卸载已安装的jdk rpm -e --nodeps java-1.8.0-openjdk-1.8.0.131-11.b12.el7.x86_64 rpm -e --nodeps java-1.7.0-openjdk-1.7.0.141-2.6.10.5.el7.x86_64 rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.131-11.b12.el7.x86_64 rpm -e --nodeps java-1.7.0-openjdk-headless-1.8.0.131-11.b12.el7.x86_64 --- 验证是否卸载完全 rpm -qa|grep java java -version`

**解压安装包**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 新建文件目录 mkdir /usr/local/java -- 解压到指定目录下 tar -zxvf jdk-8u211-linux-x64.tar.gz -C /usr/local/java`

**配置环境变量**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 修改配置 vim /etc/profile -- 加入配置到最后面 # java environment export JAVA_HOME=/usr/local/java/jdk1.8.0_211 export PATH=$PATH:${JAVA_HOME}/bin -- 更新配置文件 、使其生效 source /etc/profile`

**检查是否配置成功**

xml

 体验AI代码助手

 代码解读

复制代码

`javac java -version`

### mysql5.7(rpm安装)

**检查是否安装mysql**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 检查系统中有无安装过 mysqlrpm -qa|grep mysql -- 查询所有mysql 对应的文件夹，全部删除 whereis mysql find / -name mysql`

**卸载系统自带的mariadb**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 查看系统自带的Mariadb rpm -qa|grep mariadb rpm -qa | grep mysql -- 卸载系统自带的Mariadb rpm -e --nodeps mariadb-libs-5.5.44-2.el7.centos.x86_64 -- 删除etc目录下的my.cnf ，一定要删掉，等下再重新建，之前我将就用这个文件，后面改配置各种不生效 rm /etc/my.cnf`

**下载安装包**

xml

 体验AI代码助手

 代码解读

复制代码

`下载地址：https://dev.mysql.com/downloads/mysql/5.7.html#downloads mysql-5.7.25-1.el7.x86_64.rpm-bundle.tar.gz`

**解压安装包**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 解压到指定目录 tar -zxvf mysql-5.7.25-1.el7.x86_64.rpm-bundle.tar.gz -C /usr/local/mysql-5.7 -- 解压完之后,会有较多的rpm文件,这些就是我们需要安装的 ... mysql-community-common-5.7.24-1.el7.x86_64.rpm mysql-community-libs-5.7.24-1.el7.x86_64.rpm mysql-community-client-5.7.24-1.el7.x86_64.rpm mysql-community-server-5.7.24-1.el7.x86_64.rpm ...`

**顺序rpm安装**

xml

 体验AI代码助手

 代码解读

复制代码

 `rpm -ivh mysql-community-common-5.7.24-1.el7.x86_64.rpm    rpm -ivh mysql-community-libs-5.7.24-1.el7.x86_64.rpm    rpm -ivh mysql-community-client-5.7.24-1.el7.x86_64.rpm    rpm -ivh mysql-community-server-5.7.24-1.el7.x86_64.rpm`
 

**启动mysql**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 查看mysql启动状态 service mysqld status -- 启动mysql  service mysqld start`

**修改密码**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 查看临时密码(在冒号最后面) grep password /var/log/mysqld.log -- 用临时密码登录mysql mysql -uroot –p -- 修改成新的密码 set password = password("root"); -- 开启远程连接,允许远程连接数据库 GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '密码' WITH GRANT OPTION;`

**修改密码规则(如上述出现错误使用)**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 查询密码规则 SHOW VARIABLES LIKE 'validate_password%'; -- 重置密码难度为最低 set global validate_password_policy=0; -- 重置密码长度最低为4 set global validate_password_length=4; -- 这样就可设置简单的密码`

**配置my.cnf文件**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 打开配置文件my.cnf vi /etc/my.cnf -- 在[mysqld]下面配置几行(其他配置,可详见my.cnf解析) [mysql] # 设置3306端口 port = 3306 # 设置mysql客户端默认字符集 default-character-set=utf8 [mysqld] # 设置3306端口 port = 3306 # 设置mysql的安装目录 basedir=C:\Program_Java\mysql-5.7.26-winx64 # 设置mysql数据库的数据的存放目录 datadir=C:\Program_Java\mysql-5.7.26-winx64\data # 允许最大连接数 max_connections=200 # 服务端使用的字符集默认为8比特编码的latin1字符集 character-set-server=utf8 # 创建新表时将使用的默认存储引擎 default-storage-engine=INNODB # 开启查询缓存 explicit_defaults_for_timestamp=true -- 重启mysql 重启配置才能生效 service mysqld restart`

**配置my.cnf解析，参考**

xml

 体验AI代码助手

 代码解读

复制代码

`# 客户端设置 [client] port = 3306 # 默认情况下，socket文件应为/usr/local/mysql/mysql.socket,所以可以ln -s xx  /tmp/mysql.sock socket = /tmp/mysql.sock  # 服务端设置 [mysqld] ########################################################################################################## # 基础信息 #Mysql服务的唯一编号 每个mysql服务Id需唯一 server-id = 1 #服务端口号 默认3306 port = 3306 # 启动mysql服务进程的用户 user = mysql ########################################################################################################## # 安装目录相关 # mysql安装根目录 basedir = /usr/local/mysql-5.7.21 # mysql数据文件所在位置 datadir = /usr/local/mysql-5.7.21/data # 临时目录 比如load data infile会用到,一般都是使用/tmp tmpdir  = /tmp # 设置socke文件地址 socket  = /tmp/mysql.sock ########################################################################################################## # 事务隔离级别，默认为可重复读（REPEATABLE-READ）。（此级别下可能参数很多间隙锁，影响性能，但是修改又影响主从复制及灾难恢复，建议还是修改代码逻辑吧） # 隔离级别可选项目：READ-UNCOMMITTED  READ-COMMITTED  REPEATABLE-READ  SERIALIZABLE # transaction_isolation = READ-COMMITTED transaction_isolation = REPEATABLE-READ ########################################################################################################## # 数据库引擎与字符集相关设置 # mysql 5.1 之后，默认引擎就是InnoDB了 default_storage_engine = InnoDB # 内存临时表默认引擎，默认InnoDB default_tmp_storage_engine = InnoDB # mysql 5.7新增特性，磁盘临时表默认引擎，默认InnoDB internal_tmp_disk_storage_engine = InnoDB #数据库默认字符集,主流字符集支持一些特殊表情符号（特殊表情符占用4个字节） character-set-server = utf8 #数据库字符集对应一些排序等规则，注意要和character-set-server对应 collation-server = utf8_general_ci # 设置client连接mysql时的字符集,防止乱码 # init_connect='SET NAMES utf8' # 是否对sql语句大小写敏感，默认值为0，1表示不敏感 lower_case_table_names = 1 ########################################################################################################## # 数据库连接相关设置 # 最大连接数，可设最大值16384，一般考虑根据同时在线人数设置一个比较综合的数字，鉴于该数值增大并不太消耗系统资源，建议直接设10000 # 如果在访问时经常出现Too Many Connections的错误提示，则需要增大该参数值 max_connections = 10000 # 默认值100，最大错误连接数，如果有超出该参数值个数的中断错误连接，则该主机将被禁止连接。如需对该主机进行解禁，执行：FLUSH HOST # 考虑高并发场景下的容错，建议加大。 max_connect_errors = 10000 # MySQL打开的文件描述符限制，默认最小1024; # 当open_files_limit没有被配置的时候，比较max_connections*5和ulimit -n的值，哪个大用哪个， # 当open_file_limit被配置的时候，比较open_files_limit和max_connections*5的值，哪个大用哪个。 open_files_limit = 65535 # 注意：仍然可能出现报错信息Can't create a new thread；此时观察系统cat /proc/mysql进程号/limits，观察进程ulimit限制情况 # 过小的话，考虑修改系统配置表，/etc/security/limits.conf和/etc/security/limits.d/90-nproc.conf # MySQL默认的wait_timeout  值为8个小时, interactive_timeout参数需要同时配置才能生效 # MySQL连接闲置超过一定时间后(单位：秒，此处为1800秒)将会被强行关闭 interactive_timeout = 1800  wait_timeout = 1800  # 在MySQL暂时停止响应新请求之前的短时间内多少个请求可以被存在堆栈中  # 官方建议back_log = 50 + (max_connections / 5),封顶数为900 back_log = 900 ########################################################################################################## # 数据库数据交换设置 # 该参数限制服务器端，接受的数据包大小，如果有BLOB子段，建议增大此值，避免写入或者更新出错。有BLOB子段，建议改为1024M max_allowed_packet = 128M ########################################################################################################## # 内存，cache与buffer设置 # 内存临时表的最大值,默认16M，此处设置成128M tmp_table_size = 64M # 用户创建的内存表的大小，默认16M，往往和tmp_table_size一起设置，限制用户临师表大小。 # 超限的话，MySQL就会自动地把它转化为基于磁盘的MyISAM表，存储在指定的tmpdir目录下，增大IO压力，建议内存大，增大该数值。 max_heap_table_size = 64M # 表示这个mysql版本是否支持查询缓存。ps：SHOW STATUS LIKE 'qcache%'，与缓存相关的状态变量。 # have_query_cache # 这个系统变量控制着查询缓存工能的开启的关闭，0时表示关闭，1时表示打开，2表示只要select 中明确指定SQL_CACHE才缓存。 # 看业务场景决定是否使用缓存，不使用，下面就不用配置了。 query_cache_type = 0  # 默认值1M，优点是查询缓冲可以极大的提高服务器速度, 如果你有大量的相同的查询并且很少修改表。 # 缺点：在你表经常变化的情况下或者如果你的查询原文每次都不同,查询缓冲也许引起性能下降而不是性能提升。 query_cache_size = 64M  # 只有小于此设定值的结果才会被缓冲，保护查询缓冲,防止一个极大的结果集将其他所有的查询结果都覆盖。 query_cache_limit = 2M # 每个被缓存的结果集要占用的最小内存,默认值4kb，一般不怎么调整。 # 如果Qcache_free_blocks值过大，可能是query_cache_min_res_unit值过大，应该调小些 # query_cache_min_res_unit的估计值：(query_cache_size - Qcache_free_memory) / Qcache_queries_in_cache query_cache_min_res_unit = 4kb # 在一个事务中binlog为了记录SQL状态所持有的cache大小 # 如果你经常使用大的,多声明的事务,你可以增加此值来获取更大的性能. # 所有从事务来的状态都将被缓冲在binlog缓冲中然后在提交后一次性写入到binlog中 # 如果事务比此值大, 会使用磁盘上的临时文件来替代. # 此缓冲在每个连接的事务第一次更新状态时被创建 binlog_cache_size = 1M #*** MyISAM 相关选项 # 指定索引缓冲区的大小, 为MYISAM数据表开启供线程共享的索引缓存,对INNODB引擎无效。相当影响MyISAM的性能。 # 不要将其设置大于你可用内存的30%,因为一部分内存同样被OS用来缓冲行数据 # 甚至在你并不使用MyISAM 表的情况下, 你也需要仍旧设置起 8-64M 内存由于它同样会被内部临时磁盘表使用. # 默认值 8M，建议值：对于内存在4GB左右的服务器该参数可设置为256M或384M。注意：该参数值设置的过大反而会是服务器整体效率降低！ key_buffer_size = 64M # 为每个扫描MyISAM的线程分配参数设置的内存大小缓冲区。  # 默认值128kb，建议值：16G内存建议1M，4G：128kb或者256kb吧 # 注意，该缓冲区是每个连接独占的，所以总缓冲区大小为 128kb*连接数；极端情况128kb*maxconnectiosns，会超级大，所以要考虑日常平均连接数。 # 一般不需要太关心该数值，稍微增大就可以了， read_buffer_size = 262144  # 支持任何存储引擎 # MySQL的随机读缓冲区大小，适当增大，可以提高性能。 # 默认值256kb；建议值：得参考连接数，16G内存，有人推荐8M # 注意，该缓冲区是每个连接独占的，所以总缓冲区大小为128kb*连接数；极端情况128kb*maxconnectiosns，会超级大，所以要考虑日常平均连接数。 read_rnd_buffer_size = 1M # order by或group by时用到  # 支持所有引擎，innodb和myisam有自己的innodb_sort_buffer_size和myisam_sort_buffer_size设置 # 默认值256kb；建议值：得参考连接数，16G内存，有人推荐8M. # 注意，该缓冲区是每个连接独占的，所以总缓冲区大小为 1M*连接数；极端情况1M*maxconnectiosns，会超级大。所以要考虑日常平均连接数。 sort_buffer_size = 1M # 此缓冲被使用来优化全联合(full JOINs 不带索引的联合) # 类似的联合在极大多数情况下有非常糟糕的性能表现,但是将此值设大能够减轻性能影响. # 通过 “Select_full_join” 状态变量查看全联合的数量 # 注意，该缓冲区是每个连接独占的，所以总缓冲区大小为 1M*连接数；极端情况1M*maxconnectiosns，会超级大。所以要考虑日常平均连接数。 # 默认值256kb;建议值：16G内存，设置8M. join_buffer_size = 1M # 缓存linux文件描述符信息，加快数据文件打开速度 # 它影响myisam表的打开关闭，但是不影响innodb表的打开关闭。 # 默认值2000，建议值：根据状态变量Opened_tables去设定 table_open_cache = 2000 # 缓存表定义的相关信息，加快读取表信息速度 # 默认值1400，最大值2000，建议值：基本不改。 table_definition_cache = 1400 # 该参数是myssql 5.6后引入的，目的是提高并发。 # 默认值1，建议值：cpu核数，并且<=16 table_open_cache_instances = 2 # 当客户端断开之后，服务器处理此客户的线程将会缓存起来以响应下一个客户而不是销毁。可重用，减小了系统开销。 # 默认值为9，建议值：两种取值方式，方式一，根据物理内存，1G  —> 8；2G  —> 16； 3G  —> 32； >3G  —> 64； # 方式二，根据show status like  'threads%'，查看Threads_connected值。 thread_cache_size = 16 # 默认值256k,建议值：16/32G内存，512kb，其他一般不改变，如果报错：Thread stack overrun，就增大看看, # 注意，每个线程分配内存空间，所以总内存空间。。。你懂得。 thread_stack = 512k ########################################################################################################## # 日志文件相关设置，一般只开启三种日志，错误日志，慢查询日志，二进制日志。普通查询日志不开启。 # 普通查询日志，默认值off，不开启 general_log = 0 # 普通查询日志存放地址 general_log_file = /usr/local/mysql-5.7.21/log/mysql-general.log # 全局动态变量，默认3，范围：1～3 # 表示错误日志记录的信息，1：只记录error信息；2：记录error和warnings信息；3：记录error、warnings和普通的notes信息。 log_error_verbosity = 2 # 错误日志文件地址 log_error = /usr/local/mysql-5.7.21/log/mysql-error.log # 开启慢查询 slow_query_log = 1 # 开启慢查询时间，此处为1秒，达到此值才记录数据 long_query_time = 3 # 检索行数达到此数值，才记录慢查询日志中 min_examined_row_limit = 100 # mysql 5.6.5新增，用来表示每分钟允许记录到slow log的且未使用索引的SQL语句次数，默认值为0，不限制。 log_throttle_queries_not_using_indexes = 0 # 慢查询日志文件地址 slow_query_log_file = /usr/local/mysql-5.7.21/log/mysql-slow.log # 开启记录没有使用索引查询语句 log-queries-not-using-indexes = 1 # 开启二进制日志 log_bin = /usr/local/mysql-5.7.21/log/mysql-bin.log # mysql清除过期日志的时间，默认值0，不自动清理，而是使用滚动循环的方式。 expire_logs_days = 0 # 如果二进制日志写入的内容超出给定值，日志就会发生滚动。你不能将该变量设置为大于1GB或小于4096字节。 默认值是1GB。 max_binlog_size = 1000M # binlog的格式也有三种：STATEMENT，ROW，MIXED。mysql 5.7.7后，默认值从 MIXED 改为 ROW # 关于binlog日志格式问题，请查阅网络资料 binlog_format = row # 默认值N=1，使binlog在每N次binlog写入后与硬盘同步，ps：1最慢 # sync_binlog = 1  ########################################################################################################## # innodb选项 # 说明：该参数可以提升扩展性和刷脏页性能。 # 默认值1，建议值：4-8；并且必须小于innodb_buffer_pool_instances innodb_page_cleaners = 4 # 说明：一般8k和16k中选择，8k的话，cpu消耗小些，selcet效率高一点，一般不用改 # 默认值：16k；建议值：不改， innodb_page_size = 16384 # 说明：InnoDB使用一个缓冲池来保存索引和原始数据, 不像MyISAM.这里你设置越大,你在存取表里面数据时所需要的磁盘I/O越少. # 在一个独立使用的数据库服务器上,你可以设置这个变量到服务器物理内存大小的60%-80% # 注意别设置的过大，会导致system的swap空间被占用，导致操作系统变慢，从而减低sql查询的效率 # 默认值：128M，建议值：物理内存的60%-80% innodb_buffer_pool_size = 512M # 说明:只有当设置 innodb_buffer_pool_size 值大于1G时才有意义，小于1G，instances默认为1，大于1G，instances默认为8 # 但是网络上有评价，最佳性能，每个实例至少1G大小。 # 默认值：1或8，建议值：innodb_buffer_pool_size/innodb_buffer_pool_instances >= 1G innodb_buffer_pool_instances = 1 # 说明：mysql 5.7 新特性，defines the chunk size for online InnoDB buffer pool resizing operations. # 实际缓冲区大小必须为innodb_buffer_pool_chunk_size*innodb_buffer_pool_instances*倍数，取略大于innodb_buffer_pool_size # 默认值128M，建议值：默认值就好，乱改反而容易出问题，它会影响实际buffer pool大小。 innodb_buffer_pool_chunk_size = 128M  # 在启动时把热数据加载到内存。默认值为on，不修改 innodb_buffer_pool_load_at_startup = 1 # 在关闭时把热数据dump到本地磁盘。默认值为on，不修改 innodb_buffer_pool_dump_at_shutdown = 1 # 说明：影响Innodb缓冲区的刷新算法，建议从小到大配置，直到zero free pages；innodb_lru_scan_depth * innodb_buffer_pool_instances defines the amount of work performed by the page cleaner thread each second. # 默认值1024，建议值: 未知 innodb_lru_scan_depth = 1024 # 说明：事务等待获取资源等待的最长时间，单位为秒，看具体业务情况，一般默认值就好 # 默认值：50，建议值：看业务。 innodb_lock_wait_timeout = 60 # 说明：设置了Mysql后台任务（例如页刷新和merge dadta from buffer pool）每秒io操作的上限。 # 默认值：200，建议值：方法一，单盘sata设100，sas10，raid10设200，ssd设2000，fushion-io设50000；方法二，通过测试工具获得磁盘io性能后，设置IOPS数值/2。 innodb_io_capacity = 2000 # 说明：该参数是所有缓冲区线程io操作的总上限。 # 默认值：innodb_io_capacity的两倍。建议值：例如用iometer测试后的iops数值就好 innodb_io_capacity_max = 4000 # 说明：控制着innodb数据文件及redo log的打开、刷写模式，三种模式：fdatasync(默认)，O_DSYNC，O_DIRECT # fdatasync：数据文件，buffer pool->os buffer->磁盘；日志文件，buffer pool->os buffer->磁盘； # O_DSYNC：  数据文件，buffer pool->os buffer->磁盘；日志文件，buffer pool->磁盘； # O_DIRECT： 数据文件，buffer pool->磁盘；           日志文件，buffer pool->os buffer->磁盘； # 默认值为空，建议值：使用SAN或者raid，建议用O_DIRECT，不懂测试的话，默认生产上使用O_DIRECT innodb_flush_method = O_DIRECT # 说明：mysql5.7之后默认开启，意思是，每张表一个独立表空间。 # 默认值1，开启 innodb_file_per_table = 1 # 说明：The path where InnoDB creates undo tablespaces.通常等于undo log文件的存放目录。 # 默认值./;自行设置 innodb_undo_directory = /usr/local/mysql-5.7.21/log # 说明：The number of undo tablespaces used by InnoDB.等于undo log文件数量。5.7.21后开始弃用 # 默认值为0，建议默认值就好，不用调整了。 innodb_undo_tablespaces = 0 # 说明：定义undo使用的回滚段数量。5.7.19后弃用 # 默认值128，建议不动，以后弃用了。 innodb_undo_logs = 128 # 说明：5.7.5后开始使用，在线收缩undo log使用的空间。 # 默认值：关闭，建议值：开启 innodb_undo_log_truncate = 1 # 说明：结合innodb_undo_log_truncate，实现undo空间收缩功能 # 默认值：1G，建议值，不改。 innodb_max_undo_log_size = 1G # 说明：重作日志文件的存放目录 innodb_log_group_home_dir = /usr/local/mysql-5.7.21/log # 说明：日志文件的大小 # 默认值:48M,建议值：根据你系统的磁盘空间和日志增长情况调整大小 innodb_log_file_size = 128M # 说明：日志组中的文件数量，mysql以循环方式写入日志 # 默认值2，建议值：根据你系统的磁盘空间和日志增长情况调整大小 innodb_log_files_in_group = 3 # 此参数确定些日志文件所用的内存大小，以M为单位。缓冲区更大能提高性能，但意外的故障将会丢失数据。MySQL开发人员建议设置为1－8M之间 innodb_log_buffer_size = 16M # 说明：可以控制log从系统buffer刷入磁盘文件的刷新频率，增大可减轻系统负荷 # 默认值是1；建议值不改。系统性能一般够用。 innodb_flush_log_at_timeout = 1 # 说明：参数可设为0，1，2； # 参数0：表示每秒将log buffer内容刷新到系统buffer中，再调用系统flush操作写入磁盘文件。 # 参数1：表示每次事物提交，将log buffer内容刷新到系统buffer中，再调用系统flush操作写入磁盘文件。 # 参数2：表示每次事物提交，将log buffer内容刷新到系统buffer中，隔1秒后再调用系统flush操作写入磁盘文件。 innodb_flush_log_at_trx_commit = 1 # 说明：限制Innodb能打开的表的数据，如果库里的表特别多的情况，请增加这个。 # 值默认是2000，建议值：参考数据库表总数再进行调整，一般够用不用调整。 innodb_open_files = 8192 # innodb处理io读写的后台并发线程数量，根据cpu核来确认，取值范围：1-64 # 默认值：4，建议值：与逻辑cpu数量的一半保持一致。 innodb_read_io_threads = 4 innodb_write_io_threads = 4 # 默认设置为 0,表示不限制并发数，这里推荐设置为0，更好去发挥CPU多核处理能力，提高并发量 innodb_thread_concurrency = 0 # 默认值为4，建议不变。InnoDB中的清除操作是一类定期回收无用数据的操作。mysql 5.5之后，支持多线程清除操作。 innodb_purge_threads = 4  # 说明：mysql缓冲区分为new blocks和old blocks；此参数表示old blocks占比； # 默认值：37，建议值，一般不动 innodb_old_blocks_pct = 37 # 说明：新数据被载入缓冲池，进入old pages链区，当1秒后再次访问，则提升进入new pages链区。 # 默认值：1000 innodb_old_blocks_time=1000 # 说明：开启异步io，可以提高并发性，默认开启。 # 默认值为1，建议不动 innodb_use_native_aio = 1 # 说明：默认为空，使用data目录，一般不改。 innodb_data_home_dir=/usr/local/mysql-5.7.21/data # 说明：Defines the name, size, and attributes of InnoDB system tablespace data files. # 默认值，不指定，默认为ibdata1:12M:autoextend innodb_data_file_path = ibdata1:12M:autoextend # 说明:设置了InnoDB存储引擎用来存放数据字典信息以及一些内部数据结构的内存空间大小,除非你的数据对象及其多，否则一般默认不改。 # innodb_additional_mem_pool_size = 16M # 说明：The crash recovery mode。只有紧急情况需要恢复数据的时候，才改为大于1-6之间数值，含义查下官网。 # 默认值为0； #innodb_force_recovery = 0 ############################################################################################### # 其他。。。。 # 参考http://www.kuqin.com/database/20120815/328905.html # skip-external-locking # 禁止MySQL对外部连接进行DNS解析，使用这一选项可以消除MySQL进行DNS解析的时间。 # 缺点：所有远程主机连接授权都要使用IP地址方式，因为只认得ip地址了。 # skip_name_resolve = 0 # 默认值为off,timestamp列会自动更新为当前时间，设置为on|1，timestamp列的值就要显式更新 explicit_defaults_for_timestamp = 1 [mysqldump] # quick选项强制 mysqldump 从服务器查询取得记录直接输出而不是取得所有记录后将它们缓存到内存中 quick max_allowed_packet = 16M [mysql] # mysql命令行工具不使用自动补全功能，建议还是改为 # no-auto-rehash auto-rehash socket = /tmp/mysql.sock`

**注意：需要开启3306端口或者直接关闭防火墙**

### redis

**安装gcc依赖**

xml

 体验AI代码助手

 代码解读

复制代码

`gcc --version # 查看gcc版本检测是否存在 whereis gcc # 查看gcc的位置 -- 由于 redis 是用 C 语言开发，安装之前必先确认是否安装 gcc 环境（gcc -v），如果没有安装，执行以下命令进行安装 yum install -y gcc` 

**下载并解压安装包**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 下载 wget http://download.redis.io/releases/redis-5.0.3.tar.gz -- 解压到/uer/local目录下 tar -zxvf redis-5.0.3.tar.gz -C /usr/local -- 切换到/uer/local/redis-5.0.3目录下,编译 make -- 安装并指定安装目录 make install PREFIX=/usr/local/redis`

**后台启动**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 从 redis 的源码目录中复制 redis.conf 到 redis 的安装目录 cp /usr/local/redis-5.0.3/redis.conf /usr/local/redis/bin/      -- 修改 redis.conf 文件，把 daemonize no 改为 daemonize yes -- 设置密码 requirepass 6i4ygz8zkx7q vim redis.conf -- 后台启动 ./redis-server redis.conf -- 查看是否启动 ps -ef|grep redis -- 停止 ./redis-cli shutdown`

**添加开机启动服务**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 新建文件夹 vim /etc/systemd/system/redis.service -- 添加配置数据 [Unit] Description=The redis-server Process Manager After=syslog.target network.target [Service] Type=simple PIDFile=/var/run/redis_6379.pid ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/bin/redis.conf ExecReload=/bin/kill -USR2 $MAINPID ExecStop=/bin/kill -SIGINT $MAINPID [Install] WantedBy=multi-user.target ### 或者(建议使用第下面该方式) [Unit] Description=redis-server After=network.target [Service] Type=forking ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/bin/redis.conf PrivateTmp=true [Install] WantedBy=multi-user.target       ### 解析 Description:描述服务 After:描述服务类别 [Service]服务运行参数的设置 Type=forking是后台运行的形式 ExecStart为服务的具体运行命令 ExecReload为重启命令 ExecStop为停止命令 PrivateTmp=True表示给服务分配独立的临时空间 注意：[Service]的启动、重启、停止命令全部要求使用绝对路径 [Install]运行级别下服务安装的相关设置，可设置为多用户，即系统运行级别为3`

**设置开机启动**

xml

 体验AI代码助手

 代码解读

复制代码

`systemctl daemon-reload systemctl start redis.service systemctl enable redis.service`

**服务操作命令**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 添加完开机启动服务后,以下命令生效 systemctl start redis.service   	#启动redis服务 systemctl stop redis.service   		#停止redis服务 systemctl restart redis.service   	#重新启动服务 systemctl status redis.service   	#查看服务当前状态 systemctl enable redis.service   	#设置开机自启动 systemctl disable redis.service   	#停止开机自启动`

### nginx

**gcc安装**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 检查是否安装 gcc -v -- 安装gcc yum install gcc-c++`

**pcre安装**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 检查是否安装 rpm -qa pcre -- 安装pcre yum install -y pcre pcre-devel`

**zlib安装**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 检查是否安装 yum list installed | grep zlib* -- 安装zlib yum install -y zlib zlib-devel`

**OpenSSL安装**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 检查是否安装 rpm -qa openssl -- 安装OpenSSL yum install -y openssl openssl-devel`

**解压nginx压缩包**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 解压到/usr/local tar -zxvf nginx-1.9.9.tar.gz -C /usr/local`

**编译安装**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 切换到/usr/local/nginx-1.9.9安装目录 cd /usr/local/nginx-1.9.9/      -- 编译安装 ./configure --with-http_ssl_module make make install`

**nginx.conf配置**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 切换到安装位置 cd /usr/local/nginx -- 配置文件的位置 vim /usr/local/nginx/conf/nginx.conf`

**nginx常用命令**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 启动 sbin/nginx -- 查看nginx服务是否启动成功 ps -ef | grep nginx -- 正常停止 sbin/nginx -s stop -- 强制停止 sbin/nginx -s quit -- 重新加载nginx.conf配置 sbin/nginx -s reload`

**配置开机自启动**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 切换到指定目录下,新建nginx.service文件 vim /etc/systemd/system/nginx.service -- 添加配置数据 [Unit] Description=nginx service After=network.target      [Service]  Type=forking  ExecStart=/usr/local/nginx/sbin/nginx ExecReload=/usr/local/nginx/sbin/nginx -s reload ExecStop=/usr/local/nginx/sbin/nginx -s quit PrivateTmp=true      [Install]  WantedBy=multi-user.target`

**设置开机启动**

xml

 体验AI代码助手

 代码解读

复制代码

`systemctl daemon-reload			# 添加服务 systemctl start nginx.service		# 启动nginx服务 systemctl enable nginx.service		# 设置开机自启动`

**服务操作命令**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 添加完开机启动服务后,以下命令生效 systemctl start nginx.service　         # 启动nginx服务 systemctl stop nginx.service　          # 停止服务 systemctl restart nginx.service　       # 重新启动服务 systemctl list-units --type=service    	# 查看所有已启动的服务 systemctl status nginx.service         	# 查看服务当前状态 systemctl enable nginx.service         	# 设置开机自启动 systemctl disable nginx.service        	# 停止开机自启动`

### nacos

**解压：安装前需要jdk环境**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 解压到指定目录 tar -zxvf nacos-server-2.0.0-ALPHA.1.tar.gz -C /usr/local`

**单机启动**

xml

 体验AI代码助手

 代码解读

复制代码

`-- nacos默认是集群启动,需要修改/usr/local/nacos/bin/startup.sh中的配置 -- 大概在第58行出现export MODE="cluster",cluster表示集群启动 -- 修改为export MODE="standalone",standalone表示单机启动`

**nacos命令**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 启动nacos ./startup.sh -- 停止nacos ./shutdown.sh -- 查看启动日志 vim /usr/local/nacos/logs/start.out`

**持久化配置**

xml

 体验AI代码助手

 代码解读

复制代码

`-- nacos安装目录下的conf/application.properties修改 spring.datasource.platform=mysql db.num=1 db.url.0=jdbc:mysql://localhost:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true db.user=root db.password=admin`

**nacos启动失败原因**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 异常信息 Please set the JAVA_HOME variable in your environment, We need java(x64)! -- 原因 nacos未找到jdk的安装路径 -- 方法一 全局配置jdk环境 -- 方法二 在nacos目录下的/bin/startup.sh文件中最顶部添加JAVA_HOME=/usr/local/java/jdk1.8.0_251的安装路径`

**配置开机自启动**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 切换到指定目录下,新建nacos.service文件 vim /etc/systemd/system/nacos.service -- 添加配置数据 [Unit] Description=nacos After=network.target   [Service] Type=forking ExecStart=/usr/local/nacos/bin/startup.sh -m standalone ExecReload=/usr/local/nacos/bin/shutdown.sh ExecStop=/usr/local/nacos/bin/shutdown.sh PrivateTmp=true   [Install]   WantedBy=multi-user.target`

**设置开机启动**

xml

 体验AI代码助手

 代码解读

复制代码

`systemctl daemon-reload			# 添加服务 systemctl start nacos.service		# 启动nacos服务 systemctl enable nacos.service		# 设置开机自启动`

**服务操作命令**

xml

 体验AI代码助手

 代码解读

复制代码

`-- 添加完开机启动服务后,以下命令生效 systemctl start nacos.service　         # 启动nacos服务 systemctl stop nacos.service　          # 停止服务 systemctl restart nacos.service　       # 重新启动服务 systemctl list-units --type=service    	# 查看所有已启动的服务 systemctl status nacos.service         	# 查看服务当前状态 systemctl enable nacos.service         	# 设置开机自启动 systemctl disable nacos.service        	# 停止开机自启动`

本文转自 <https://juejin.cn/post/7118932184370511908>，如有侵权，请联系删除。