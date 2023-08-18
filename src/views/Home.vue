<template>
    <div>
        <div v-if="datosRegistro">
        <h2>Apellido y Nombre: {{ datosRegistro.apellido }} {{ datosRegistro.nombre }}</h2>
        <h2>DNI: {{ datosRegistro.dni }}</h2>
        <h3>Forma de Suscripción: {{ datosRegistro.forma_suscripcion }}</h3>
        <p>Fecha de Registro: {{ datosRegistro.fecha_registro.toDate() }}</p>
        </div>

        <p>{{ userStore.userData?.email }}</p>

        <div ref="qrCode"></div>
    </div>
</template>

<script setup>
import {useUserStore} from '../stores/user';
import {useDatabaseStore} from '../stores/database';
import { ref, onMounted } from 'vue';
// import { useRouter} from 'vue-router';
import QRCode from 'qrcode';
import { auth } from '../firebaseConfig';

const userStore=useUserStore();
const databaseStore=useDatabaseStore();
const qrCode=ref(null);
const datosRegistro=ref(null);
const fechaRegistro=ref(null);

// const router=useRouter();


// if(userStore.userData){
//     alert(userStore.userData.uid);
//     datosRegistro.value=await databaseStore.obtenerRegistrado(userStore.userData.uid);
// }

onMounted( async() => {
    await obtenerDatosRegistro();
    generateQRCode(datosRegistro);

});

async function generateQRCode(datosR){
    const userData=JSON.stringify(userStore.userData);
    const registerData=JSON.stringify(datosR.value);
    const combineData={userData, registerData};
    const combineDataString=JSON.stringify(combineData);
    console.log('regData en el qr ?'+JSON.stringify(combineDataString));

    const canvas=await QRCode.toCanvas(combineDataString);

    if(qrCode.value){
        qrCode.value.innerHTML='';
        qrCode.value.appendChild(canvas);
    }
}

async function obtenerDatosRegistro(){
    const idUser=auth.currentUser.uid;
    datosRegistro.value=await databaseStore.obtenerRegistrado(idUser);
    fechaRegistro.value = datosRegistro.value.fecha_registro.toDate();
    console.log("Fecha registro"+datosRegistro.value.fecha_registro.toDate());
}

function formatearFecha(fecha) {
    const dia = fecha.getDate().toString().padStart(2, '0');
    const mes = (fecha.getMonth() + 1).toString().padStart(2, '0'); // Los meses en JavaScript van de 0 a 11
    const año = fecha.getFullYear();

    return `${dia}/${mes}/${año}`;
}

// databaseStore.getUrls();

// const url=ref('');

// const handleSubmit= () => {
//     // console.log('submit')
//     // validaciones de esa url

//     databaseStore.addUrl(url.value);

// }
</script>

