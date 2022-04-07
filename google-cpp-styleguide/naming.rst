7. 命名约定
------------------

最重要的一致性规则是命名管理. 命名的风格能让我们在不需要去查找类型声明的条件下快速地了解某个名字代表的含义: 类型, 变量, 函数, 常量, 宏, 等等, 甚至. 我们大脑中的模式匹配引擎非常依赖这些命名规则.

命名规则具有一定随意性, 但相比按个人喜好命名, 一致性更重要, 所以无论你认为它们是否重要, 规则总归是规则.

.. _general-naming-rules:

7.1. 通用命名规则
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**总述**

函数命名, 变量命名, 文件命名要有描述性; 少用缩写.

**说明**

尽可能使用描述性的命名, 别心疼空间, 毕竟相比之下让代码易于新读者理解更重要. 不要用只有项目开发者能理解的缩写, 也不要通过砍掉几个字母来缩写单词.

.. code-block:: c

    int price_count_reader;    // 无缩写
    int num_errors;            // "num" 是一个常见的写法
    int num_dns_connections;   // 人人都知道 "DNS" 是什么

.. code-block:: c

    int n;                     // 毫无意义.
    int nerr;                  // 含糊不清的缩写.
    int n_comp_conns;          // 含糊不清的缩写.
    int wgc_connections;       // 只有贵团队知道是什么意思.
    int pc_reader;             // "pc" 有太多可能的解释了.
    int cstmr_id;              // 删减了若干字母.

注意, 一些特定的广为人知的缩写是允许的, 例如用 ``i`` 表示迭代变量.


7.2. 文件命名
~~~~~~~~~~~~~~~~~~~~~~

**总述**

文件名要全部小写, 可以包含下划线 (``_``) 或连字符 (``-``), 依照项目的约定. 如果没有约定, 那么 "``_``" 更好.

**说明**

可接受的文件命名示例:

* ``my_useful_class.c``

* ``my-useful-class.c``

* ``myusefulclass.c``

* ``myusefulclass_test.c`` // ``_unittest`` 和 ``_regtest`` 已弃用.

C 文件要以 ``.c`` 结尾, 头文件以 ``.h`` 结尾. 专门插入文本的文件则以 ``.inc`` 结尾, 参见 :ref:`头文件自足 <self-contained-headers>`.

不要使用已经存在于 ``/usr/include`` 下的文件名 (Yang.Y 注: 即编译器搜索系统头文件的路径), 如 ``db.h``.

通常应尽量让文件名更加明确. ``http_server_logs.h`` 就比 ``logs.h`` 要好.

内联函数必须放在 ``.h`` 文件中. 如果内联函数比较短, 就直接放在 ``.h`` 中.

.. _type-names:

7.3. 类型命名
~~~~~~~~~~~~~~~~~~~~~~

**总述**

类型名称的每个单词首字母均大写, 不包含下划线: ``MyExcitingClass``, ``MyExcitingEnum``.

**说明**

所有类型命名 —— 结构体, 类型定义 (``typedef``), 枚举 —— 均使用相同约定, 即以大写字母开始, 每个单词首字母均大写, 不包含下划线. 例如:

.. code-block:: c

    // 结构体
    struct UrlTableProperties { ...

    // 类型定义
    typedef Type * TypePtr;

    // 枚举
    enum UrlTableErrors { ...

.. _variable-names:

7.4. 变量命名
~~~~~~~~~~~~~~~~~~~~~~

**总述**

变量 (包括函数参数) 和数据成员名一律小写, 单词之间用下划线连接.

**说明**

普通变量命名
=============================

举例:

.. code-block:: c

    string table_name;  // 好 - 用下划线.
    string tablename;   // 好 - 全小写.

    string tableName;  // 差 - 混合大小写

结构体变量
=============================

不管是静态的还是非静态的, 结构体数据成员都可以和普通变量一样:

.. code-block:: c

    struct UrlTableProperties {
      string name;
      int num_entries;
      Pool *pool;
    };

.. _constant-names:

7.5. 常量命名
~~~~~~~~~~~~~~~~~~~~~~

**总述**

声明为 ``const`` 的变量, 或在程序运行期间其值始终保持不变的, 命名时以 "k" 开头, 大小写混合. 例如:

.. code-block:: c

    const int kDaysInAWeek = 7;

**说明**

所有具有静态存储类型的变量 (例如静态变量或全局变量, 参见 `存储类型 <http://en.cppreference.com/w/cpp/language/storage_duration#Storage_duration>`_) 都应当以此方式命名. 对于其他存储类型的变量, 如自动变量等, 这条规则是可选的. 如果不采用这条规则, 就按照一般的变量命名规则.

.. _function-names:

7.6. 函数命名
~~~~~~~~~~~~~~~~~~~~~~

**总述**

常规函数使用大小写混合: ``MyExcitingFunction()``, ``MyExcitingMethod()``.

**说明**

一般来说, 函数名的每个单词首字母大写 (即 "驼峰变量名" 或 "帕斯卡变量名"), 没有下划线. 对于首字母缩写的单词, 更倾向于将它们视作一个单词进行首字母大写 (例如, 写作 ``StartRpc()`` 而非 ``StartRPC()``).

.. code-block:: c

    AddTableEntry()
    DeleteUrl()
    OpenFileOrDie()

7.7. 枚举命名
~~~~~~~~~~~~~~~~~~~~~~

**总述**

枚举的命名应当和 :ref:`常量 <constant-names>` 或 :ref:`宏 <macro-names>` 一致: ``kEnumName`` 或是 ``ENUM_NAME``.

**说明**

单独的枚举值应该优先采用 :ref:`常量 <constant-names>` 的命名方式. 但 :ref:`宏 <macro-names>` 方式的命名也可以接受. 枚举名 ``UrlTableErrors`` (以及 ``AlternateUrlTableErrors``) 是类型, 所以要用大小写混合的方式.

.. code-block:: c

    enum UrlTableErrors {
        kOK = 0,
        kErrorOutOfMemory,
        kErrorMalformedInput,
    };
    enum AlternateUrlTableErrors {
        OK = 0,
        OUT_OF_MEMORY = 1,
        MALFORMED_INPUT = 2,
    };

2009 年 1 月之前, 我们一直建议采用 :ref:`宏 <macro-names>` 的方式命名枚举值. 由于枚举值和宏之间的命名冲突, 直接导致了很多问题. 由此, 这里改为优先选择常量风格的命名方式. 新代码应该尽可能优先使用常量风格. 但是老代码没必要切换到常量风格, 除非宏风格确实会产生编译期问题.

.. _macro-names:

7.8. 宏命名
~~~~~~~~~~~~~~~~~~

**总述**

像这样命名: ``MY_MACRO_THAT_SCARES_SMALL_CHILDREN``.

**说明**

参考 :ref:`预处理宏 <preprocessor-macros>`; 通常 *不应该* 使用宏. 如果不得不用, 其命名像枚举命名一样全部大写, 使用下划线:

.. code-block:: c

    #define ROUND(x) ...
    #define PI_ROUNDED 3.0

7.9. 命名规则的特例
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**总述**

如果你命名的实体与已有 C/C++ 实体相似, 可参考现有命名策略.

``bigopen()``: 函数名, 参照 ``open()`` 的形式

``uint``: ``typedef``

``bigpos``: ``struct``, 参照 ``pos`` 的形式

``LONGLONG_MAX``: 常量, 如同 ``INT_MAX``

译者（acgtyrant）笔记
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#. 感觉 Google 的命名约定很高明, 比如写了简单的类 QueryResult, 接着又可以直接定义一个变量 query_result, 区分度很好. 
