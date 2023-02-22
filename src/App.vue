<!--下面写JS-->
<script>

export default {
  data() {
    return {
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
      ],
      month: ''//Sun Jan 01 2023 00:00:00 GMT+0800 (中国标准时间)
    };

  },
  mounted() {
    // this作用域赋给window
    window.refresh = this.refresh;
    window.calculateNumMonth = this.calculateNumMonth;
    window.ifYear = this.ifYear;
    refresh();
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
          var month1 = new Date(this.month);
          var getMonth = month1.getMonth() + 1;
          var year = month1.getFullYear();
          var y = year;
          var m = getMonth;

          for (var w = 1; w <= 6; w++)
            for (var d = 1; d <= 7; d++)
              for (var e = 0; e <= 99; e++)
                if (json[m][w][d][e] != null)
                  console.log(json[y][m][w][d][e].title);
          //         document.getElementById("week" + w + "day" + d).value += json[y][m][w][d][e].title;
        });
    },
    ifYear: function (year) {
      console.log(year);
      if (year % 4 == 0) {
        if (year % 100 == 0) {
          if (year % 400 == 0) {
            console.log("被4、100、400整除 闰年");
            return 29;
          }
          else {
            console.log("被4、100整除 平年");
            return 28;
          }
        }
        else {
          console.log("被4整除 闰年");
          return 29;
        }
      }
      else {
        console.log("不能被4整除 平年");
        return 28;
      }
    },
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
    refresh: function () {
      var month1 = new Date(this.month);
      var getMonth = month1.getMonth() + 1;
      var getDay = month1.getDay();

      var delta = 0;
      if (getDay == 0)
        // 周日
        delta = 6;
      else
        delta = getDay - 1;
      for (var i = 1; i <= 6; i++) {
        for (var j = 1; j <= 7; j++) {
          var dayData = (((i - 1) * 7) + j) - delta;
          if (dayData > calculateNumMonth(getMonth)) {
            document.getElementById("week" + i + "day" + j + "num").innerHTML = dayData - calculateNumMonth(getMonth);

          }
          else if (dayData < 1) {
            document.getElementById("week" + i + "day" + j + "num").innerHTML = dayData + calculateNumMonth(getMonth - 1);
          }
          else {
            document.getElementById("week" + i + "day" + j + "num").innerHTML = dayData;
          }
        }
      }
    }
  }
}

</script>

<!--下面写HTML-->
<template>
  <el-button id="uploadButton" v-on:click="uploadFile()">上传JSON</el-button>
  <input id="uploadFile" type="file" style="display:none" @change="tirggerFile($event)" />


  <div class="block">
    <span class="demonstration">Month</span>
    <el-date-picker v-model="month" type="month" placeholder="Pick a month" @change="refresh()" />
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
        <el-input :id="onWeek.id + day.id"></el-input>
      </td>
    </tr>
  </table>
</template>