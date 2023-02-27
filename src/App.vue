<!--下面写JS-->
<script>
const fs = require('fs');

export default {
  data() {
    return {
      days: [
        { num: 1, id: "day1", day: "星期一" },
        { num: 2, id: "day2", day: "星期二" },
        { num: 3, id: "day3", day: "星期三" },
        { num: 4, id: "day4", day: "星期四" },
        { num: 5, id: "day5", day: "星期五" },
        { num: 6, id: "day6", day: "星期六" },
        { num: 7, id: "day7", day: "星期日" },
      ],
      onWeeks: [
        { num: 1, id: "week1", week: "第一周" },
        { num: 2, id: "week2", week: "第二周" },
        { num: 3, id: "week3", week: "第三周" },
        { num: 4, id: "week4", week: "第四周" },
        { num: 5, id: "week5", week: "第五周" },
        { num: 6, id: "week6", week: "第六周" },
      ],
      month: '',
      getMonth: '',
      getYear: '',
      dayData: '',
      whichBlock: [{}, {}, {}, {}, {}, {}, {}],
      checked: [{}, {}, {}, {}, {}, {}, {}]
    };

  },
  mounted() {
    // this作用域赋给window
    window.refresh = this.refresh;
    window.getToday = this.getToday;
    window.calculateNumMonth = this.calculateNumMonth;
    window.ifYear = this.ifYear;
    window.render = this.render;
    window.getDayConversion = this.getDayConversion;
    window.tableEvent = this.tableEvent;
    window.completeEvent = this.completeEvent;
    window.displayJson = this.displayJson;
    window.tirggerFile = this.tirggerFile;
    window.saveJson = this.saveJson;
    window.cleanBlock = this.cleanBlock;
    window.cleanAndRefresh = this.cleanAndRefresh;

    getToday();
    refresh();
  },
  methods: {
    // 触发 "uploadFile" 元素的点击事件
    uploadFile: function () {
      document.getElementById("uploadFile").click();
    },
    // 抓取文件
    tirggerFile: function (event) {
      // 利用console.log输出看结构就知道如何处理档案资料
      // var file = event.target.files[0];
      var month1 = new Date(this.month);
      var getMonth = month1.getMonth() + 1;
      var getYear = month1.getFullYear();
      var filePath = './jsonData/' + getYear + getMonth + '.json';
      fetch(filePath)
        .then((response) => response.json())
        .then((json) => this.displayJson(json));
    },
    // 渲染Json内容
    displayJson: function (json) {
      console.log(json)
      for (var i = 1; i <= 6; i++) {
        for (var j = 1; j <= 7; j++) {
          if (json[i][j] != undefined) {
            this.whichBlock[i][j] = json[i][j];
          }
        }
      }
    },
    // 闰年判断
    ifYear: function (year) {
      if (year % 4 == 0) {
        if (year % 100 == 0) {
          if (year % 400 == 0) {
            return 29;
          }
          else {
            return 28;
          }
        }
        else {
          return 29;
        }
      }
      else {
        return 28;
      }
    },
    // 月份与每月天数
    calculateNumMonth: function (getMonth) {
      var month1 = new Date(this.month);
      var year = month1.getFullYear();
      if (getMonth < 1 || getMonth > 12)
        return 31
      else if (getMonth == 1)
        return 31
      else if (getMonth == 2)
        return ifYear(year)
      else if (getMonth == 3)
        return 31
      else if (getMonth == 4)
        return 30
      else if (getMonth == 5)
        return 31
      else if (getMonth == 6)
        return 30
      else if (getMonth == 7)
        return 31
      else if (getMonth == 8)
        return 31
      else if (getMonth == 9)
        return 30
      else if (getMonth == 10)
        return 31
      else if (getMonth == 11)
        return 30
      else if (getMonth == 12)
        return 31
      else
        console.error("monthDay数值错误！")
    },
    // 获取今天
    getToday: function () {
      // 是一个字符串，因为 this.month 是字符串，不能赋对象。
      var toDay = new Date();
      // 创建一个新的事件对象，用当前年当前月，其他为默认的内容来表示月的时间戳。与ElementUI的Date Picker相匹配。
      var toMonth = new Date(toDay.getFullYear(), toDay.getMonth())
      // console.log(toMonth)
      this.month = toMonth;
    },
    // 渲染页面
    render: function (month, delta) {
      for (var i = 1; i <= 6; i++) {
        for (var j = 1; j <= 7; j++) {
          var dayData = (((i - 1) * 7) + j) - delta;
          this.dayData = dayData;
          if (dayData > calculateNumMonth(month)) {
            document.getElementById("week" + i + "day" + j + "num").innerHTML = dayData - calculateNumMonth(month);
          }
          else if (dayData < 1) {
            document.getElementById("week" + i + "day" + j + "num").innerHTML = dayData + calculateNumMonth(month - 1);
          }
          else {
            document.getElementById("week" + i + "day" + j + "num").innerHTML = dayData;
          }
        }
      }
    },
    // getDay换算
    getDayConversion: function (getDay) {
      if (getDay == 0)
        // 因为西方国家一周的开始是周日，所以周日是0。咱们用周一为开始，要做转换。
        return 6;
      else
        return getDay - 1;
    },
    // 判断完成状态 加删除线
    completeEvent: function () {
      for (var i = 1; i <= 6; i++) {
        for (var j = 1; j <= 7; j++) {
          console.log(this.checked[i][j])
          // 如果checked，添加删除线
          if (this.checked[i][j] == true) {
            // 改变样式，加删除线
            document.getElementById("week" + i + "day" + j).style.textDecoration = "line-through";
          }
          // 否则，去掉删除线
          else if (this.checked[i][j] == false) {
            document.getElementById("week" + i + "day" + j).style.textDecoration = "";
          }
        }
      }
    },
    saveJson: function () {
      var month1 = new Date(this.month);
      var getMonth = month1.getMonth() + 1;
      var getYear = month1.getFullYear();
      console.log(__dirname)
      console.log(JSON.stringify(this.whichBlock))
      fs.writeFile('./jsonData/' + getYear + getMonth + '.json', JSON.stringify(this.whichBlock),
        // 写入文件后调用的回调函数
        function (err) {
          if (err) throw err;
          // 如果没有错误
          console.log("Data is written to file successfully.")
        });
    },
    cleanAndRefresh: function () {
      for (var i = 1; i <= 6; i++) {
        for (var j = 1; j <= 7; j++) {
          this.whichBlock[i][j] = "";
          this.checked[i][j] = false;
        }
      }
      tirggerFile();
    },
    // 刷新函数
    refresh: function () {
      var month1 = new Date(this.month);
      var getDay = month1.getDay();
      var getMonth = month1.getMonth() + 1;
      this.getMonth = getMonth;
      var year = month1.getFullYear();
      this.getYear = year;

      // 管线
      render(getMonth, getDayConversion(getDay));
      completeEvent();
      saveJson();
      tirggerFile();
    },
  }
}
</script>

