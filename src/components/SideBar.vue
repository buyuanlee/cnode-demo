<template>
  <div class="sidebar">
    <!--作者信息-->
    <p class="header">作者</p>
    <section class="authorsummary">
      <router-link :to="{
      name:'user_info',
      params:{
      name:userinfo.loginname
      }
      }">
        <img class="avatar" :src="userinfo.avatar_url">
        <span class="loginname">{{userinfo.loginname}}</span>
      </router-link>
      <p>积分：{{userinfo.score}}</p>
    </section>
    <!--最近话题-->
    <p class="header">作者最近其他话题</p>
    <section class="recent_topics">
      <ul>
        <li v-for="item in topiclimit">
          <router-link :to="{
            name:'post_content',
            params: {
            id:item.id,
            name:item.author.loginname
            }
          }">
            {{item.title}}
          </router-link>
        </li>
      </ul>
    </section>
    <!--最近回复-->
    <p class="header">作者最近参与话题</p>
    <section class="recent_replies">
      <ul>
        <li v-for="item in replylimit">
          <router-link :to="{
            name:'post_content',
            params: {
            id:item.id,
            name:item.author.loginname
            }
          }">
            {{item.title}}
          </router-link>
        </li>
      </ul>
    </section>
  </div>
</template>

<script>
  export default {
    name: "SideBar",
    data() {
      return {
        userinfo: {}
      }
    },
    methods: {
      getData() {
        this.$http.get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
          .then(res => {
            this.userinfo = res.data.data
          })
          .catch(err => {
            console.log(err)
          })
      }
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
      this.getData()
    }
  }
</script>

<style scoped>
  section {
    background-color: #fff;
    border-radius: 0 0 3px 3px;
    margin: 1px 0 10px 0;
    font-size: 14px;
    color: #777777;
    padding: 10px;
  }

  a {
    color: #777777;
    text-decoration: none;
  }

  a:hover {
    color: #4183C4;
  }

  li {
    margin: 5px;
  }

  .header {
    margin: 0;
    border-radius: 3px 3px 0 0;
    background-color: #F6F6F6;
    font-size: 14px;
    padding: 5px;
  }

  .sidebar {
    width: 20%;
    margin: 0 auto;
  }

  .avatar {
    width: 40px;
    height: 40px;
    border-radius: 3px;
  }

  .loginname {
    vertical-align: top;

  }


</style>
