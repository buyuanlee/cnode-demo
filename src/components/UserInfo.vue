<template>
  <div class="user_info">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <main v-else>
      <!--主页部分-->
      <p class="section_header">主页</p>
      <section class="mainpage">
        <img class="avatar" :src="userinfo.avatar_url" alt="">
        <span class="loginname">{{userinfo.loginname}}</span>
        <p class="score">{{userinfo.score}}积分</p>
        <svg class="icon" aria-hidden="true">
          <use xlink:href="#icon-github"></use>
        </svg>
        <a :href="'https://github.com/' + userinfo.githubUsername">@{{userinfo.githubUsername}}</a>
        <p class="registration_time">注册时间：{{userinfo.create_at|formatDate}}</p>
      </section>
      <!--创建话题部分-->
      <p class="section_header">最近创建的话题</p>
      <section>
        <ul>
          <li v-for="item in topiclimit">
            <router-link :to="{
          name:'user_info',
          params:{
            name:item.author.loginname,
          }
          }">
              <img class="avatar" :src="item.author.avatar_url">
            </router-link>
            <router-link :to="{
            name:'post_content',
            params:{
            id:item.id,
            name:item.author.loginname

            }}">
              {{item.title}}
            </router-link>
          </li>
          <a class="more" :href="'https://cnodejs.org/user/'+this.userinfo.loginname+'/topics'">查看更多>>></a>
        </ul>
      </section>
      <!--参与话题部分-->
      <p class="section_header">最近参与的话题</p>
      <section>
        <ul>
          <li v-for="item in replylimit">
            <router-link :to="{
          name:'user_info',
          params:{
            name:item.author.loginname
          }
          }">
              <img class="avatar" :src="item.author.avatar_url">
            </router-link>
            <router-link :to="{
            name:'post_content',
            params:{
            id:item.id,
            name:item.author.loginname
            }}">
              {{item.title}}
            </router-link>
          </li>
          <a class="more" :href="'https://cnodejs.org/user/'+this.userinfo.loginname+'/replies'">查看更多>>></a>
        </ul>
      </section>
    </main>
  </div>
</template>

<script>
  export default {
    name: "UserInfo",
    data() {
      return {
        isLoading: false,
        userinfo: {},
        posts: [],
      }
    },
    methods: {
      getData() {
        this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
          .then(res => {
            this.isLoading = false
            this.userinfo = res.data.data
          })
          .catch(err => {
            console.log(err)
          })
      },
    },
    computed: {
      topiclimit() {
        if (this.userinfo.recent_topics) {
          return this.userinfo.recent_topics.slice(0, 5)
        } else {
          return this.userinfo.recent_topics
        }
      },
      replylimit() {
        if (this.userinfo.recent_replies) {
          return this.userinfo.recent_replies.slice(0, 5)
        } else {
          return this.userinfo.recent_replies
        }
      }
    },
    beforeMount() {
      this.isLoading = true;
      this.getData();
    },
    watch: {
      '$route'(to, from) {
        this.getData();
      }
    }
  }
</script>

<style scoped>
  .user_info {
    margin: 0 auto;
    width: 80%;
  }

  section {
    margin-bottom: 10px;
    background-color: #fff;
    border-radius: 0 0 3px 3px;
  }

  svg, a {
    color: #777777;
    vertical-align: top;
  }

  svg:hover, a:hover {
    color: #75b403;
  }

  a:visited {
    text-decoration: underline;

  }

  li {
    border-bottom: 1px solid #e1e1e1;
    padding: 10px 0 0 15px;
  }

  .mainpage {
    font-size: 14px;
    color: #777777;
    padding: 15px;
  }

  .avatar {
    width: 40px;
    height: 40px;
    margin-right: 5px;
  }

  .loginname {
    font-size: 14px;
    color: #777777;
    vertical-align: top;
  }

  .section_header {
    background-color: #f6f6f6;
    border-radius: 3px 3px 0 0;
    color: #75b403;
    padding: 10px;
    font-size: 14px;
    margin-bottom: 1px;
  }

  .more {
    display: inline-block;
    padding: 10px;
  }

</style>
