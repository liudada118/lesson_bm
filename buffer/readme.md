### Buffer是一个类似于数组的对象 需要实例化

### Buffer对象的内存分配不是在v8的对内存中，而是在node的c++层面实现内存的申请的
    因为处理大量的字节数据不能采用需要一点内存就向操作系统申请一点内存的方式，这可能造成大量的内存申请的系统调用，对操作系统有一定压力。为此node在内存的使用上应用的是在c++层面申请内存、在JavaScript中分配内存的策略

    为了能高效地使用申请来的内存，node采用slab分配机制。slab是一种动态内存管理机制

简而言之slab是一块申请好的固定大小的内存区域，slab具有三种状态

- full ： 完全分配状态
- partial ：部分分配状态
- empty ： 没有被分配状态

#### Buffer也有大小之分，8kb是它的区分临界值

    例如： new Buffer(size)
    Node以8kb为界限来区分Buffer是大对象还是小对象：
    Buffer.poolsize= 8*1024;
    这个8kb的值就是每个slab的值，在JavaScript中，以它作为单位单元进行内存分配

    