
<script setup>
    import {ref} from 'vue';
    import Llave from "../llave.js";
    import axios from "axios";
    const token = Llave.llave;
    axios.defaults.headers.common = {'Authorization': `bearer ${token}`};
    const urlImagen = ref("https://picsum.photos/400/400");
    console.log("Ahora en imagen");
    console.log(Llave.llave);

    async function cambiaImagen()
    {
      //console.log(token);
      let respuesta2 = await axios.get("https://localhost:4005/pasaporte");
      console.log(respuesta2);
        let respuesta = await fetch ("https://picsum.photos/400/400");
        //console.log(respuesta);
        //console.log(urlImagen);
        urlImagen.value = respuesta.url;
    }

    const chiste = ref("") ;
    async function cambiarChiste(){
        let respuestaChiste = await fetch ('https://api.chucknorris.io/jokes/random');
        const datos = await respuestaChiste.json();

        chiste.value =  datos.value;
        //console.log("Dato obtenido es "+chiste);
    }
    cambiarChiste();
</script>

<template>
  <v-card
    class="mx-auto"
    max-width="600"
  >
    <v-img
      class="align-end text-white"
      height="400"
      :src = "urlImagen"
      cover
    >
      <v-card-title>imagen   al azar </v-card-title>
    </v-img>

    <v-card-subtitle class="pt-4">
        Chiste de Chuck Norris
    </v-card-subtitle>

    <v-card-text>
       {{chiste}}
    </v-card-text>

    <v-card-actions>
      <v-btn color="orange" @click="cambiaImagen">Nueva imagen</v-btn>
      
      <v-btn color="orange" @click="cambiarChiste">Cambiar chiste </v-btn>
    </v-card-actions>
  </v-card>
</template>
 


 