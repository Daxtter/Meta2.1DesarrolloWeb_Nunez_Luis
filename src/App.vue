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
          title="tabla"
          link to ="/tablas"
          prepend-icon = "mdi mdi-database-arrow-right"
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
