<template>
  <div class="header">
    <img class="logo" src="../assets/cnodejs_light.svg" alt="logo">
    <ul>
      <li><a href="#">首页</a></li>
      <li><a href="#">新手入门</a></li>
      <li><a href="#">API</a></li>
      <li><a href="#">关于</a></li>
      <li><a href="#">注册</a></li>
      <li><a href="#">登陆</a></li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: "Header"
  }
</script>

<style scoped>
  .header {
    background-color: #444444;
  }

  .logo {
    max-width: 120px;
    margin: 10px 0 10px 50px;
  }

  ul {
    list-style: none;
    float: right;
    margin: 10px 50px 10px 0;
    height: 100%;
  }

  li {
    display: inline-block;
    height: 100%;
    padding: 5px 10px;

  }

  a {
    text-decoration: none;
    color: #ccc;
    font-size: 14px;
  }
</style>
