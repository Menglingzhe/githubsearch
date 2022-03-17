<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">查找 Github 用户</h3>
    <div>
      <input
        type="text"
        placeholder="enter the name you search"
        v-model="keyWord"
      />&nbsp;
      <button @click="searchUser">查询</button>
    </div>
  </section>
</template>

<script>
import pubsub from "pubsub-js";
import axios from "axios";
export default {
  name: "Search",
  data() {
    return {
      keyWord: "",
    };
  },
  methods:{
	  searchUser(){
		  pubsub.publish('updateListData',{isLoading:true,errMsg:'',users:[],isFirst:false})
      	// this.$bus.$emit('updateListData',{isLoading:true,errMsg:'',users:[],isFirst:false})
      
		  axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
			  response=>{
				  console.log('请求成功')
          //  console.log({isLoading:false,errMsg:'',users:response.data.items})
          
				  pubsub.publish('updateListData',{isLoading:false,errMsg:'',users:response.data.items})
          // pubsub.publish('updateListData','发送数据')

			  },error=>{
				  console.log('请求失败')
				  pubsub.publish('updateListData',{isLoading:false,errMsg:error.message,users:[]})
			  }
		  )
      //全局事件总线玩法
      // axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
			// 		response => {
			// 			console.log('请求成功了')
			// 			//请求成功后更新List的数据
			// 			this.$bus.$emit('updateListData',{isLoading:false,errMsg:'',users:response.data.items})
			// 		},
			// 		error => {
			// 			//请求后更新List的数据
			// 			this.$bus.$emit('updateListData',{isLoading:false,errMsg:error.message,users:[]})
			// 		}
			// 	)
	  }
  }
};
</script>
