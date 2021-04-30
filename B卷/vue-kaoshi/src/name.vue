<template>
  <div class="App">
    <!-- 考试  -->
    <div class="top">
      <div v-if="!checked">
        用户名<input v-model="username" type="text" /><br />
        <p class="zi" v-if="display">用户名必须是6到18位的数字，字母，下划线</p>
        <button @click="btn1">登录</button>
      </div>
      <div v-if="checked">
        用户名：{{ username }}
        <button @click="btn2">退出</button>
      </div>
    </div>
    <!-- 模态框 -->
    <div v-if="!checked" class="box"></div>
    <!--  -->
    <div class="font">
      <div
        v-if="jian == item.right"
        v-for="(item, index) in dataList"
        :key="index"
      >
        <h3>{{ item.title }}</h3>
        <div v-for="(arritem, index1) in dataList[index].options" :key="index1">
          <p><input type="radio" :value="index1" v-model="t" />{{ arritem }}</p>
        </div>
        <button @click="tijiao(item.right)">提交答案</button>
        <p v-show="isShow1">{{ num }}后进入下一题</p>
        <h3>共有{{ num }}/4道题</h3>
      </div>
      <div class="font" v-if="jian == -1">
        <span>没有题目了</span>
        <button @click="xin">重新答题</button>
      </div>
    </div>
  </div>
</template>
<script>
import dataJson from "@/assets/data.json";
export default {
  data() {
    return {
      username: "",
      dataList: [],
      jian: 3,
      daan: [],
      checked: false,
      display: false,
      num: 1,
      t: -1,
      isShow1: false,
    };
  },
  mounted() {
    this.dataList = dataJson.data;
    console.log(dataJson.data);
  },
  methods: {
    // 登录
    btn1() {
      let name = /^[a-zA-Z0-9_-]{6,18}$/;
      if (!name.test(this.username)) {
        this.display = true;
        return;
      } else {
        this.display = false;
        this.checked = true;
      }
    },
    // 退出
    btn2() {
      this.checked = false;
      this.username = "";
      this.num = 1;
      this.jian = 3;
    },
    // 提交
    tijiao() {
      // setTimeout({}, 1000);
      this.num += 1;
      this.jian -= 1;
    },
    // 重新
    xin() {
      this.num = 1;
      this.jian = 3;
    },
  },
};
</script>
<style lang="css" scoped>
.App {
  position: relative;
  display: flex;
}
.top {
  width: 500px;
  height: 500px;
}
.zi {
  color: red;
}
.box {
  position: absolute;
  top: 0px;
  left: 500px;
  width: 1000px;
  height: 600px;
  background-color: rgba(0, 0, 0, 0.5);
}
.font {
  margin-left: 100px;
}
</style>