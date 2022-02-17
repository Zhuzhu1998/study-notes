## 二分查找

二分查找是对有序数组查找特定元素的一种较为高效的查找方式。

**算法思想:**

不妨设有序数组是升序。比较当前区间中间元素与查找目标，如果中间元素是查找目标，则找到目标停止；如果目标大于中间元素，那么缩小区间，在大于中间元素的区间重复寻找过程；如果目标小于于中间元素，则在小于中间元素的区间重复寻找过程。重复上述过程直至区间长度为负数。

**Rust代码:**

```rust
pub fn binary_search(nums: &Vec<i32>, target_value: i32) -> Option<usize> {
    if nums.len() == 0{
        None
    }
    else{
        let mut left:i32 = 0;
        let mut right:i32 = nums.len() as i32 -1;
        while left <= right {
            let mid = (right - left) / 2 + left;
            let u_mid = mid as usize;
            let value = nums[u_mid];
            if value == target_value {
                return Some(u_mid);
            }
            else if value < target_value {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }
        None
    }
}
```


(~￣▽￣)~欸，等一下，`vec`的索引类型是`usize` ，`len()` 返回的也是 `usize`，那么在此处为什么还将 left 、right 和 mid 设计为`i32`呢？as 过去 as 回来的，怪麻烦的…  
(￣ε(#￣)☆╰╮o(￣皿￣///) 八嘎！就是这个问题让我浪费了半个小时！

如果这样的话，将收获同款报错：  
`thread 'main' panicked at 'attempt to subtract with overflow'`

也就是说，产生了下溢。

&#x1F330; 举个栗子 &#x1F330;

`nums = vec![1, 2]`
`target_value = 0`

| left | right | mid | value |
|:----: | :----: | :----: | :----: |
|0|1|0| 41|
|0|-1|—| —|

第二轮循环中，出现了 `0 -1`,  `usize` 肯定不允许呀~

_________________

参数类型:  
Vec的存储方式类似String, 没有实现`Copy` trait，如果直接移交所有权，那么该数组在执行完查找之后就释放掉了，所以在此我们仅仅“借用”一下。

_________________

返回值的处理:  
看这里(づ￣ ³￣)づ[match 控制流运算符](https://rustwiki.org/zh-CN/book/ch06-02-match.html)  
下面就简单打印一下。
```rust
pub fn display_search(nums: &Vec<i32>, target_value: i32) {
    match binary_search(nums, target_value) {
        None => println!("No such number"),
        Some(target_index) => {
            println!("We found number:{} at no. {}", target_value, target_index)
        },
    }
}
```

二分查找还有诸多变体，嘛~下次再说叭……


