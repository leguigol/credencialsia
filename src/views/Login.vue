<template>
    <a-row>
        <a-col :span="24">
            <a-form
                :model="formState"
                name="basicLogin"
                autocomplete="off"
                layout="vertical"
                @finish="onFinish"
                @finishFailed="onFinishFailed"
            >

                <a-form-item
                    label="Ingresa tu correo"
                    name="email"
                    :rules="[{ required: true, type: 'email', message: 'Por favor ingresa tu correo electronico' }]"
                >
                    <a-input v-model:value="formState.email" />
                </a-form-item>
                <a-form-item
                    label="Ingrese contraseña"
                    name="password"
                    :rules="[{ required: true, min: 6, message: 'Por favor ingresa una contraseña minimo 6 caracteres' }]"
                >
                    <a-input-password v-model:value="formState.password" />
                </a-form-item>
                <a-form-item :style="{textAlign: 'center'}">
                    <a-button type="primary" html-type="submit" :disabled="userStore.loadingUser" :style="{ width: '50%'}">Ingresar</a-button>
                </a-form-item>
            </a-form>
        
        </a-col>
    </a-row>

</template>

<script setup>
import { reactive } from 'vue';
import {useUserStore} from '../stores/user';
import { message } from 'ant-design-vue';

const userStore=useUserStore();

const formState=reactive({
    email: '',
    password: '',
});


const onFinish=async(values) => {

    const error=await userStore.loginUser(formState.email,formState.password);
    
    if(!error){
        return;
    }

    switch(error){
        case 'auth/user-not-found':
            message.error('No existe el correo electronico registrado');
            break;
        case 'auth/wrong-password':
            message.error('Error de contraseña');
            break;
        default:
            message.error('Error en el servidor');
            break;    
    }

};
const onFinishFailed= errorInfo => {
    console.log('failed:'+errorInfo);
};

</script>

