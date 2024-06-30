# 从这里开始吧
#### 2024.6.30 广州

通过 &s1 语法，我们创建了一个指向 s1 的引用，但是并不拥有它。因为并不拥有这个值，当引用离开作用域后，其指向的值也不会被丢弃。

同理，函数 calculate_length 使用 & 来表明参数 s 的类型是一个引用：

fn calculate_length(s: &String) -> usize { // s 是对 String 的引用
    s.len()
} // 这里，s 离开了作用域。但因为它并不拥有引用值的所有权，
  // 所以什么也不会发生
人总是贪心的，可以拉女孩小手了，就想着抱抱柔软的身子（读者中的某老司机表示，这个流程完全不对），因此光借用已经满足不了我们了，如果尝试修改借用的变量呢？

fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}
很不幸，妹子你没抱到，哦口误，你修改错了：

error[E0596]: cannot borrow `*some_string` as mutable, as it is behind a `&` reference
 --> src/main.rs:8:5
  |
7 | fn change(some_string: &String) {
  |                        ------- help: consider changing this to be a mutable reference: `&mut String`
                           ------- 帮助：考虑将该参数类型修改为可变的引用: `&mut String`
8 |     some_string.push_str(", world");
  |     ^^^^^^^^^^^ `some_string` is a `&` reference, so the data it refers to cannot be borrowed as mutable
                     `some_string`是一个`&`类型的引用，因此它指向的数据无法进行修改
正如变量默认不可变一样，引用指向的值默认也是不可变的，没事，来一起看看如何解决这个问题。

可变引用
只需要一个小调整，即可修复上面代码的错误：


fn main() {
    let mut s = String::from("hello");

    change(&mut s);