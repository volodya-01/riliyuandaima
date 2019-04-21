<template>
  <div class="outbox">
    <div class="leftoutbox">
      <div class="leftoutbox_topoutbox">
        <div class="leftoutbox_topoutbox_left">{{value}}年</div>
        <div class="leftoutbox_topoutbox_right">
          <!--  <span class="sanjiaoxing1"></span> -->
          <span class="span_slect_box">
            <span class="sanjiaoxing1"></span>
            <el-select v-model="value">
              <el-option
                v-for="item in options"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </span>
        </div>
      </div>
      <div class="leftoutbox_downoutbox">
        <div class="leftoutbox_downoutbox_left">
          <span class="monthnum">{{valuemonth}}</span>
          <span class="monthname">月</span>
        </div>
        <div class="leftoutbox_downoutbox_right">
          <!--  <span class="sanjiaoxing2"></span> -->
          <span class="span_slect_box">
            <span class="sanjiaoxing2"></span>
            <el-select v-model="valuemonth">
              <el-option
                v-for="item in optionsmonth"
                :key="item.valuemonth"
                :label="item.labelmonth"
                :value="item.valuemonth"
              ></el-option>
            </el-select>
          </span>
        </div>
      </div>
    </div>
    <div class="centeroutbox">
      <swiper3/>
    </div>
    <div class="rightoutbox" @click="todayriliapi">
      <div class="rightoutbox_topoutbox">2019年</div>
      <div class="rightoutbox_downoutbox">今天</div>
    </div>
  </div>
</template>
<script>
import Bus from "@/bus.js";
import swiper3 from "@/components/swiper3";
export default {
  name: "rili",
  components: {
    swiper3
  },
  data() {
    return {
      options: [
        {
          label: "2019年",
          value: "2019"
        },
        {
          label: "2018年",
          value: "2018"
        },
        {
          label: "2017年",
          value: "2017"
        },
        {
          label: "2016年",
          value: "2016"
        },
        {
          label: "2015年",
          value: "2015"
        }
      ],
      value: "2019",
      optionsmonth: [
        {
          labelmonth: "1月",
          valuemonth: "01"
        },
        {
          labelmonth: "2月",
          valuemonth: "02"
        },
        {
          labelmonth: "3月",
          valuemonth: "03"
        },
        {
          labelmonth: "4月",
          valuemonth: "04"
        },
        {
          labelmonth: "5月",
          valuemonth: "05"
        },
        {
          labelmonth: "6月",
          valuemonth: "06"
        }
      ],
      valuemonth: "03"
    };
  },

  mounted() {
    var date = new Date();
    this.value = date.getFullYear();
    var todaymonth = date.getMonth() + 1;
    if (todaymonth < 10) {
      this.valuemonth = "0" + todaymonth;
    } else {
      this.valuemonth = todaymonth;
    }
    /*   console.log(this.value)
     console.log(this.valuemonth) */
    var optyeararray = [];
    var optmontharray = [];
    for (var i = 1980; i < 2050; i++) {
      var optyear = {};
      optyear.label = i + "年";
      optyear.value = i + "";
      optyeararray.push(optyear);
    }

    for (var j = 1; j <= 12; j++) {
      var optmonth = {};
      optmonth.labelmonth = j + "月";
      if (j < 10) {
        optmonth.valuemonth = "0" + j;
      } else {
        optmonth.valuemonth = j + "";
      }
      optmontharray.push(optmonth);
    }
    this.options = optyeararray;
    this.optionsmonth = optmontharray;
    this.$nextTick(this.SelectDayCardData());
  },
  methods: {
    SelectDayCardData() {
      var _this = this;
      var SelectDayCardData = {};
      SelectDayCardData.Customer = "shaoxing";
      SelectDayCardData.yearMonth = this.value + this.valuemonth + "";
      console.log("SelectDayCardData");
      console.log(SelectDayCardData);
      _this.$axios
        .post(
          /*  urlClass.axiosUrWarningEvent + "areaBigUserData", */
          "http://192.168.0.105:8184/Service1.svc/SelectDayCardData",
          JSON.stringify(SelectDayCardData),
          {
            headers: { "Content-Type": "application/json;" }
          }
        )
        .then(res => {
           Bus.$emit("SelectDayCardDatares", res);
          console.log(res);
        })
        .catch(error => {
          /*  console.log(error); */
        });
    },
    todayriliapi() {
      var date = new Date();
      var year = date.getFullYear();
      var todaymonth = date.getMonth() + 1;
      if (todaymonth < 10) {
       var month = "0" + todaymonth;
      } else {
      var month  = todaymonth;
      }
      var SelectDayCardData = {};
      SelectDayCardData.Customer = "shaoxing";
      SelectDayCardData.yearMonth = year+ month + "";
      console.log("todayriliapi_SelectDayCardData");
      console.log(SelectDayCardData);
      this.$axios
        .post(
          /*  urlClass.axiosUrWarningEvent + "areaBigUserData", */
          "http://192.168.0.105:8184/Service1.svc/SelectDayCardData",
          JSON.stringify(SelectDayCardData),
          {
            headers: { "Content-Type": "application/json;" }
          }
        )
        .then(res => {
        Bus.$emit("todayriliapires", res);
          console.log(res);
        })
        .catch(error => {
          /*  console.log(error); */
        });
    }
  },
  watch: {
    value() {
      this.$nextTick(this.SelectDayCardData());
    },
    valuemonth() {
      this.$nextTick(this.SelectDayCardData());
    }
  }
};
</script>
<style lang='stylus' scoped>
.outbox {
  /* width: 1600px; */
  width: 86vw;
  height: 60px;
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: space-between;
  margin: 200px 100px;
  /* background-color: aquamarine */
}

