<template>
  <div class="main">

    <!-- 時鐘 -->
    <time_show #default=Time>
      <span class="time-t">{{ `${Time.nowTime.hour}:${Time.nowTime.minute}:${Time.nowTime.second}` }}</span>
      <br/>
      <span class="time-d">{{ `${Time.nowTime.chmonth} ${Time.nowTime.day}` }}</span>
    </time_show>
    <!-- 搜尋框 -->
    <search class="search-box">
      <input id="Search_inp" type="text" placeholder="Search" v-model="keyword" v-on:keydown="onEnter()"/>
      <template #btn >
        <button class="search-btn" v-show ="keyword != ''" @click="keyword = ''"><span>X</span></button>
        <button class="search-btn" v-show ="keyword != ''" @click="seabtn()" id="ent"><img src="./assets/magnifying-glass.svg"></button>
      </template>
    </search>
  </div>

  <!-- 右側書籤 -->
  <div class="iconlist">
    <iconlist v-for=" bm in bookmarks">
      <p class="icon-text" >{{ bm.name }}</p>
      <template #icon>
        <a :href=bm.URL class="icon-img-out">
          <img :src="bm.icon" class="icon-img"/>
        </a>
      </template>
    </iconlist>
  </div>
  
  
</template>

<script setup>
  import iconlist from './components/iconlist.vue';
  import time_show from './components/time.vue';
  import search from './components/search.vue';
  import settingJson from '../public/setting.json';
  import {reactive, ref ,onMounted } from "vue";
  const bookmarks = settingJson.bookmark;
  const serchURL = settingJson.searchURL;
  const Time = ref('');
  const keyword = ref('');

  const seabtn = ()=>{
    window.location.replace(serchURL+encodeURIComponent(keyword.value));
    keyword.value = '';
  }
  const onEnter = ()=>{
    if(event.key === 'Enter'){
      seabtn();
    }
  }

  
  
  
  
</script>

<style>
  :root{
    --font-family: MComic HK;
    --icon-size: max(8vh,50px);
    --icon-text-size: 0.8rem;
    /* --icon-text-color: #fff; */
    --icon-text-color: rgba(0,0,0,0);
    /* --text-shadow-alpha :0.35; */
    --text-shadow-alpha :0;
    --search-box-shadow-alpha :0.35;
    --time-text-shadow-alpha :0.45;
    

    }
  #app {
    font-family: var(--font-family),微軟正黑體,Arial, sans-serif;
    display: grid;
    grid-template-columns: 10% 80% 10%;
    grid-template-areas:
    "null main linkblocks";
    justify-content: center;
    align-items: center;
    padding: 0;
    overflow: hidden;
    width: 100%;
    user-select: none;
  }
  
  /* === time === */
  .main {
    display: grid;
    width: 100%;
    height: 100%;
    justify-items: center;
    grid-template-rows: 1fr 1.5fr 3fr;
    grid-template-areas:
    "main-time"
    "main-search"
    "main-weather";
    margin: 0 auto;
    text-align: center;
    grid-area:main;
  }
  .time{
    color: #fff;
    text-shadow:1px 2px 6px rgba(0, 0, 1, var(--time-text-shadow-alpha));
  }
  .time-t{
    display: inline-block;
    font-size: 4.9rem;
    margin-bottom: -2rem;
  }
  .time-d{
    display: inline-block;
    font-size: 1.5rem;

  }
  /* === 搜尋 === */
  .search-box {
    grid-area:main-search;
    
    background-color: rgba(180, 180, 180, 0.35);
    border-radius: 10px;
    display: flex;
    overflow: hidden;

    backdrop-filter: blur(10px);

    min-width: 20rem;
    max-width: 40rem;
    width: 30vw;
    height: 42px;
    box-shadow:1px 2px 6px rgba(0, 0, 0, var(--search-box-shadow-alpha));
    transition: width  0.5s ;
    input{
      width: 100%;
      height: 100%;
      outline: none;
      border: none;
      color: #fff; 
      padding: 0 10px;
      background-color: rgba(255, 255, 255, 0);
      text-shadow:1px 2px -1px rgba(0, 0, 0, var(--search-box-shadow-alpha));
    } 
  }
  .search-box input::placeholder{
      color: #fff; 
    }
  .search-box:hover{
    width: 40vw;
    background-color: rgba(180, 180, 180, 0.35);

  }
  .search-btn{
    height: 100%;
    padding: 0;
    color: #fff;
    height: 100%;
    width: 3rem;
    border-radius: 0;
    outline: none;
    border: none;
    background-color: rgba(255, 255, 255, 0);
    

    img,span{
      background-color: rgba(255, 255, 255, 0);
      height: 1.2em;
      width: 1.2em;
    }
    img{
      margin-bottom: -0.2rem;
    }
  }
  .search-btn:focus{
      outline: none;
      border: none;
    }
  .search-btn:hover{
      background-color: rgba(255, 255, 255, 0.35);
    }
  /* === icon === */
  .icon-text{
    margin: -9px auto -1px auto;
    font-size: var(--icon-text-size);
    color: var(--icon-text-color);
    text-shadow:1px 2px 6px rgba(0, 0, 0, var(--text-shadow-alpha))
  }
  .icon-img{
    width:90%;
    margin: calc((50% - 45%)) auto;
    transition: all .1s;
    border-radius:1rem;
    
  }
  .icon-img-out{
    --icon-outsize: calc(var(--icon-size)*1.1);
    width:--icon-outsize;
    height: --icon-outsize;
  }

  .icon-img:hover{
    width: 100%;
    margin: 0 auto;
    box-shadow: 1px 2px 6px rgba(0, 0, 0, var(--text-shadow-alpha));
  }
  .icon-obj{
    width: var(--icon-size);
    margin: 0 auto;
    transition: all .5s;
  }
  .iconlist{
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: center;
    width: calc(var(--icon-size)*1.1);
    margin: 5px auto;
    grid-area:linkblocks;
    overflow: hidden;
  }
  
  
</style>
