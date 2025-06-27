<script setup>
import { RouterLink } from 'vue-router';
</script>
<template>
   <nav>
   <RouterLink to="/">Главная</RouterLink> 
   <RouterLink to="/about">О Нас</RouterLink> 
   <RouterLink to="/contact">Контакты</RouterLink>
    <RouterLink to="/VideoU">Загрузка видеолекций</RouterLink>
   </nav> 
</template>