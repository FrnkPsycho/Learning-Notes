# cpp 函数用法

Created: 2021-11-22 14:57:37 +0800

Modified: 2021-11-23 19:26:06 +0800

---


vector 删除重复元素

删除重复元素，首先将vector排序。

sort( vecSrc.begin(), vecSrc.end() );

然后使用unique算法。

vecSrc.erase( unique( vecSrc.begin(), vecSrc.end() ), vecSrc.end() );





去掉字符串开头0

erase方法 遍历字符串 不为0时break



reverse(str.begin(),str.end()) 直接对str进行反转



四舍五入可以用 round()

涉及到小数点的四舍五入可以先乘上10的倍数四舍五入后再除回去



sprintf(arr,"%",var);
