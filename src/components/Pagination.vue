<template>
  <div class="pagination">
    <button @click="change">首页</button>
    <button @click="change">上一页</button>
    <button v-if="judge">……</button>
    <button class="btnnum" v-for="btn in btns"
            @click="change(btn)"
            :class="[{active:btn===currentPage}]">
      {{btn}}
    </button>
    <button @click="change">下一页</button>
  </div>
</template>

<script>
  export default {
    name: "Pagination",
    data() {
      return {
        btns: [1, 2, 3, 4, 5, '……'],
        currentPage: 1,
        judge: false
      }
    },
    methods: {
      change(page) {
        //点击上一页，下一页,首页
        if (typeof page != 'number') {
          switch (page.target.innerText) {
            case '上一页':
              $('button.active').prev().click();
              break;
            case '下一页':
              $('button.active').next().click();
              break;
            case '首页':
              this.btns = [1, 2, 3, 4, 5, '……'];
              this.change(1);
              break;
            default:
              break;
          }
          return;
        }
        this.currentPage = page;
        if (page > 4) {
          this.judge = true
        } else {
          this.judge = false
        }
        if (page === this.btns[4]) {
          // 移除第一个元素
          this.btns.shift();
          this.btns.splice(4, 0, this.btns[3] + 1);
        }
        else if (page === this.btns[0] && page != 1) {
          this.btns.unshift(this.btns[0] - 1);
          this.btns.splice(5, 1);
        }
        this.$emit('handleList', this.currentPage)
      }
    }
  }
</script>

<style scoped>
  button {
    width: 80px;
    height: 30px;
    border: 1px solid #777777;
    border-radius: 3px;
  }

  .btnnum {
    margin: 2px;
  }

  .active {
    background-color: #777777;
    color: #ffffff;
  }
</style>
