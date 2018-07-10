## Python 对象赋值
Python "一切皆对象” 。对象的赋值都是进行对象引用（内存地址）传递，修改其中一个对象里面的元素，另外一个的内容也改变（内存地址传递，若对象里的元素地址改变，另外对象的地址也会改变）。

[原文作者：田小计划](http://www.cnblogs.com/wilber2013/)
#### 对象赋值，观察被赋值对象和赋值对象的地址和值。 
```Python
list1 = ["orig_str", 28, ["Python", "C#"]]
print("id(list1): ",id(list1))
print ("id(ele): ",[id(ele) for ele in list1])
print("")
list2 = list1
print("对象复制后：")
print("id(list2): ",id(list2))
print ("id(ele): ",[id(ele) for ele in list2])
```
>id(list1):  2202310956040
>id(ele):  [2202310956720, 1698694480, 2202310349576] 
>对象复制后：
>id(list2):  2202310956040
>id(ele):  [2202310956720, 1698694480, 2202310349576]


####  修改原对象里面的元素，再次观察
```Python
list1[0] = "changed_str" 
list1[2].append("append C++")
#换成list2改变，结果一样
print("对被复值的对象进行修改后：")
print("list1",list1)
print("list2",list2)

print("")
print("id(list1): ",id(list1))
print ("id(ele): ",[id(ele) for ele in list1])
print("")
print("id(list2): ",id(list2))
print ("id(ele): ",[id(ele) for ele in list2])
```

>对被复值的对象进行修改后：
>list1 ['changed_str', 28, ['Python', 'C#', 'append C++']]
>list2 ['changed_str', 28, ['Python', 'C#', 'append C++']]

>id(list1):  2202310956040
>id(ele):  [2202310943984, 1698694480, 2202310349576]

>id(list2):  2202310956040
>id(ele):  [2202310943984, 1698694480, 2202310349576]
注意：list里面的str改变，会产生新的地址。由orig_str到changed_str地址由2202310956720改变为2202310943984。



## Python 浅拷贝
* 通过copy模块里面的浅拷贝函数copy()创建一个新的对象（分配新的内存地址）
* 但是，对于对象中的元素，浅拷贝就只会使用原始元素的引用（内存地址），也就是说"list2[i] is list1[i]"(是“is”而不是“==”,等于只是值相同，内存地址不同）
当对will进行修改的时候
* 由于这里列表类型的第一个元素str类型是不可变类型，所以当list1的str修改，产生新的对象，则List2的str对象和list1的str对象不再是一个，则修改list的str与list2的str完全无关。
但是list的第三个元素是一个可不变类型，修改操作不会产生新的对象，list1和list2的第三元素是指向同一地址是相同对象，当然修改其中一个会改变另外一个了。

### 总结一下，当我们使用下面的操作的时候，会产生浅拷贝的效果：
* 使用切片[:]操作
* 使用工厂函数（如list/dir/set）
* 使用copy模块中的copy()函数

#### 对象浅拷贝，观察被拷贝对象和拷贝目标对象的地址和值。 
```Python
import copy

list1 = ["orig_str", 28, ["Python", "C#"]]
print("list1",list1)
print("id(list1): ",id(list1))
print ("id(ele): ",[id(ele) for ele in list1])
print("")
list2 = copy.copy(list1)
print("对象浅拷贝后：")
print("list2",list2)
print("id(list2): ",id(list2))
print ("id(ele): ",[id(ele) for ele in list2])
```
>list1 ['orig_str', 28, ['Python', 'C#']]
>id(list1):  2202310957704
>id(ele):  [2202310956144, 1698694480, 2202310955720]

>对象浅拷贝后：
>list2 ['orig_str', 28, ['Python', 'C#']]
>id(list2):  2202310951304
>id(ele):  [2202310956144, 1698694480, 2202310955720]

####  修改被拷贝对象（原对象）里面的元素，再次观察
```Python
list1[0] = "changed_str"
list1[2].append("append C++")

print("对被浅拷贝的对象进行修改后：")
print("list1",list1)
print("list2",list2)

print("")
print("id(list1): ",id(list1))
print ("id(ele): ",[id(ele) for ele in list1])
print("")
print("id(list2): ",id(list2))
print ("id(ele): ",[id(ele) for ele in list2])
```
>对被浅拷贝的对象进行修改后：
>list1 ['changed_str', 28, ['Python', 'C#', 'append C++']]
>list2 ['orig_str', 28, ['Python', 'C#', 'append C++']]

>id(list1):  2202310957704
>id(ele):  [2202310957616, 1698694480, 2202310955720]

>id(list2):  2202310951304
>id(ele):  [2202310956144, 1698694480, 2202310955720]

[对象赋值](#Python 对象赋值)
[浅拷贝](#Python 浅拷贝)
[深拷贝](#Python 深拷贝)
[全文总结](# 全文总结)

## Python 深拷贝

* 跟浅拷贝类似，深拷贝也会创建一个新的对象
* 但是，对于对象中的元素，深拷贝都会重新生成一份（有特殊情况，下面会说明），而不是简单的使用原始元素的引用（内存地址） 
list1和list2的第三个元素地址不同（值变化自然也不相关），这里str对象和数字对象特殊，地址没改变，后面介绍。

* 当对list1进行修改的时候
* 列表的第一个元素str对象是特殊元素，str对象没修改前，List1和List2的str指向同一内存，是同一对象，
但是由于str对象是不可变类型，改变list1的第一个元素产生新的对象，则list1，list2的第一个元素无关了，变化自然不相关。
* 但是，尽管列表的第三元素是可变类型，修改操作不会产生新的对像，这在浅拷贝变化是相同的。可是啊，list1和list2的第三个元素早在list2深拷贝产生新的对象时候就不是一个对象，变化不相关。

### 拷贝的特殊情况：
* 对于非容器类型（如数字、字符串、和其他'原子'类型的对象）没有拷贝这一说
也就是说，对于这些类型，"obj is copy.copy(obj)" 、"obj is copy.deepcopy(obj)"
* 如果元祖变量只包含原子类型对象，则不能深拷贝，看下面的例子
![image](https://user-images.githubusercontent.com/32263576/40785291-b967c8f2-651a-11e8-9078-b5c09ba9f28b.png)

```Python
import copy
list1 = ["orig_str", 28, ["Python", "C#"]]
print("list1",list1)
print("id(list1): ",id(list1))
print ("id(ele): ",[id(ele) for ele in list1])
print("")
list2 = copy.deepcopy(list1)
print("对象深拷贝后：")
print("list2",list2)
print("id(list2): ",id(list2))
print ("id(ele): ",[idlist1 ['orig_str', 28, ['Python', 'C#']]
```
id(list1):  2202310944648
id(ele):  [2202310956144, 1698694480, 2202310954952]
>对象深拷贝后：
>list2 ['orig_str', 28, ['Python', 'C#']]
>id(list2):  2202309987208
>id(ele):  [2202310956144, 1698694480, 2202310951432](ele) for ele in list2])

####  修改被深拷贝对象（原对象）里面的元素，再次观察
```Python
list1[0] = "changed_str"
list1[2].append("append C++")

print("对被深拷贝的对象进行修改后：")
print("list1",list1)
print("list2",list2)

print("")
print("id(list1): ",id(list1))
print ("id(ele): ",[id(ele) for ele in list1])
print("")
print("id(list2): ",id(list2))
print ("id(ele): ",[id(ele) for ele in list2])
```
>对被深拷贝的对象进行修改后：
>list1 ['changed_str', 28, ['Python', 'C#', 'append C++']]
>list2 ['orig_str', 28, ['Python', 'C#']]

>id(list1):  2202310944648
>id(ele):  [2202310945904, 1698694480, 2202310954952]

>id(list2):  2202309987208
>id(ele):  [2202310956144, 1698694480, 2202310951432]


## 全文总结
本文介绍了对象的赋值和拷贝，以及它们之间的差异：

* Python中对象的赋值都是进行对象引用（内存地址）传递
* 使用copy.copy()，可以进行对象的浅拷贝，它复制了对象，但对于对象中的元素，依然使用原始的引用.
* 如果需要复制一个容器对象，以及它里面的所有元素（包含元素的子元素），可以使用copy.deepcopy()进行深拷贝
* 对于非容器类型（如数字、字符串、和其他'原子'类型的对象）没有被拷贝一说
* 如果元祖变量只包含原子类型对象，则不能深拷贝


