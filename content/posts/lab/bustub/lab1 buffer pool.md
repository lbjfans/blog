---
share: true
title: cmu15445--lab1 buffer pool
date: 2024-08-12T14:41:00+08:00
tags:
  - lab
  - database
  - "#bustub"
  - bustub
dir: posts/lab/bustub/
summary: cmu15445 lab1 buffer pool
---
# 复习理论

buffer pool：算子需要从disk获取数据时，可以先把数据放入buffer中，从而减少下次获取数据的时间开销
DBMS无法控制OS的pagecache，可能出现常用页被换出的情况，会导致性能下降
DBMS可以对大量数据读写，增加顺序IO，从而减少IO的开销
可以同时使用buffer和OS的小部分缓冲区，这样不影响其他进程的情况下增加DBMS的查询性能


LRU（最近最少使用）
buffer > disk，当buffer满了，需要替换页时，常用的算法
- [146. LRU 缓存 - 力扣（LeetCode）](https://leetcode.cn/problems/lru-cache/)
- 可以使用hash + 双链表实现
如：对于全表查询的场景，每个页只访问一次，其实没有必要放到buffer中，这样可能会把之前有用的页刷出，造成**缓存污染**


LRU-K
当页访问次数到达K次，才把页放入buffer中；维护两个队列，未到k次的一个，到达k次的一个，前者使用任意替换算法（如FIFO），后者使用LRU替换
- 优点：相比较LRU，减少了缓存污染问题，增加了buffer命中率
- 缺点：需要维护未进入buffer的数据，内存开销更大

2Q（LRU-2）
使用FIFO+LRU，且k设置为2
- 实际应用中LRU-2是综合各种因素后最优的选择（减少使用LRU的偶然、单次访问的缓存污染问题，维护开销不大），LRU-3或者更大的K值命中率会高，但适应性差，需要大量的数据访问才能将历史访问记录清除掉（如对于热点数据少的场景不适用，且内存开销大）

# 面经

1. 介绍一下LRU，LRU-K，优缺点
2. lru-k 比 lru 好在哪
3. k怎么选择？依据？
4. 为什么要自己做缓存池，操作系统不是有pagecache吗？
5. fsync出现卡顿怎么处理 ？
6. 介绍一下其他的置换算法
	- OPT, FIFO, LFU 
7. 手撕LRU, LFU

- LRU-K算法的应用场景？
- LRU-K算法相对于LRU算法的优势有哪些？
- 缓冲池的执行流程
- Page Cache的执行流程
	- [文件系统（三）IO类型、page(buffer)cache (yuque.com)](https://www.yuque.com/huiyizenmoqian/xfg28q/xvosrd)
- 什么情况下可以用O_Direct
	- 如DBMS已经缓存页，没必要让OS再次缓存
- 如果页的大小为16KB，而磁盘只保证4KB的读写是原子的，刷脏页的时候只刷了部分页进程就崩溃了，怎么处理？

# 开始lab1

[Project #1 - Buffer Pool | CMU 15-445/645 :: Intro to Database Systems (Spring 2023)](https://15445.courses.cs.cmu.edu/spring2023/project1/#lru-k-replacer)

- LRU-K Replacement Policy
- Buffer Pool Manager
- Read/Write Page Guards

# LRU-K

实现思路：从test入手，跟着头文件要求写

使用hash，两个双链表实现：历史队列用FIFO，缓存队列用LRU
- 相比于直接使用时间戳，对于需要频繁更新的场景，时间复杂度更低
还需要两个哈希表记录是否到达K次，是否可以驱逐
每个函数都上一把大锁

## 疑问

c++ list erase reverse_iterator
[反向迭代器删除元素_std::list删除支持反向迭代器吗-CSDN博客](https://blog.csdn.net/u011391040/article/details/50433237)
[c++ - How to call erase with a reverse iterator - Stack Overflow](https://stackoverflow.com/questions/1830158/how-to-call-erase-with-a-reverse-iterator)
```cpp
v.erase(std::next(it).base());
```

assert和static_cast
```cpp
#define BUSTUB_ASSERT(expr, message) assert((expr) && (message))
BUSTUB_ASSERT(frame_id <= static_cast<int>(replacer_size_), "in lruk: Remove");
```


总结锁
```cpp
std::lock_guard<std::mutex>guard(latch_);
```

类的protected

make_unique

write_page里：
std::scoped_lock scoped_db_io_latch(db_io_latch_);

左值引用：改名字
auto &a = v[idx]

buffer_pool_test的测试样例: random

noexcept
[一文入魂：妈妈再也不担心我不懂C++移动语义了 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/455848360)

## 测试

- `LRUKReplacer`: test/buffer/lru_k_replacer_test.cpp
- `BufferPoolManager`: test/buffer/buffer_pool_manager_test.cpp
- `PageGuard`: test/storage/page_guard_test.cpp

```
// 本地测试样例没有给remove，可以自己补充

make lru_k_replacer_test -j$(nproc)
./test/lru_k_replacer_test

make buffer_pool_manager_test -j$(nproc)
./test/buffer_pool_manager_test

 make page_guard_test -j$(nproc)
./test/page_guard_test
```

自己写的test
- lru_k：remove
```cpp
TEST(LRUKReplacerTest, SelfTest1) {  // 样例测试：remove
  LRUKReplacer lru_replacer(7, 2);

  // Scenario: add six elements to the replacer. We have [1,2,3,4,5]. Frame 6 is non-evictable.
  lru_replacer.RecordAccess(1);
  lru_replacer.RecordAccess(2);
  lru_replacer.RecordAccess(3);
  lru_replacer.RecordAccess(4);
  lru_replacer.RecordAccess(5);
  lru_replacer.RecordAccess(6);
  lru_replacer.SetEvictable(1, true);
  lru_replacer.SetEvictable(2, true);
  lru_replacer.SetEvictable(3, true);
  lru_replacer.SetEvictable(4, true);
  lru_replacer.SetEvictable(5, true);
  lru_replacer.SetEvictable(6, false);
  ASSERT_EQ(5, lru_replacer.Size());

  // Scenario: Insert access history for frame 1. Now frame 1 has two access histories.
  // All other frames have max backward k-dist. The order of eviction is [2,3,4,5,1].
  lru_replacer.RecordAccess(1);

  // Scenario: Evict three pages from the replacer. Elements with max k-distance should be popped
  // first based on LRU.
  int value;
  lru_replacer.Remove(3);
  lru_replacer.Evict(&value);
  ASSERT_EQ(2, value);
  lru_replacer.Evict(&value);
  ASSERT_EQ(4, value);
  ASSERT_EQ(2, lru_replacer.Size());
}
```
- page guard: read
```cpp
TEST(PageGuardTest, SampleTest) {  // 加入：move test
  const std::string db_name = "test.db";
  const size_t buffer_pool_size = 5;
  const size_t k = 2;

  auto disk_manager = std::make_shared<DiskManagerUnlimitedMemory>();
  auto bpm = std::make_shared<BufferPoolManager>(buffer_pool_size, disk_manager.get(), k);

  page_id_t page_id_temp;
  auto *page0 = bpm->NewPage(&page_id_temp);

  auto guarded_page = BasicPageGuard(bpm.get(), page0);

  EXPECT_EQ(page0->GetData(), guarded_page.GetData());
  EXPECT_EQ(page0->GetPageId(), guarded_page.PageId());
  EXPECT_EQ(1, page0->GetPinCount());

  // move test
  auto guard_page1 = std::move(guarded_page);
  EXPECT_EQ(1, page0->GetPinCount());
  // guarded_page.Drop();
  guard_page1.Drop();
  EXPECT_EQ(0, page0->GetPinCount());

  // Shutdown the disk manager and remove the temporary file we created.
  disk_manager->ShutDown();
}


// NOLINTNEXTLINE
TEST(PageGuardTest, MyReadTest) {  // read test
  const std::string db_name = "test.db";
  const size_t buffer_pool_size = 5;
  const size_t k = 2;

  auto disk_manager = std::make_shared<DiskManagerUnlimitedMemory>();
  auto bpm = std::make_shared<BufferPoolManager>(buffer_pool_size, disk_manager.get(), k);

  page_id_t page_id_temp;
  auto *page0 = bpm->NewPage(&page_id_temp);
  EXPECT_EQ(1, page0->GetPinCount());
  // read test: Drop()
  {
    auto read_guard = bpm->FetchPageRead(page_id_temp);
    EXPECT_EQ(2, page0->GetPinCount());
  }
  EXPECT_EQ(1, page0->GetPinCount());

  auto guarded_page = BasicPageGuard(bpm.get(), page0);
  // read test: ReadPageGuard(ReadPageGuard &&that)
  {
    auto reader_guard = bpm->FetchPageRead(page_id_temp);
    auto reader_guard_2 = ReadPageGuard(std::move(reader_guard));
    EXPECT_EQ(2, page0->GetPinCount());
  }
  EXPECT_EQ(1, page0->GetPinCount());
  // ReadPageGuard::operator=(ReadPageGuard &&that)
  {
    auto reader_guard_1 = bpm->FetchPageRead(page_id_temp);
    auto reader_guard_2 = bpm->FetchPageRead(page_id_temp);
    EXPECT_EQ(3, page0->GetPinCount());
    reader_guard_1 = std::move(reader_guard_2);
    EXPECT_EQ(2, page0->GetPinCount());
  }
  EXPECT_EQ(1, page0->GetPinCount());

  // Shutdown the disk manager and remove the temporary file we created.
  disk_manager->ShutDown();
}
```

# BUFFER POOL

buffer逻辑：
算子需要page_id > buffer pool > disk
- buffer pool有空位(free_list_)，直接从disk获取到buffer
- buffer没有空位，从replacer替换一个页

实现思路：按照test和头文件实现

free_list：记录空的页框(framd_id)
replacer_：使用实现的LRU-K
page_table_：将算子所需要的page_id对应到buffer的页框中

# Read/Write Page Guards

需要了解c++智能指针的简单实现
- [C++11 智能指针【详解+实现】【面试常考】_智能指针的底层实现-CSDN博客](https://blog.csdn.net/qq_39384184/article/details/123853862)

实现智能指针
- basic：自动unpin
- read/write：自动释放锁


# 优化

第一次提交
![](/blog/images/Pasted%20image%2020240815162624.png)

总结上面的内容

学一下perf和profiling
[CMU 15-445 2023 P1 优化攻略 [rank#3] - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/644160340)

```math
scan_qps_0ms / 10000 + get_qps_0ms / 10000 + scan_qps_1ms + get_qps_1ms * 10
```
优化思路：做完2023fall再优化
- 最大的性能瓶颈是随机读
- diskmanager：现在刷脏时需要等待IO结束才解锁，可以将IO和buffer的其他操作分开（异步IO）

# 上传

```git
// 创建写的分支
git checkout -b lru_k
// 写完一部分后提交
Git add .
Git commit -m 'first p1'
git checkout buffer_pool
git merge lru_k
git push origin buffer_pool:buffer_pool

```

