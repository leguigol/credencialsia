<template>
  <a-layout style="background-color: white;">
  <a-layout-header style="background-color: white; text-align: center; justify-content: center; height: 100px;">
    <h1>SIA CLUB</h1>
  </a-layout-header>
    <!-- <nav v-if="!userStore.loadinSession">
      <router-link to="/" v-if="userStore.userData">Home</router-link> |
      <router-link to="/login" v-if="!userStore.userData">Login</router-link> |
      <router-link to="/Register" v-if="!userStore.userData">Register</router-link> |
      <button @click="userStore.logoutUser" v-if="userStore.userData">Logout</button>
    </nav>
    <div v-else>
      loading user....
    </div> -->
    <a-layout-content style="padding: 0 50px">
      <div :style="{background: '#fff', padding: '24px', minHeight: '280px'} ">
          <router-view></router-view>
      </div>
    </a-layout-content>
    <a-layout-footer style="text-align: center;">
      SIA CLUB 2023 Creado Por Leguicode
    </a-layout-footer>
  </a-layout>
  
</template>

<script setup>

// import {useUserStore} from './stores/user';

// const userStore=useUserStore();

</script>

