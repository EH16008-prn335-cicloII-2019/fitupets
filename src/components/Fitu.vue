<template>
  <v-app>
    <v-card>
      <v-toolbar
        app
        class="line"
        flat
        shrink-on-scroll
        scroll-target="#scrolling-techniques-2"
        md="4"
        cols="12"
      >
        <v-spacer></v-spacer>

        <p class="title teal--text text--darken-4">Admin</p>

        <v-btn class="d-flex mt-8" color="teal accent-4" dark small
          >Fitu Pets</v-btn
        >
      </v-toolbar>
    </v-card>

    <v-navigation-drawer dark permanent app color="blue darken-3">
      <v-list-item color="blue darken-4">
        <v-list-item-content>
          <v-list-item-title class="title text-center">
            fitu
          </v-list-item-title>
          <v-list-item-subtitle class="text-center caption">
            Pets
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list dense nav class="align-center">
        <v-list-item v-for="item in items" :key="item.title" link>
          <v-list-item-icon>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-card>
      <v-tabs color="blue darken-3">
        <v-tab>All</v-tab>
      </v-tabs>
    </v-card>
    <p class="title ml-14 mt-12 teal--text text--darken-4">Pets</p>

    <v-card class="mx-auto" width="90%" outlined>
      <v-data-table
        :headers="headers"
        :items="desserts"
        class="elevation-1"
        hide-default-footer
      >
      </v-data-table>
    </v-card>

    <v-main> </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: "Fitu",
  data() {
    return {
      items: [
        { title: "Pets", icon: "mdi-dog-side" },
        { title: "Add pets", icon: "mdi-dog-side" },
      ],
      right: null,
      headers: [
        {
          text: "ID",
          
          value: "id",
        },
        { text: "Name", value: "nombre" },
        { text: "Type", value: "tipo" },
        { text: "Color", value: "color" },
        { text: "Breed", value: "raza" },
        { text: "Gender", value: "genero" },
         { text: "Age", value: "edad" },
          { text: "Created at", value: "fecha" },
      ],
      desserts: [
        {
          name: "Frozen Yogurt",
          calories: 159,
          fat: 6.0,
          carbs: 24,
          protein: 4.0,
          iron: "1%",
        },
        {
          name: "Ice ",
          calories: 237,
          fat: 9.0,
          carbs: 37,
          protein: 4.3,
          iron: "1%",
        },
        {
          name: "Eclair",
          calories: 262,
          fat: 16.0,
          carbs: 23,
          protein: 6.0,
          iron: "7%",
        },
        {
          name: "Cupcake",
          calories: 305,
          fat: 3.7,
          carbs: 67,
          protein: 4.3,
          iron: "8%",
        },
        {
          name: "Gingerbread",
          calories: 356,
          fat: 16.0,
          carbs: 49,
          protein: 3.9,
          iron: "16%",
        },
        {
          name: "Jelly bean",
          calories: 375,
          fat: 0.0,
          carbs: 94,
          protein: 0.0,
          iron: "0%",
        },
        {
          name: "Lollipop",
          calories: 392,
          fat: 0.2,
          carbs: 98,
          protein: 0,
          iron: "2%",
        },
        {
          name: "Honeycomb",
          calories: 408,
          fat: 3.2,
          carbs: 87,
          protein: 6.5,
          iron: "45%",
        },
        {
          name: "Donut",
          calories: 452,
          fat: 25.0,
          carbs: 51,
          protein: 4.9,
          iron: "22%",
        },
        {
          name: "KitKat",
          calories: 518,
          fat: 26.0,
          carbs: 65,
          protein: 7,
          iron: "6%",
        },
      ],
    };
  },


  created(){
  //  let token='7b64d09060db17ca6b96c0af99575903'
//const res = axios.get(`http://api-pets.fituapp.com/api/v1/pets?${token}`, {
  //headers: {
    //authorization: '7b64d09060db17ca6b96c0af99575903',
   // Content-Type: 'application/json',
  //}
//})
  //console.log(res)
  const params = new URLSearchParams([['token', '7b64d09060db17ca6b96c0af99575903']]);
  axios.get("http://api-pets.fituapp.com/api/v1/pets", { params },
  
   {
  headers: {
    //authorization: '7b64d09060db17ca6b96c0af99575903',
    authorization: 'Access-Control-Allow-Origin '
    }
    
    }).then((result) => {
      console.log(result);
    })
  }
 

 
  

  

};
</script>

<style>
</style>