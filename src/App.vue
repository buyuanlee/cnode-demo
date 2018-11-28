<template>
  <div id="app">
    <nav>
      <Header></Header>
    </nav>
    <main>
      <router-view name="main"></router-view>
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
    width: 80%;
    background-color: #fff;
    border-radius: 5px;
    margin: 0 auto;
  }
</style>
