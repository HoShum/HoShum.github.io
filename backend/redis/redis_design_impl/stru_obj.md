# 数据结构和对象

## 大纲
相信很多人都用过Redis，也知道Redis是一个Key-Value键值对形的数据库，但是如果它只是一个简单的键值对型数据库，那在Java中也完全可以使用`Map`去代替，为什么还需要用Redis呢？<br/>

本章节会为你解答这个问题，会给你展示Redis中的数据是以什么样的形式进行存储，又做了哪些优化
> 注意，书中主要展示的是以原理为主，而非源码，对源码感兴趣请查阅其他资料

## 数据结构

### 1、简单动态字符串 SDS

#### 结构

```c
struct sdshdr {
    //记录buf数组中已使用字节的数量
    //等于SDS所保存字符串的长度
    int len;
    //记录buf数组中未使用字节的数量
    int free;
    //字节数组，用于保存字符串
    char buf[];
};
```
<div align="center">
    <img src="https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306252311851.png" style="zoom:50%;" />
</div>
#### 特点

1.  兼容C语言语法：保留了C语言以空字符结尾的特点，使其可以兼容部分C语言跟字符串相关的函数
2.  获取长度快：使用`len`字段来获取字符串长度，获取长度时间复杂度为O(1)，因为SDS常用于作为key，提高了效率
3.  二进制安全：C语言中规定了字符串以`\0`作为结束标记，因此如果一个字符串中有该标记，则容易导致消息的不完整，且C语言中规定了只能使用特定的编码，因此C语言字符串只能用来存放文本数据；而SDS则不同，因为它是通过`len`来判断是否结束，因此理论上SDS能存放任何二进制数据
4.  杜绝缓冲区溢出：C语言中，假设现在有`s1`和`s2`两个字符串，它们在内存中是紧挨着的，此时需要对`s1`进行`append`，但因为粗心忘记对`s1`扩容，这样就会导致`s2`的数据被覆盖；而SDS则不会出现这个问题，因为SDS在执行函数时会先判断它的空间是否允许修改，不允许的话会先扩容
5.  减少修改字符串时导致的内存重分配次数：C语言字符串总是要实现一个`N+1`的char型数组，因此当修改字符串时会可能发生内存重分配，这样会增加耗时，而SDS通过以下两点减少了内存重分配：
    -   空间预分配：简单来说，当SDS修改后，会预先分配一些额外的空间，使用`len`记录
    -   惰性空间释放：简单来说，当SDS缩短后，不会立即释放多余内存，而是使用`free`记录，当有需要时再使用

### 2、链表

#### 结构

链表节点

```c
typedef struct listNode {
    // 前置节点
    struct listNode * prev;
    // 后置节点
    struct listNode * next;
    //节点的值
    void * value;
}listNode;
```

链表结构

```c
typedef struct list {
    //
    表头节点
    listNode * head;
    //
    表尾节点
    listNode * tail;
    //
    链表所包含的节点数量
    unsigned long len;
    //
    节点值复制函数
    void *(*dup)(void *ptr);
    //
    节点值释放函数
    void (*free)(void *ptr);
    //
    节点值对比函数
    int (*match)(void *ptr,void *key);
} list;
```

>   可能对上面三个函数会比较懵，实际上上面三个函数都是函数指针，因为C语言中没有类这个概念，自然是不能在结构体里面放函数的，因此用函数指针来代替；实际行把它理解为类中的方法接口

![img](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306260011154.png)

#### 特点

链表结构很简单，就是一个双向链表，表头跟表尾都指向NULL，因此无环；另外因为记录了表头和表尾指针，获取的时间复杂度都是O(1)，获取链表节点的数量也是O(1)

### 3、字典 / 哈希表

#### 结构

哈表表结构

```c
typedef struct dictht {
    //哈希表数组
    dictEntry **table;
    //哈希表大小
    unsigned long size;  
    //哈希表大小掩码，用于计算索引值
    unsigned long sizemask;
    //该哈希表已有的节点数量
    unsigned long used;
} dictht;
```

哈希表节点

```c
typedef struct dictEntry {
    //键值对中的键
    void *key;
  
    //键值对中的值
    union {
        void *val; //相当于Java中的Object
        uint64_t u64; //64位无符号整数
        int64_t s64; //64位有符号整数
        double d;
    } v;
    //指向下一个哈希表节点，形成链表
    struct dictEntry *next;
} dictEntry;
```

![整体哈希表结构](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306260028987.png)

#### 哈希冲突

采用链地址法解决哈希冲突，即当索引值相同时，会组成链表，且为了性能考虑，会**增加在链表表头**

#### rehash 

实际上，Redis并不是单纯采用`dictht`结构来存储哈希表，为了让哈希表的负载因子维持在合理范围内，Redis采用了两个`dictht`结构来实现哈希

```c
typedef struct dict {
    …
    //两个Hash表，交替使用，用于rehash操作
    dictht ht[2]; 
    …
} dict;
```

![真正的哈希结构](https://cdn.jsdelivr.net/gh/HoShum/PictureRepo/imgs/202306260036541.png)

简单说明一下，rehash就是重新散列的意思，因为如果当哈希表的数据过多，就会导致哈希频繁发生哈希冲突，严重降低哈希表的性能（严重时甚至会退化到O(n))，因此rehash就是当数据过多时，对哈希表进行扩容，并重新计算它们的哈希值，再复制到新的哈希表中，数据过少时同理。

rehash结束后，会把`h[1]`设置为`h[0]`，并为`h[1]`新创建一个空白哈希表，为下一次rehash做准备

#### 渐进式rehash

所谓的渐进式，指的是rehash这个过程并不是一次完成的，而是分开多次完成的，原因是如果哈希表存储的数据很多时，一次完成可能会使服务器宕机

具体来说，哈希表会维持一个索引值，每次rehash都会将该索引值下的所有数据迁移到另一个哈希表中，当迁移后，索引值+1

在这个过程中，对哈希表的查询、修改、删除都会同时发生在两个哈希表中，比如先查哈希表0，没有再查哈希表1，而增加则只会在哈希表1增加，这种采用了`分而治之`思想的设计，很好地分摊了两个哈希表的压力

### 4、跳跃表

### 5、整数集合

### 6、压缩列表

## 对象