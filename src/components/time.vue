<template>
  <div class="time">
    <slot :nowTime=nowTime>
      <span>{{nowTimes}}</span>
    </slot>
    
  </div>
</template>

<script setup>
import {reactive, ref ,onMounted } from "vue";
  const nowTimes = ref("")
  const nowTime = reactive({
    mod: "",
    year: "",
    month: "",
    day: "",
    hour: "",
    minute: "",
    second: "",
    chmonth: ""
  })

  const getnowTime = (nowTimes)=>{
      setInterval(() => {
          let date = new Date();
          nowTimes.value = date.toLocaleString();
          
          nowTime.mod = nowTimes.value.split(' ')[1].substring(0,2);
          nowTime.hour = nowTimes.value.split(' ')[1].substring(2).split(':')[0].padStart(2, '0');
          nowTime.minute = nowTimes.value.split(' ')[1].split(':')[1];
          nowTime.second = nowTimes.value.split(' ')[1].split(':')[2];
          nowTime.year = nowTimes.value.split(' ')[0].split('/')[0];
          nowTime.month = nowTimes.value.split(' ')[0].split('/')[1].padStart(2, '0');
          nowTime.day = nowTimes.value.split(' ')[0].split('/')[2].padStart(2, '0');

          nowTime.chmonth =ch_month(nowTime.month);
      }, 1000)
  }
  onMounted(() => {
      getnowTime(nowTimes)
  });
  //月份改中文
  const ch_month =(month)=>{
    switch (month) {
      case '01':
        return '一月';
      case '02':
        return '二月';
      case '03':
        return '三月';
      case '04':
        return '四月';
      case '05':
        return '五月';
      case '06':
        return '六月';
      case '07':
        return '七月';
      case '08':
        return '八月';
      case '09':
        return '九月';
      case '10':
        return '十月';
      case '11':
        return '十一月';
      case '12':
        return '十二月';
      default:
        return '';
    }
  }
</script>

<style>
  
  
</style>
