<template>
    <a-layout class="custom-layout">
      <a-layout-header :style="{ backgroundColor: 'white', marginTop: '150px', marginLeft: '20px', marginRight: '20px'}" v-if="!userStore.loadingSession">
        <a-menu 
          mode="horizontal"
          :style="{ lineHeight: '64px'}"
          v-model:selectedKeys="selectedKeys"

        >
            <a-menu-item v-if="userStore.userData" key="home">
              <router-link to="/">Home</router-link>        
            </a-menu-item>
            <a-menu-item v-if="!userStore.userData" key="login">
              <router-link to="/login">Login</router-link>
            </a-menu-item>
            <a-menu-item v-if="!userStore.userData" key="register">
              <router-link to="/Register">Registrarse</router-link>
            </a-menu-item>
            <a-menu-item @click="userStore.logoutUser" v-if="userStore.userData" key="logout">Cerrar Sesion</a-menu-item>                  
      </a-menu>  
      </a-layout-header>
      <a-layout-content style="padding: 0 50px">
        <div
          :style="{
            background: '#fff',
            padding: '24px',
            minHeight: '280px',
          }"        
        >
          <div v-if="userStore.loadingSession">loading user....</div>
          <router-view></router-view>
        </div>
      </a-layout-content>
  </a-layout>

</template>
<style>
.custom-layout {
  background-image: url('./assets/credencial_blanco.JPEG'); /* Cambia la ruta a la imagen */
  background-size: cover;
  background-position: center;
  min-height: 100vh;
  position: absolute;
  /* Otros estilos si es necesario */
}
</style>
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

