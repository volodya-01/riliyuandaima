
51 lines (49 sloc) 1.45 KB
<template>
  <div class="wrapper">
    <!-- swiper -->
    <swiper :options="swiperOption" class="swiper-container">
      <swiper-slide class="swiper-item" v-for="(slide, index) in RData" :key="index">
        <div class="lunbooutbox" @click="swiperitemtSelect(slide,index)">
          <div class="lunboinboxbgcolorsymbol" :style="{height:(slide.Count/slide.MaxCount*60)+'px'}" :class="Number(slide.MaxGrade)==1? 'graybgcolorsymbol' :(Number(slide.MaxGrade)==2? 'bluebgcolorsymbol' :(Number(slide.MaxGrade)==3? 'redbgcolorsymbol' :''))" ></div>
          <div class="lunboinbox" :class="{'lunboinboxbox_active': i == index}">
            <div class="lunboinbox_topbox">
              <div class="lunboinbox_topbox_dayeventnum" :class="Number(slide.MaxGrade)==1? 'graybgcolor' :(Number(slide.MaxGrade)==2? 'bluebgcolor' :(Number(slide.MaxGrade)==3? 'redbgcolor' :''))" >{{slide.Count==0?'':slide.Count}}</div>
            </div>
            <div class="lunboinbox_downbox">
              <div class="lunboinbox_downbox_fontoutboxmonthnum">{{slide.Day<10?'0'+slide.Day:slide.Day}}</div>
              <div class="lunboinbox_downbox_fontoutboxweeknum">{{slide.Week}}</div>
            </div>
          </div>
        </div>
      </swiper-slide>
      <div class="swiper-button-prev" slot="button-prev"></div>
      <div class="swiper-button-next" slot="button-next"></div>
    </swiper>
  </div>
</template>

<script>
import Bus from "@/bus.js";
export default {
  name: "swiper3",
  data() {
    return {
      i: 0,
      RData:[],
      bigMaxCount:'',
      /* swiperSlides: 31, */
      swiperOption: {
        slidesPerView: 26,
        spaceBetween: 8,
        navigation: {
          nextEl: ".swiper-button-next",
          prevEl: ".swiper-button-prev"
        }
      }
    };
  },
  created() {
    var ClienWidth = document.documentElement.clientWidth;
    if (ClienWidth == 1366) {
      this.swiperOption.slidesPerView = 19;
    }
    if (ClienWidth == 1440) {
      this.swiperOption.slidesPerView = 20;
    }
    if (ClienWidth == 1680) {
      this.swiperOption.slidesPerView = 23;
    }
    console.log(ClienWidth);
  },
  mounted() {
    Bus.$on("SelectDayCardDatares", e => {
      var _this = this;
       _this.RData=e.data.RData
      _this.swiperSlides = e.data.RData.length;
      var arr=e.data.RData
      for (var i = 0; i < e.data.RData.length - 1; i++) {
          for (var j = 0; j < e.data.RData.length - 1 - i; j++) {
            if (arr[j].MaxCount < arr[j + 1].MaxCount) {
              var temp = arr[j];
              arr[j] = arr[j + 1];
              arr[j + 1] = temp;
            }
          }
        }
         _this.bigMaxCount=arr[0].MaxCount
      console.log("SelectDayCardDatares");
      console.log(e);
      console.log('arr');
      console.log(this.bigMaxCount);
    });
    Bus.$on("todayriliapires", e => {
      var _this = this;
      _this.swiperSlides = e.data.RData.length;
      console.log("todayriliapires");
      console.log(e);
    });
  },
  methods: {
    swiperitemtSelect(slide, index) {
      this.i = index;
      /*  console.log(slide) */
      console.log(index);
     /*  alert(this.RData[index].DateTime) */
      /*  WindowsEvent.MyDispatchEvent("Rcdd_WarningEventDraw", item); */
    }
  }
};
</script>
<style lang='stylus' scoped>

/* 修改swiper轮播组件样式 */
.wrapper {
  .swiper-container {
    /* width 1400px */
    width: 75vw;

    /* padding 0 50px */
    .swiper-item {
      width: 45px;
      height: 68px;

      .swiper-item-contentoutbox {
      }
    }
  }
}

.lunbooutbox {
  width: 45px;
  height: 60px;
  /* background-color: aqua; */
  position: relative;
  background-color: #f4f4f4;
  z-index: 44;
  pointer-events: all;
}

.lunboinbox {
  width: 42px;
  height: 57px;
  pointer-events: none;
}

.lunboinboxbox_active {
  /* outline :3px #c9ecb8 solid; */
  position: absolute;
  border: 3px #c9ecb8 solid;
  z-index: 666;
}

.lunboinboxbgcolorsymbol {
  width: 45px;
  height: 30px;
  /* background-color: #f5c3c4; */
  position: absolute;
  bottom: 0;
  z-index: -2;
  pointer-events: none;
}
.graybgcolorsymbol{
  background-color: #d5dbe0
}
.bluebgcolorsymbol{
 background-color: #aee0eb
}
.redbgcolorsymbol{
 background-color: #f5c3c4
}
.lunboinbox_topbox {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  justify-content: flex-end;
  align-items: flex-end;
  height: 18px;
  width: 45px;
  z-index: 3;
  /* background-color :green */
}

.lunboinbox_topbox_dayeventnum {
  height: 16px;
  width: 16px;
  border-radius: 2px;
 /*  background-color: #ff6f73; */
  margin-right: 2px;
  font: bold 10px / 16px '微软雅黑';
  color: #fff;
  text-align: center;
}
.graybgcolor{
  background-color: #b8bec3
}
.bluebgcolor{
 background-color: #63bfd4
}
.redbgcolor{
 background-color: #ff6f73
}
.lunboinbox_downbox {
  height: 42px;
  width: 45px;
  /* background-color: blue; */
}

.lunboinbox_downbox_fontoutboxmonthnum {
  height: 22px;
  width: 45px;
  color: #717f8d;
  font: bold 18px / 24px '微软雅黑';
  text-align: center;
  /* background-color :green */
}

.lunboinbox_downbox_fontoutboxweeknum {
  height: 20px;
  width: 45px;
  color: #717f8d;
  font: bold 12px / 16px '微软雅黑';
  text-align: center;
}

/* 修改前进后退按钮样式 */
.wrapper >>>.swiper-button-prev {
  opacity: 0;
  background-image: url('~@/assets/left1.svg') !important;
}

.wrapper:hover .swiper-button-prev {
  opacity: 1;
  background-image: url('~@/assets/left1.svg') !important;
  position: absolute !important;
  left: 0px !important;
  top: 22px !important;
  z-index: 59995 !important;
  background-color: #ffffff;
  width: 50px !important;
  height: 68px !important;
}

.wrapper >>>.swiper-button-next {
  opacity: 0;
  background-image: url('~@/assets/right.svg') !important;
}

.wrapper:hover .swiper-button-next {
  opacity: 1;
  background-image: url('~@/assets/right.svg') !important;
  position: absolute !important;
  right: 0px !important;
  top: 22px !important;
  z-index: 59995 !important;
  background-color: #ffffff;
  width: 50px !important;
  height: 68px !important;
}
</style>
