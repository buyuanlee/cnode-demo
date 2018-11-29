<template>
  <div id="app">
    <nav>
      <Header></Header>
    </nav>
    <main>
      <router-view name="main"></router-view>
      <router-view name="sidebar"></router-view>
    </main>
  </div>
</template>

<script>
  import Header from './components/Header'
  import PostList from './components/PostList'

  export default {
    name: 'App',
    components: {
      Header,
      PostList
    }
  }
</script>

<style scoped>
  #app {
    background-color: #E1E1E1;
  }

  nav {
    width: 100%;
    margin-bottom: 20px;
  }

  main {
    margin: 0 auto;
    width: 90%;
    border-radius: 5px;
    display: flex;
  }
</style>
