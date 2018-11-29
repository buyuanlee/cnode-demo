<template>
  <div class="user_info">
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <main v-else>
      <section>
        <img class="avatar" :src="userinfo.avatar_url" alt="">
        <span class="loginname">{{userinfo.loginname}}</span>
        <p class="score">{{userinfo.score}}积分</p>
        <p class="registration_time">注册时间：{{userinfo.create_at|formatDate}}</p>
      </section>
      <section>
        <p class="section_header">最近创建的话题</p>
        <ul>
          <li v-for="item in userinfo.recent_topics">
            <router-link :to="{
            name:'post_content',
            params:{
            id:item.id,
            name:item.author.loginname

            }}">
              {{item.title}}
            </router-link>
          </li>
        </ul>
      </section>
      <section>
        <p class="section_header">最近参与的话题</p>
        <ul>
          <li v-for="item in userinfo.recent_replies">
            <router-link :to="{
            name:'post_content',
            params:{
            id:item.id,
            name:item.author.loginname
            }}">
              {{item.title}}
            </router-link>
          </li>
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
        userinfo: {}
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
      }
    },
    beforeMount() {
      this.isLoading = true
      this.getData()
    }
  }
</script>

<style scoped>
  section {
    padding: 15px;
    margin-bottom: 10px;
  }

  .avatar {
    width: 40px;
    height: 40px;
  }

  .loginname {
    font-size: 14px;
    color: #777777;
    vertical-align: top;
  }

  .section_header {
    background-color: #f6f6f6;
  }
</style>
