<!--下面写JS-->
<script>
import { get } from 'lodash';

export default {
  data() {
    return {
      titleData: '',
      contentData: '',
      markData: '',
      days: [
        { id: "day1", day: "星期一" },
        { id: "day2", day: "星期二" },
        { id: "day3", day: "星期三" },
        { id: "day4", day: "星期四" },
        { id: "day5", day: "星期五" },
        { id: "day6", day: "星期六" },
        { id: "day7", day: "星期日" },
      ],
      onWeeks: [
        { id: "week1", week: "第一周" },
        { id: "week2", week: "第二周" },
        { id: "week3", week: "第三周" },
        { id: "week4", week: "第四周" },
        { id: "week5", week: "第五周" },
        { id: "week6", week: "第六周" },
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
        // 调取属性的两种方式，点操作符和中括号操作符。纯数字只能用中括号。
        .then((json) => {
          console.log(json[2][4][3]);
          // document.getElementById("").innerHTML = json[2][4][3];
          var m = 2;
          for (var w = 1; w <= 6; w++) {
            for (var d = 1; d <= 7; d++)
              for (var e = 0; e <= 99; e++)
                if (json[m][w][d][e] != null)
                  document.getElementById("week" + w + "day" + d).innerHTML += json[m][w][d][e].title;
          }
        });
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
    <!-- HTML里面，虽然没有限制，但每个元素的ID最好唯一 -->
    <tr v-for="onWeek in onWeeks" :id="onWeek.id">
      <td v-for="day in days" :id="onWeek.id + day.id">
        <!-- <div id="timeBlock"> -->
        <!-- <el-input class="timeBlock" id="timeBlockTitle" v-model.lazy="titleData" @change="innerChange()" />
          <el-input class="timeBlock" id="timeBlockData" v-model.lazy="contentData" @change="innerChange()" />
          <el-input class="timeBlock" id="timeBlockMark" v-model.lazy="markData" @change="innerChange()" /> -->

        <!-- </div> -->
      </td>
    </tr>
  </table>
</template>