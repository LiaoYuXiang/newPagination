<template>
  <div class="search-box">
      <slot class="name">
        <input type="text" placeholder="請輸入關鍵字" v-model="keyword" id="intext" />
      </slot>
      <slot name="btn">
        <button @click="search">Serch</button>
      </slot>
    </div>
    
  </template>
  
  <script setup>
    
    
  </script>
  
  <style>
    
  </style>
  