<template>
  <div class="postlist">
    <!--数据未返回时显示loading-->
    <div class="loading" v-if="isLoading">
      <img src="../assets/loading.gif">
    </div>
    <!--主题列表-->
    <div v-else>
      <ul>
        <li>
          <div class="topbar">
            <span @click="change" :class="[tab === '' ? 'active':'deactive']">全部</span>
            <span @click="change" :class="[tab === 'good' ? 'active':'deactive']">精华</span>
            <span @click="change" :class="[tab === 'share' ? 'active':'deactive']">分享</span>
            <span @click="change" :class="[tab === 'ask' ? 'active':'deactive']">问答</span>
            <span @click="change" :class="[tab === 'job' ? 'active':'deactive']">招聘</span>
          </div>
        </li>
        <li v-for="post in posts">
          <!--头像-->
          <router-link :to="{
          name:'user_info',
          params:{
          name:post.author.loginname
          }
          }">
            <img class="avatar" :src="post.author.avatar_url" alt="">
          </router-link>
          <!--回复浏览数-->
          <span class="count">
          <span class="reply_count">{{post.reply_count}}</span><span class="visit_count">/{{post.visit_count}}</span>
          </span>
          <!--分类-->
          <span :class="[{
          post_good:(post.good===true),
          post_top:(post.top===true),
          topiclist_tab:(post.good!=true&&post.top!=true)
          }]">{{post|tabFormatter}}</span>
          <!--标题-->
          <router-link :to="{
          name:'post_content',
          params:{
            id:post.id,
            name:post.author.loginname
          }}">
            <span>{{post.title}}</span>
          </router-link>
          <!--最后回复时间-->
          <span class="last_reply">{{post.last_reply_at | formatDate}}</span>
          <!--分割线-->
        </li>
        <!--页码表-->
        <li>
          <Pagination @handleList="renderList"></Pagination>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  import Pagination from './Pagination'

  export default {
    name: "PostList",
    data() {
      return {
        isLoading: false,
        //页面列表数组
        posts: [],
        postpage: 1,
        tab: ''
      }
    },
    components: {
      Pagination
    },
    methods: {
      getData() {
        this.$http.get('https://cnodejs.org/api/v1/topics', {
          params: {
            page: this.postpage,
            limit: 20,
            tab: this.tab,
          }
        })
        //返回成功
          .then(res => {
            this.isLoading = false
            this.posts = res.data.data
          })
          //返回失败
          .catch(err => {
            console.log(err)
          })
      },
      change(event) {
        if (event.currentTarget.innerText === '全部') {
          this.tab = ''
        } else if (event.currentTarget.innerText === '精华') {
          this.tab = 'good'
        } else if (event.currentTarget.innerText === '分享') {
          this.tab = 'share'
        } else if (event.currentTarget.innerText === '问答') {
          this.tab = 'ask'
        } else if (event.currentTarget.innerText === '招聘') {
          this.tab = 'job'
        } else {

        }

        this.getData()
      },
      renderList(value) {
        this.postpage = value;
        this.getData()
      }
    },
    beforeMount() {
      this.isLoading = true
      this.getData()
    }
  }
</script>

<style scoped>
  ul {
    padding: 0;
  }

  ul li:not(:first-child) {
    padding: 10px;
    font-size: 15px;
    font-weight: 400;
    border-top: 1px solid #f0f0f0;
  }

  li:hover {
    background-color: #E1E1E1;
  }

  .topbar {
    font-size: 14px;
  }

  .active {
    color: #ffffff;
    background-color: #80BD22;
    border-radius: 3px;
    padding: 3px 4px;
  }

  .deactive {
    color: #80BD22;
    padding: 10px;
  }

  .postlist {
    background-color: #fff;
    width: 90%;
    margin: 0 auto;
  }

  .topbar {
    background-color: #f6f6f6;
    padding: 10px;
    margin: 0;
  }

  .count {
    display: inline-block;
    width: 70px;
    color: #444444;
    text-align: center;
  }

  .reply_count {
    color: #9e78c0;
  }

  .visit_count {
    font-size: 10px;
    color: #444444;
  }

  .post_good, .post_top {
    background-color: #80BD22;
    color: #FFFFFF;
    border-radius: 3px;
    padding: 2px 4px;
    font-size: 12px;
    margin-right: 10px;
  }

  .topiclist_tab {
    background-color: #E5E5E5;
    color: #b4b4b4;
    border-radius: 3px;
    padding: 2px 4px;
    font-size: 12px;
    margin-right: 10px;
  }

  .last_reply {
    color: #b4b4b4;
    font-size: 12px;
    float: right;
    margin: 10px;
  }
</style>
