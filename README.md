# 【注意】N年前写的，不会再维护。
# 简单优雅的Vue评论组件

## 主要功能

1.  支持颜表情`emoji`（╮(￣▽￣)╭）
2.  支持滑动验证。
3.  评论为空不允许提交。
4.  封装了几个常用的方法。

## 在线浏览

### 1、用户已登录

![](http://mardown-pic-1252666898.coscd.myqcloud.com/github/2018-12-vue-comment/had_login.gif)

### 2、用户未登录

![](http://mardown-pic-1252666898.coscd.myqcloud.com/github/2018-12-vue-comment/not_login.gif)

## 引入方法

### 1、引入element-ui

``` bash
1、 cnpm i element-ui -D
2、 在main.js配置。（也可以按需加载）
```

### 2、

```vue
<template>
    <b_comment 
		ref="my_comment" 
		:placeholder="placeholder"
		:if_not_logined="if_not_logined"
        :emoji_list="emoji_list"
		:verify_once="verify_once"
		@submit_click="submit_click"
	/>
</template>

<script>
import b_comment from './vue_comment/b_comment.vue'
export default {
  name: "HelloWorld",
	components: {
		'b_comment':b_comment,
	},
  data() {
    return {
		placeholder: "想说点什么？",//默认文字提示。
        if_not_logined: true,//用户是否没有登录。
	    //颜文字列表。
		emoji_list: ['(⌒▽⌒)', '（￣▽￣）', '(=・ω・=)'...],
		verify_once: false,//未登录时，每次评论都需输入验证码。
    };
  },
  computed:{
	  comment_text(){//获取子组件的评论内容。
		  return this.$refs.my_comment.insert_comment.comment_text;
	  },
	  comment_name(){//获取子组件的评论昵称。
		  return this.$refs.my_comment.insert_comment.comment_name;
	  }
  },
  methods:{
	  //点击评论按钮后，触发的事件。
	  //（在这之前会先检验是否为空、是否输入验证码）
	  submit_click(){
		  console.log("用户输入的评论内容是：" + this.comment_text);
		  if(this.comment_name !== ""){
		  		console.log("用户输入的昵称是：" + this.comment_name);
		  }

		  //你可以在这里验证用户输入的格式。
		  //若格式错误可调用此函数：
		  	//this.$refs.my_comment.err_submitFn("格式错误",1500)

		  //你可以在这儿请求AJAX
		  //失败回调：
			// this.$refs.my_comment.err_submitFn("评论失败",1500)
		  //成功回调
		    this.$refs.my_comment.success_submit("评论成功",1500)
	  }
  },
};
</script>
```

## 文档

### 1、属性

| 属性名           | 类型    | 描述                                 |
| ---------------- | ------- | ------------------------------------ |
| `placeholder`    | String  | 默认的文字提示。                     |
| `if_not_logined` | String  | 用户是否没有登录（未登录强制验证）。 |
| `emoji_list`     | Array   | 颜文字列表。                         |
| `verify_once`    | Boolean | 验证码是否只验证一次。               |
| `comment_text`   | String  | 评论内容。                           |
| `comment_name`   | String  | 评论昵称。                           |

### 2、方法

| 方法名                                                | 描述                                                         | 参数列表                                      |
| ----------------------------------------------------- | ------------------------------------------------------------ | --------------------------------------------- |
| submit_click                                          | 点击`评论`按钮，触发的函数。                                 | -                                             |
| this.$refs.my_comment.err_submitFn("评论失败",1500)   | 调用后，右下角显示错误提示。                                 | 1.提示内容，string格式。2.显示时间，int格式。 |
| this.$refs.my_comment.success_submit("评论成功",1500) | 调用后，右下角显示提示。并清除已输入内容。根据设置重置验证码组件。 | 同上                                          |

## 下载demo

``` bash
1. git clone 或者 手动下载。
2. cnpm install
3. npm run dev
4. 打开：localhost:8080
```

## 说明

文档 => 优雅，界面 => 简单。

但代码有很多改进的空间。。。有机会再优化吧。

有问题欢迎Issues。

我封装的第一个组件，欢迎star鼓励(=・ω・=)



p.s:有空的话，我会把（同样风格的）评论列表也封装成组件。

