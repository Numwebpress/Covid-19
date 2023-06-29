<script setup>
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'


</script>

<template>
  <header>
    

    <div class="wrapper">
      <HelloWorld  msg="กราฟและตารางข้อมูลผู้ป่วย Covid-19" />

      <nav>
        <RouterLink to="/">ตาราง</RouterLink>
        <RouterLink to="/about">กราฟ</RouterLink>
      </nav>
    </div>
  </header>
  
  <RouterView  />
</template>

<style scoped>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.wrapper{
  margin-top: 100px;
}
nav {
  width: 100%;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
 
 

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
