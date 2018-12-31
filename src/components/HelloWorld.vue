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
		emoji_list: ['(⌒▽⌒)', '（￣▽￣）', '(=・ω・=)', '(｀・ω・´)', '(〜￣△￣)〜', '(･∀･)', '(￣3￣)', '╮(￣▽￣)╭', '( ´_ゝ｀)', '←_←', '→_→', '(<_<)', '(>_>)', '(;¬_¬)', '("▔□▔)/', '(ﾟДﾟ≡ﾟдﾟ)!?', 'Σ(ﾟдﾟ;)', '(｡･ω･｡)', '(´･_･`)', '（￣へ￣）', '(╯°口°)╯(┴—┴', '_(:3」∠)_'],
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

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