<!--下面写HTML-->
<template>
  <el-button id="uploadButton" v-on:click="uploadFile()">上传JSON</el-button>
  <el-button id="saveAsButton" v-on:click="saveAsJson()">导出JSON</el-button>
  <input id="uploadFile" type="file" style="display:none" @change="tirggerFile($event)" />

  <div class="block">
    <span class="demonstration">Month</span>
    <el-date-picker v-model="month" type="month" placeholder="Pick a month" @change="cleanAndRefresh()" />
  </div>
  <p>{{ month }}</p>

  <table id="outer">
    <!-- 表头 -->
    <tr id="header">
      <td v-for="day in days">
        {{ day.day }}
      </td>
    </tr>
    <!-- 表格 -->
    <!-- HTML里面，虽然没有限制，但每个元素的ID最好唯一 -->
    <tr v-for="onWeek in onWeeks" :id="onWeek.id">
      <td v-for="day in days">
        <div class="inputnum" :id="onWeek.id + day.id + 'num'"></div>
        <div class="inputBlock">
          <el-input :id="onWeek.id + day.id" v-model="whichBlock[onWeek.num][day.num]" @change="refresh()"></el-input>
          <el-checkbox :id="onWeek.id + day.id + 'checkbox'" v-model="checked[onWeek.num][day.num]" size="small"
            @change="refresh()" style="" />
        </div>
      </td>
    </tr>
  </table>
</template>