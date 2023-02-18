<!--下面写JS-->
<script>
export default {
  data() {
    return {
      titleData: '',
      contentData: '',
      markData: '',
      days: [
        { id: 1, day: "星期一" },
        { id: 2, day: "星期二" },
        { id: 3, day: "星期三" },
        { id: 4, day: "星期四" },
        { id: 5, day: "星期五" },
        { id: 6, day: "星期六" },
        { id: 7, day: "星期日" },
      ],
      onWeeks: [
        { id: 1, week: "第一周" },
        { id: 2, week: "第二周" },
        { id: 3, week: "第三周" },
        { id: 4, week: "第四周" },
        { id: 5, week: "第五周" },
        { id: 6, week: "第六周" },
      ]
    };
  },
  mounted() {
    // 接着写JS……
    this.titleData = localStorage.getItem('Title');
    this.contentData = localStorage.getItem('Data');
    this.markData = localStorage.getItem('Mark');
  },
  methods: {
    // 接着写JS……
    innerChange: function () {
      localStorage.setItem('Title', this.titleData);
      localStorage.setItem('Data', this.contentData);
      localStorage.setItem('Mark', this.markData);
    },
    uploadFile: function () {
      document.getElementById("uploadFile").click();
    },
    uploadInput: function () {
      console.log(document.getElementById("uploadFile").value)
    },
    tirggerFile: function (event) {
      // 利用console.log输出看结构就知道如何处理档案资料
      var file = event.target.files[0];
      // console.log(file);
      fetch(file.path)
        .then((response) => response.json())
        .then((json) => console.log(json));
    }
  }
}
</script>

<!--下面写HTML-->
<template>
  <el-button id="uploadButton" v-on:click="uploadFile()">上传JSON</el-button>
  <input id="uploadFile" type="file" style="display:none" @change="tirggerFile($event)" />
  <table>
    <!-- 表头 -->
    <tr>
      <td v-for="day in days">
        {{ day.day }}
      </td>
    </tr>
    <!-- 表格 -->
    <tr v-for="onWeek in onWeeks" :id="onWeek.id">
      <td v-for="day in days" :id="day.id">
        <div id="timeBlock">
          <!-- <el-input class="timeBlock" id="timeBlockTitle" v-model.lazy="titleData" @change="innerChange()" />
          <el-input class="timeBlock" id="timeBlockData" v-model.lazy="contentData" @change="innerChange()" />
          <el-input class="timeBlock" id="timeBlockMark" v-model.lazy="markData" @change="innerChange()" /> -->

        </div>
      </td>
    </tr>
  </table>
</template>