# 链表操作

- - -

## 链表生成： 
* list = [x for x in range(1,10)];
* list = [x +y for x in 'ABCDE' for y in '123456']

## 链表操作：
### 链表相加： 
* list += list1           # 链表相加
* list.append(x)          # 链表尾部增加内容
* list.remove(x)          # 删除元素，如果元素不存在则会抛出异常
* list.clear()            # 清空链表
* list.insert(index, x)   # 在index位置插入元素    
### 链表切片： 
* list = list1[1,4]       # 切取部分
* list = list1[:]         # 切取全部
* list = list1[-3:-1]     # 尾部倒序切取
* list = list1[::-1]      # 反转链表
### 链表排序：
* sorted(list)            # 普通排序
* sorted(list, reverse=True)  # 反转排序
* sorted(list, key=len)   # 根据元素的长度排序

***

## 集合操作：
### 集合生成：
* set = {1,2 ....}
* set = set(list)
* set = set(range(1, 9))
### 集合操作：
* set.update([1,2,x])         # 增加元素
* set.discard(x)              # 去除一个元素，没有无异常
* set.remove(x)               # 删除元素，没有抛出异常
* set.pop()                   # 返回并删除set顶部元素
* set1 & set2                 # 两个集合的并集
* set | set2                  # 两个集合的合集
* set - set2                  # 两个集合的差集
* set <= set2                 # set2是否是set的超集

***

## 字典操作：
### 字典生成：
* scores = {'张三':77, '李四':99,'张三':42}
### 字典操作：
* scores['张三']
* scores.update(王五=88, 元芳=98)  # 插入元素
* scores.get('张三', 60)      # 获取元素，不存在则使用默认值
* scores.popitem()            # 删除栈顶元素
* scores.pop('张三', 100)     # 删除指定元素并返回，不存在使用默认值
* scores.clear()              # 清空链表

