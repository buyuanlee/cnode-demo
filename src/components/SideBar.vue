<template>
  <div class="sidebar">
    <!--作者信息-->
    <section class="authorsummary">
      <p>作者</p>
      <router-link :to="{
      name:'user_info',
      params:{
      name:userinfo.loginname
      }
      }">

        <img class="avatar" :src="userinfo.avatar_url">
        <span class="loginname">{{userinfo.loginname}}</span>
      </router-link>
    </section>
    <!--最近话题-->
    <section class="recent_topics">
      <p>作者最近其他话题</p>
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
    <section class="recent_replies">
      <p>作者最近参与话题</p>
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
    border: 1px solid red;
  }

  a {
    color: #777777;
    text-decoration: none;
  }

  a:hover {
    color: #4183C4;
  }

  .sidebar {
    width: 20%;
    margin: 0 auto;
  }

  .avatar {
    width: 30px;
    height: 30px;
    border-radius: 3px;
  }

  .loginname {
    vertical-align: top;

  }


</style>