.leftoutbox {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: flex-start;
  min-width: 66px;
  width: 66px;
  height: 60px;
  border: 1px #e3e6e9 solid;
  cursor: pointer;
  z-index: 9999999999;
  /* background-color: #63bfd4 */
}

.leftoutbox_topoutbox {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: flex-start;
  height: 20px;
  min-width: 66px;
  width: 66px;
  background-color: #63bfd4;
}

.leftoutbox_topoutbox_left {
  width: 56px;
  min-width: 56px;
  height: 20px;
  font: bold 12px / 20px '微软雅黑';
  text-align: center;
  color: #fff;
}

.leftoutbox_topoutbox_right {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-end;
  align-items: flex-end;
  width: 10px;
  min-width: 10px;
  height: 20px;
  /* position: relative; */
  /* background-color: #7b63d4 */
}

.span_slect_box {
  display: inline-block;
  width: 10px;
  height: 15px;
  position: relative;
  /* background-color red */
  pointer-events: none;
}

.sanjiaoxing1 {
  display: inline-block;
  width: 0px;
  height: 0px;
  border-width: 0px 0px 6px 6px;
  border-color: #fff transparent;
  border-style: solid;
  margin-right: 2px;
  margin-bottom: 2px;
}

.span_slect_box >>>.el-select {
  display: inline-block;
  position: absolute;
  left: -1px;
  width: 10px;
  height: 15px;
  z-index: 99;
  opacity: 0;
}

.span_slect_box >>>.el-select .el-input__inner {
  width: 14px;
  height: 20px;
  padding: 0;
  margin: 0;
  pointer-events: all;
  position: absolute;
  top: -24px;
  left: -2px;
  pointer-events: all;
}

.span_slect_box >>>.el-select .el-input__suffix {
  display: none;
}

/* .sanjiaoxing1{
  display: inline-block;
  width: 0px;
  height: 0px;
  border-width: 0px 0px 6px 6px ;
  border-color: #fff transparent;
  border-style: solid;
  margin-right: 2px;
  margin-bottom: 2px
} */
.leftoutbox_downoutbox {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: flex-start;
  height: 38px;
  min-width: 66px;
  width: 66px;
}

.leftoutbox_downoutbox_left {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: center;
  align-items: center;
  width: 56px;
  min-width: 56px;
  height: 38px;
}

.monthnum {
  display: inline-block;
  width: 30px;
  height: 38px;
  font: bold 18px / 38px '微软雅黑';
  text-align: center;
  color: #63bfd4;
}

.monthname {
  display: inline-block;
  width: 15px;
  height: 18px;
  font: bold 10px / 18px '微软雅黑';
  text-align: center;
  color: #63bfd4;
  margin-top: 3px;
}

.leftoutbox_downoutbox_right {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-end;
  align-items: flex-end;
  width: 10px;
  min-width: 10px;
  height: 38px;
  /* background-color: #7b63d4 */
}

.sanjiaoxing2 {
  display: inline-block;
  width: 0px;
  height: 0px;
  border-width: 0px 0px 6px 6px;
  border-color: #63bfd4 transparent;
  border-style: solid;
  margin-right: 2px;
  margin-bottom: 2px;
}

.centeroutbox {
  /* width:calc(100%- 66px); */
  /* min-width: 1468px; */
  width: 1366px;
  height: 63px;
  cursor: pointer;
   /* margin :0 64px; */
  margin :0 3.33%
}

.rightoutbox {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  justify-content: flex-start;
  align-items: flex-start;
  width: 66px;
  min-width: 66px;
  height: 60px;
  border: 1px #e3e6e9 solid;
  cursor: pointer;
  z-index: 9999999999;
  /* background-color:#ff6f73 */
}

.rightoutbox_topoutbox {
  height: 20px;
  min-width: 66px;
  width: 66px;
  font: bold 12px / 20px '微软雅黑';
  text-align: center;
  color: #fff;
  background-color: #ff6f73;
}

.rightoutbox_downoutbox {
  height: 38px;
  min-width: 66px;
  width: 66px;
  font: bold 18px / 38px '微软雅黑';
  text-align: center;
  color: #ff6f73;
}
</style>

