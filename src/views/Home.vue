<template>
    <a-row>
        <a-col :xs="{span: 24}" :sm="{span:12, offset:6}" :style="{textAlign: 'center'}">
            <h1>SIA CLUB</h1>
        </a-col>    
        <a-col :xs="{span: 24}" :sm="{span:12, offset:6}" :style="{textAlign: 'center'}">
            <ul style="list-style: none;">
              <li v-for="item of databaseStore.registerData" :key="item.id">
                <h2>Apellido y Nombre: {{ item.apellido }} {{ item.nombre }}</h2>
                <h2>DNI: {{ item.dni }}</h2>
                <h3>Forma de Suscripción: {{ item.forma_suscripcion }}</h3>
                <p>Fecha de Registro: {{ formatearFecha(item.fecha_registro.toDate()) }}</p>
              </li>  
            </ul>


            <!-- <p>{{ userStore.userData?.email }}</p> -->
            <QRCodeVue3
          :width="200"
          :height="200"
          :value="generateLinkWithUserData()"
          :qrOptions="{ typeNumber: 0, mode: 'Byte', errorCorrectionLevel: 'H' }"
          :imageOptions="{ hideBackgroundDots: true, imageSize: 0.4, margin: 0 }"
          :dotsOptions="{
            type: 'dots',
            color: '#26249a',
            gradient: {
              type: 'linear',
              rotation: 0,
              colorStops: [
                { offset: 0, color: '#26249a' },
                { offset: 1, color: '#26249a' },
              ],
            },
          }"
          :backgroundOptions="{ color: '#ffffff' }"
          :cornersSquareOptions="{ type: 'dot', color: '#000000' }"
          :cornersDotOptions="{ type: undefined, color: '#000000' }"
          fileExt="png"
          myclass="my-qur"
          imgclass="img-qr"
        />
        </a-col>    
    </a-row>
</template>
<script setup>
import {useDatabaseStore} from '../stores/database';
import { ref, onMounted } from 'vue';
// import { useRouter} from 'vue-router';
// import QRCode from 'qrcode';
import { auth } from '../firebaseConfig';
import QRCodeVue3 from "qrcode-vue3";


const databaseStore=useDatabaseStore();
const datosRegistro=ref([]);

databaseStore.datodelRegistrado();

//  onMounted( async() => {
//     datosRegistro.value=await databaseStore.obtenerRegistrado();

// });

const generateLinkWithUserData = () => {

//   const userData=JSON.stringify(auth.currentUser.uid)   ;
  const userData=auth.currentUser.uid;
  const link = 'https://silver-cobbler-d5eb3c.netlify.app/?id=';

  return `${link}${userData}`;
};

function formatearFecha(fecha) {
    const dia = fecha.getDate().toString().padStart(2, '0');
    const mes = (fecha.getMonth() + 1).toString().padStart(2, '0'); // Los meses en JavaScript van de 0 a 11
    const año = fecha.getFullYear();

    return `${dia}/${mes}/${año}`;
}



</script>

