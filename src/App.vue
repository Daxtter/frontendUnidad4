<template>
  <v-app>
    
    <v-container>
      <v-navigation-drawer expand-on-hover rail >
        
        <v-list-item 
        title="Pagina Principal" 
        link to ="/" 
        prepend-icon="mdi-home" 
        ></v-list-item>
        
        <v-list-item 
        title="Imagenes y chistes" 
        link to ="/imagen" 
        prepend-icon = "mdi mdi-image-area"
        ></v-list-item>

        <v-list-item 
          title="tabla de ejemplo"
          link to ="/tablas"
          prepend-icon = "mdi mdi-database-arrow-right"
        ></v-list-item>
        <v-list-item 
          title="tabla Responsable"
          link to ="/tablasCRUD"
          prepend-icon = "mdi mdi-database"
        ></v-list-item>
        <v-list-item 
          title="tabla de Acceso"
          link to ="/tablaAcceso"
          prepend-icon = "mdi mdi-database"
        ></v-list-item>
        <v-list-item 
          title="tabla de activos"
          link to ="/tablaActivo"
          prepend-icon = "mdi mdi-database"
        ></v-list-item>
        <v-list-item 
          title="tabla de activo con tag"
          link to ="/tablaActivoTag"
          prepend-icon = "mdi mdi-database"
        ></v-list-item>

        <v-list-item 
          title="tabla identificadores"
          link to ="/tablaIdentificadores"
          prepend-icon = "mdi mdi-database"
        ></v-list-item>
        <v-list-item 
          title="tabla de tag"
          link to ="/tablaTag"
          prepend-icon = "mdi mdi-database"
        ></v-list-item>
        <v-list-item 
          title="tabla ubicaciones"
          link to ="/tablaUbicaciones"
          prepend-icon = "mdi mdi-database"
        ></v-list-item>
        <v-list-item 
          title="tabla de usuarios"
          link to ="/tablaUsuarios"
          prepend-icon = "mdi mdi-database"
        ></v-list-item>

      </v-navigation-drawer>
    
    </v-container>  
    <v-main>
      <router-view />
    </v-main>



  </v-app>
</template>

<script setup>

</script>
