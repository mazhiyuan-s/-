<template>
  <view class="index">
    <view>
      <img src="" alt="">
    </view>
    {{ msg }}
    <view class="btn">
      <nut-button type="primary" @click="handleClick('text', msg2, true)">点我</nut-button>
    </view>
    <nut-toast :msg="msg" v-model:visible="show" :type="type" :cover="cover" />
    <view class="test-calendar-wrapper">
      <nut-calendar
          type="range"
          :poppable="false"
          :default-value="date"
          :is-auto-back-fill="true"
          @choose="setChooseValue"
      >
      <template v-slot:bottomInfo="date">
      {{date.getDay}}
        <view v-for="i in dates">
          <span class="info" style="fontSize:12px;lineHeight:14px">{{
            date.date ? (date.date.day == i ? '十' :'') : ''
          }}</span>
        </view>
      </template>
      </nut-calendar>
    </view>
  </view>
</template>

<script>
import { reactive, toRefs } from 'vue';
export default {
  name: 'Index',
  components: {
    
  },
  setup(){
    const state = reactive({
      msg: '欢迎使用 NutUI3.0 开发小程序',
      msg2: '你成功了～',
      type: 'text',
      show: false,
      cover: false,
      date:[],
      dates:[]
    });

    const handleClick = (type, msg, cover = false) => {
      state.show = true;
      state.msg2 = msg;
      state.type = type;
      state.cover = cover;
    };
    const setChooseValue = param => {
    };
    function getMonthWeek(year, month) {//由于这里我以获取周一为例所以向上取整
        var d = new Date(year, month, 0);
        var t = Math.ceil(d.getDate()/7)
          return t;
    }
    function getMondayTime(year, month,weekday) {
      var d = new Date();
      // 该月第一天
      d.setFullYear(year, month-1, 1);
      var w1 = d.getDay();
      if (w1 == 0) w1 = 7;
      // 该月天数
      d.setFullYear(year, month, 0);
      var dd = d.getDate();
      // 第一个周一
      let d1;
      if (w1 != 1) d1 = 7 - w1 + 2;
      else d1 = 1;
      var monday = d1+(weekday-1)*7;
      return monday;
    }
    function gettime(){
        var ds=new Date();
      
        let month=ds.getMonth()+1;//当前月
        let weeks=getMonthWeek(ds.getFullYear(),month);//调用方法传入当前年和当前月获得本月几周
        //获取月第一个周一日期
        let oneDate=getMondayTime(ds.getFullYear(),month,2);
        var weeksall = [] //所有周一
          for(var i = 0 ;i<weeks;i++){
            weeksall.push(oneDate+i*7)
          }
      console.log(oneDate,weeksall)
      state.dates = weeksall
    }
    gettime()
    return {
      ...toRefs(state),
      handleClick,
      setChooseValue
    }
  }
}
</script>

<style lang="scss">
.index {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
.test-calendar-wrapper {
  display: flex;
  width: 100%;
  height: 560px;
  overflow: hidden;
}
</style>
