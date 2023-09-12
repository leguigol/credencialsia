<template>

    <!-- <a-layout :style="{ padding: 0, marginBottom: '50px'}">
          <a-layout-header :style="{ padding: 0, marginTop: isSmallScreen ? '200px' : resolucion, marginLeft: '40px', marginRight: '40px', textAlign: 'center' }" v-if="!userStore.loadingSession">
              <a-menu 
                mode="horizontal"
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
            <a-layout-content style="padding: 0 50px"> -->
              <div class="div1">


                <a-menu 
                  mode="horizontal"
                  v-model:selectedKeys="selectedKeys"
                  :style="{ marginBottom: '50px'}"
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

                <div v-if="userStore.loadingSession">loading user....</div>
                <router-view></router-view>
              </div>
            <!-- </a-layout-content>
            <div class="relleno"></div>

      </a-layout> -->

</template>
<style>
@media (max-width: 320px) {

  body {
    background-image: url('./assets/credencial_blanco_320px.jpg');
    background-position: center center;
    background-repeat: no-repeat;
    background: cover;
    background-size: cover;
    background-attachment: fixed;
  }
}
@media (max-width: 375px) {
  body { 
    background-image: url('./assets/credencial_blanco_375.jpg');
    background-position: center center;
    background-repeat: no-repeat;
    background-size: cover;
    background-attachment: fixed;

  }

}

.div1 {
  margin-top: 150px;
  margin-right: 20px;
  margin-left: 20px;
  padding: 0px;

}

.relleno {
  height: 20vh;
}
</style>
<script setup>
import { ref, watch, onMounted, onBeforeUnmount } from 'vue';
import {useUserStore} from './stores/user';
import { useRoute } from 'vue-router';
import { Image } from "@unpic/vue";

const userStore=useUserStore();
const route=useRoute();

const selectedKeys=ref([''] );
const isSmallScreen = ref(false);
const resolucion=ref('');

const headerStyle = ref({
  padding: '0',
  marginTop: '100px'
});

if (isSmallScreen.value) {
  headerStyle.value.marginTop = '100px';
}

const checkScreenSize = () => {
  isSmallScreen.value = window.innerWidth < 553;
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

