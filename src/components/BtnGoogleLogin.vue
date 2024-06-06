<template>
    <div ref="googleLoginBtn" class="btn_google"></div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios';
import Llave from "../llave.js"
//import res from 'express/lib/response';
const googleLoginBtn = ref(null);
 onMounted(() => {
    //process.env.VUE_APP_ID_CLIENT_GOOGLE
    window.google.accounts.id.initialize({
        client_id: "115950979193-q0l4theofb51p2mrkvliaht35iginqll.apps.googleusercontent.com",
        callback: onSuccess,
        context: 'signin',
        auto_select: false,
        scope: process.env.VUE_APP_GOOGLE_SCOPES,
        referrerPolicy: {
            policy: 'strict-origin-when-cross-origin'
        }
    });
    window.google.accounts.id.renderButton(
        googleLoginBtn.value, {
        text: 'signin_with', // or 'signup_with' | 'continue_with' | 'signin'
        size: 'large', // or 'small' | 'medium'
        width: '366', // max width 400
        theme: 'outline', // or 'filled_black' |  'filled_blue'
        logo_alignment: 'center' // or 'center'
    });
});

   
function parseJwt(token) {
    const base64Url = token.split('.')[1];
    const base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
    /* Buffer.from(base64,'base64')
    decodeURIComponent(atob(base64).split('').map(function (c) */
    const jsonPayload = decodeURIComponent(atob(base64).split('').map(function (c) {
        return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
    }).join(''));

    return JSON.parse(jsonPayload);
}

function onSuccess(googleUser) {
    console.log("OnSuccess")
    console.log(googleUser);
    const user = parseJwt(googleUser.credential)
    console.log(user);
    llamarAlLogin(googleUser.credential);
    /* const name = user.name
    const imgURL = user.picture
    const email = user.email
    const id_token = googleUser.credential */
}


async function llamarAlLogin(token){
    console.log(token);
    let cuerpo = {
        token: token
    };
    let respuesta =  await axios.post("https://localhost:4005/login",cuerpo);
    Llave.llave = respuesta.data;
    console.log(respuesta.data);
    console.log(Llave.llave);
    
}

</script>
<style>
.btn_google {
    display: flex;
    justify-content: center;
}
</style>