<template>
    <a-layout class="custom-layout">
      <a-layout-header :style="{ padding: 0, marginTop: isSmallScreen ? '130px' : resolucion, marginLeft: '30px', marginRight: '30px', textAlign: 'center' }" v-if="!userStore.loadingSession">
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
          }"        
        >
          <div v-if="userStore.loadingSession">loading user....</div>
          <router-view></router-view>
          <!-- <div class="relleno">
          </div> -->
        </div>
      </a-layout-content>
  </a-layout>

</template>
<style>
body {
  margin: 0;
  padding: 0;
}
.relleno {
  height: 1px;
}
.custom-layout {

  background-image: url('./assets/credencial_blanco_768.jpg'); 
  background-repeat: no-repeat; 
  width: 100%;
  background-size: cover;
  /* background-position: center center;
  /* Otros estilos si es necesario */
}
</style>
<script setup>
import { onMounted, ref, computed, watch, onBeforeUnmount } from 'vue';
import {useUserStore} from './stores/user';
import { useRoute } from 'vue-router';

const userStore=useUserStore();
const route=useRoute();

const selectedKeys=ref([''] );
const isSmallScreen = ref(false);
const resolucion=ref('');

if (isSmallScreen.value) {
  headerStyle.value.marginTop = '100px';
}
const headerStyle = ref({
  padding: '0',
  marginTop: '200px'
  });

const checkScreenSize = () => {
  isSmallScreen.value = window.innerWidth < 650;
  let valu=parseInt(window.innerWidth)-400;
  resolucion.value=valu+"px";
  
  console.log('Ancho de pantalla:',resolucion.value);
};

// Verificar tamaño de pantalla al montar el componente
onMounted(() => {
  checkScreenSize();
  window.addEventListener('resize', checkScreenSize);
});

// Remover el evento al desmontar el componente
onBeforeUnmount(() => {
  window.removeEventListener('resize', checkScreenSize);
});


watch( 
  ()=> route.name, 
  () => {
    selectedKeys.value = [route.name]
  }
);

</script>

