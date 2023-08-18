<template>
  <a-layout>
      <a-layout-header v-if="!userStore.loadinSession">
        <a-menu 
          theme="dark "
          mode="horizontal"
          :style="{ lineHeight: '64px'}"
          v-model:selectedKeys="selectedKeys"
        >
            <a-menu-item v-if="userStore.userData" key="home">
              <router-link to="/">Home</router-link> |        
            </a-menu-item>
            <a-menu-item v-if="!userStore.userData" key="login">
              <router-link to="/login">Login</router-link> |
            </a-menu-item>
            <a-menu-item v-if="!userStore.userData" key="register">
              <router-link to="/Register">Register</router-link> |
            </a-menu-item>
            <a-menu-item @click="userStore.logoutUser" v-if="userStore.userData" key="logout">Logout</a-menu-item>                  
      </a-menu>  
      </a-layout-header>
  </a-layout>
</template>

<script setup>
import { ref, watch } from 'vue';
import {useUserStore} from './stores/user';
import { useRoute } from 'vue-router';

const userStore=useUserStore();
const route=useRoute();

const selectedKeys=ref([''] );

watch( 
  ()=> route.name, 
  () => {
    selectedKeys.value = [route.name]
  }
);

</script>

