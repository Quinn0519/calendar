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
      whichBlock: [{}, {}, {}, {}, {}, {}, {}]
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

    getToday();
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
      fetch(file.path)
        .then((response) => response.json())
        // 调取属性的两种方式，点操作符和中括号操作符。纯数字只能用中括号。
        .then((json) => {
          var y = this.getYear;
          var m = this.getMonth;
          if (json[y][m] != undefined) {
            console.log(json[y][m] + "年月已经定义")
            for (var w = 1; w <= 6; w++) {
              if (json[y][m][w] != undefined) {
                console.log(json[y][m][w] + "w已经定义")
                for (var d = 1; d <= 7; d++) {
                  if (json[y][m][w][d] != undefined) {
                    for (var e = 0; e <= 99; e++)
                      if (json[y][m][w][d][e] != undefined) {
                        console.log(json[y][m][w][d][e] + "事件已经定义")
                        console.log(json[y][m][w][d][e].title);
                        document.getElementById("week" + w + "day" + d).value += json[y][m][w][d][e].title;
                      }
                  }
                }
              }
            }
          }
        });
    },
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
    getToday: function () {
      var today = new Date();
      this.month = String(today);//是一个字符串，因为month是字符串，不能赋对象。
    },
    render: function (year, month, delta) {
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
    getDayConversion: function (getDay) {
      if (getDay == 0)
        // 因为西方国家一周的开始是周日，所以周日是0。咱们用周一为开始，要做转换。
        return 6;
      else
        return getDay - 1;
    },
    refresh: function () {
      // 管线
      var month1 = new Date(this.month);
      var getDay = month1.getDay();
      var getMonth = month1.getMonth() + 1;
      this.getMonth = getMonth;
      var year = month1.getFullYear();
      this.getYear = year;

      render(year, getMonth, getDayConversion(getDay));
    },
    saveAsJson: function () {
      console.log(JSON.stringify(this.whichBlock))
      // 将数据写入文件sample.html
      fs.writeFile('D:/OneDrive/Dev/Data/output.json', JSON.stringify(this.whichBlock),
        // fs.writeFileSync(configPath, parseInt(nextBlock, 10).toString())
        // 写入文件后调用的回调函数
        function (err) {
          if (err) throw err;
          // 如果没有错误
          console.log("Data is written to file successfully.")
        });
    }
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
        <el-input :id="onWeek.id + day.id" v-model="whichBlock[onWeek.num][day.num]"></el-input>
      </td>
    </tr>
  </table>
</template>