<!--下面写JS-->
<script>
export default {
  data() {
    return {
      titleData: ''
    };
  },
  mounted() {
    // 接着写JS……
  //   var timeBlock = { title: "数据裤应用", data: "2023年1月6日", mark: "好简单啊" };
  //   document.getElementById('timeBlockTitle').innerHTML = timeBlock.title;
  //   document.getElementById('timeBlockData').innerHTML = timeBlock.data;
  //   document.getElementById('timeBlockMark').innerHTML = timeBlock.mark;
  this.titleData = localStorage.getItem('Title');
  console.log("读取本地数据：",localStorage.getItem('Title'))
  },
  methods: {
    // 接着写JS……
    innerChange: function() {
      localStorage.setItem('Title', this.titleData);
      console.log("成功保存本地数据：",localStorage.getItem('Title'))
      // localStorage.setItem('Data', document.getElementById('timeBlockData').innerHTML);
      // localStorage.setItem('Mark', document.getElementById('timeBlockMark').innerHTML);
      // document.getElementById('testP').innerHTML = "1";
    }
  }
}
</script>

<!--下面写HTML-->
<template>
  <div>
    <div id="timeBlock">
      <textarea id="timeBlockTitle" v-model="titleData"></textarea>
      <textarea id="timeBlockData" ></textarea>
      <textarea id="timeBlockMark" ></textarea>
      <button v-on:click="innerChange()">Change</button>
      <!-- <p id="testP"></p> -->
    </div>
  </div>
</template>

<!--下面写CSS-->
<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

#timeBlock {
  display: flex;
  flex-direction: column;
}

#timeBlockTitle {
  /* height: 100%; */
  text-align: center;
  overflow: hidden;
  padding-top: 10%;
}

#timeBlockData {
  text-align: center;
  overflow: hidden;
  padding-top: 10%;
}

#timeBlockMark {
  text-align: center;
  overflow: hidden;
  padding-top: 10%;
}
</style>
