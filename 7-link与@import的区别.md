## link与@import的区别

- link功能较多，可以定义RSS，定义Rel等作用，而@import只能用于加载css
- 当解析到link时，页面会同步加载所引用的css，而@import所引用的css会等到页面加载完才被加载
- @import需要IE5以上才能使用
- link可以使用js动态引入，@import不行

- @inport不推荐被使用