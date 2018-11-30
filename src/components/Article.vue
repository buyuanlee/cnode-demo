<template>
  <article>
    <!--加载动画-->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <!--显示内容-->
    <div v-else>
      <!--头部信息-->
      <header>
        <h3>{{post.title}}</h3>
        <ul>
          <li>• 发布于:{{post.create_at|formatDate}}</li>
          <li>• 作者:{{post.author.loginname}}</li>
          <li>• {{post.visit_count}}次浏览</li>
          <li>• 来自:{{post|tabFormatter}}</li>
        </ul>
      </header>
      <!--文章内容-->
      <main v-html="post.content" class="topic_content"></main>
      <!--回复内容-->
      <div class="reply">
        <span>{{post.reply_count}} 条回复</span>
        <div class="reply_content" v-for="(reply,index) in post.replies">
          <router-link :to="{
          name:'user_info',
          params:{
            name:reply.author.loginname
          }
          }">
            <img class="avatar" :src="reply.author.avatar_url">
            <span class="reply_name">{{reply.author.loginname}}</span>
          </router-link>
          <span class="reply_index">{{index+1}}楼•{{reply.create_at|formatDate}}</span>
          <span class="awesome" v-if="reply.ups.length>0">👍{{reply.ups.length}}</span>
          <p class="content" v-html="reply.content"></p>
        </div>
      </div>
    </div>
  </article>
</template>
<script>
  export default {
    name: "Article",
    data() {
      return {
        isLoading: false,
        //当前文章页的所有内容
        post: {}
      }
    },
    methods: {
      getArticle() {
        this.$http.get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
          .then(res => {
            if (res.data.success === true) {
              this.isLoading = false
              this.post = res.data.data
            }
          })
          .catch(err => {
            console.log(err)
          })
      }
    },
    beforeMount() {
      this.isLoading = true
      this.getArticle()
    },
    watch: {
      '$route'(to, from) {
        this.getArticle()
      }
    }
  }
</script>
<style>
  @import url('../assets/themes/github.css');

  .markdown-text p {
    margin: 0;
    padding: 0;
  }
</style>
<style scoped>

  article {
    width: 75%;
    margin: 0 auto;
  }

  header {
    background-color: #fff;
    border-radius: 3px 3px 0 0;
    padding: 20px 0;
  }

  main {
    margin-top: 1px;
    background-color: #fff;
    padding: 10px 20px;
    border-radius: 0 0 3px 3px;
  }

  h3 {
    margin: 0;
    padding: 10px 0 0 15px;
  }

  ul > li {
    display: inline-block;
    font-size: 12px;
    color: #838383;
    margin-right: 10px;
  }

  .reply {
    color: #444;
    font-size: 14px;
    background-color: #F6F6F6;
    margin-top: 10px;
    border-radius: 3px 3px 0 0;
  }

  .reply > span {
    display: inline-block;
    padding: 15px;
  }

  .reply_content {
    background-color: #fff;
    border-top: 1px solid #f0f0f0;
  }

  .avatar {
    width: 30px;
    height: 30px;
    border-radius: 3px;
    margin: 15px 0 0 15px;
  }

  .reply_name, .reply_index {
    vertical-align: top;

  }

  .reply_name {
    display: inline-block;
    font-weight: bolder;
    color: #666;
    margin: 15px 5px;
  }

  .reply_index {
    display: inline-block;
    font-size: 12px;
    margin: 15px 0;

  }

  .awesome {
    float: right;
    margin-right: 15px;
  }

  .content {
    margin-left: 60px;
  }
</style>
