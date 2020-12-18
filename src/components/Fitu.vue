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

            <v-img
              src="@/assets/fitu.png"
              height="45px"
              max-width="300px"
            ></v-img>
          </v-list-item-title>
          <v-list-item-subtitle class="text-center caption">
            Pets
          </v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-layout mt-5 column align-center>
        <v-row>
          <v-col>
            <v-btn
              block
              depressed
              dark
              color="blue darken-3"
              @click="getPets()"
            >
              <v-icon>mdi-dog-side</v-icon>
              Pets
            </v-btn>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-btn block depressed dark color="blue darken-3" @click="addPet()">
              <v-icon>mdi-dog-side</v-icon>
              + Add Pets
            </v-btn>
          </v-col>
        </v-row>
      </v-layout>
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
        :items="pets"
        class="elevation-1"
        hide-default-footer
        locale="en"
      >
      <!--eslint-disable-->
        <template v-slot:item.acciones="{ item }">
          <!--eslint-disable-->
          <v-icon small class="mr-2" @click="editItem(item)">
            mdi-pencil
          </v-icon>
          <v-icon small @click="deletePet(item)"> mdi-delete </v-icon>
        </template>
      </v-data-table>
    </v-card>

    <v-main>
      <v-dialog v-model="dialogDelete" max-width="500px">
        <v-card>
          <v-card-title class="headline"
            >Are you sure you want to delete this item?</v-card-title
          >
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue darken-1" text 
              >Cancel</v-btn
            >
            <v-btn color="blue darken-1" text 
              >OK</v-btn
            >
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <div class="text-center">
        <v-row justify="center">
          <v-dialog
            scrollable
            v-model="dialogCreatePet"
            width="500px"
            persistent
          >
            <v-card hover light max-height="500">
              <v-card-title class="headline teal accent-4">
                Create a Pet
              </v-card-title>

              <v-card-text>
                <v-form ref="form" lazy-validation>
                  <v-text-field
                    append-icon="mdi-paw"
                    v-model="name"
                    label="Name:"
                    required
                  ></v-text-field>
                  <v-select
                    v-model="kind"
                    :items="kinds"
                    append-icon="mdi-paw"
                    menu-props="auto"
                    hide-details
                    label="Type:"
                    single-line
                  ></v-select>

                  <v-text-field
                    append-icon="mdi-paw"
                    v-model="breed"
                    label="Breed:"
                    required
                  ></v-text-field>
                  <v-text-field
                    append-icon="mdi-calendar-check"
                    v-model="age"
                    label="Age:"
                    required
                    type="number"
                  ></v-text-field>

                  <v-select
                    v-model="gender"
                    :items="genders"
                    append-icon="mdi-gender-male-female"
                    menu-props="auto"
                    hide-details
                    label="Gender:"
                    single-line
                  ></v-select>

                  <v-text-field
                    append-icon="mdi-palette"
                    v-model="color"
                    label="Color:"
                    required
                  ></v-text-field>
                </v-form>
              </v-card-text>
              <v-divider></v-divider>

              <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn
                  color="error"
                  elevation="24"
                  @click="dialogCreatePet = false"
                  outlined
                  >Cancelar</v-btn
                >
                <v-btn
                  color="success"
                  @click="createPet()"
                  elevation="24"
                  outlined
                  >Crear</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: "Fitu",
  data() {
    return {
      name: "",
      kind: "",
      breed: "",
      age: "",
      gender: "",
      color: "",

      kinds: ["cat", "dog", "hamster", "bird", "fish", "turtle", "rabbit"],
      genders: ["male", "female"],
      pets: [],
      dialogCreatePet: false,
      dialogDelete: false,
      yesornoDelete: false,
      items: [
        { title: "Pets", icon: "mdi-dog-side" },
        { title: "Add pets", icon: "mdi-dog-side", onclick: "addPet()" },
      ],
      right: null,
      headers: [
        {
          text: "ID",

          value: "id",

          class: "teal--text text--darken-4",
        },
        {
          text: "Name",
          value: "name",

          class: "teal--text text--darken-4",
        },
        {
          text: "Type",
          value: "kind",

          class: "teal--text text--darken-4",
        },
        {
          text: "Color",
          value: "color",

          class: "teal--text text--darken-4",
        },
        {
          text: "Breed",
          value: "breed",

          class: "teal--text text--darken-4",
        },
        {
          text: "Gender",
          value: "gender",

          class: "teal--text text--darken-4",
        },
        {
          text: "Age",
          value: "age",

          class: "teal--text text--darken-4",
        },
        {
          text: "Created at",
          value: "created_at",

          class: "teal--text text--darken-4",
        },
        {
          text: "Actions",

          value: "acciones",

          class: "teal--text text--darken-4",
        },
      ],
    };
  },
  methods: {
    getPets() {
      const params = new URLSearchParams([
        ["token", "7b64d09060db17ca6b96c0af99575903"],
      ]);
      axios
        .get("http://api-pets.fituapp.com/api/v1/pets", { params })
        .then((result) => {
          console.log(result);
          this.pets = result.data;
          //result.data.forEach(element => {
          //console.log(element.created_at);
          //let arrayFecha= element.created_at.split(['-'])
          //console.log(arrayFecha)

          //var separarFecha_hora=arrayFecha[2]
          //console.log(separarFecha_hora)
        });

      // this.separarFecha_hora=this.separarFecha_hora.forEach(letras => {
      // let arrayHoraPartida= this.separarFecha_hora.charAt(letras);
      // console.log(arrayHoraPartida)
      //  });
      //.array.forEach(letras => {
      //console.log(letras);

      //arrayFecha[2].split(['-'])
      //console.log(separarFecha_hora)
      //element.created_at
      //});
    },

    deletePet(item) {
      console.log(item.id);
      //this.dialogDelete=true;
      const params = new URLSearchParams([
        ["token", "7b64d09060db17ca6b96c0af99575903"],
      ]);
      axios
        .delete(`http://api-pets.fituapp.com/api/v1/pets/${item.id}`, {
          params,
        })
        .then((result) => {
          this.getPets();
          console.log(result);
          //this.pets = result.data;
          // console.log(result.data[0]);
        });
    },
    addPet() {
      this.dialogCreatePet = true;
    },
    createPet() {
      const params = new URLSearchParams([
        ["token", "7b64d09060db17ca6b96c0af99575903"],
      ]);
      let newPet = {
        name: this.name,
        kind: this.kind,
        breed: this.breed,
        age: this.age,
        gender: this.gender,
        color: this.color,
      };
      axios
        .post("http://api-pets.fituapp.com/api/v1/pets", newPet, { params })
        .then((result) => {
          console.log(result);
          this.cancelar();
          this.getPets();
        });
    },
    cancelar() {
      this.dialogCreatePet = false;
    },
    cancelarEliminar() {
      this.dialogDelete=false;
     // this.yesornoDelete=false;
    }
  },
  watch: {},
  created() {
    this.getPets();

    //  let token='7b64d09060db17ca6b96c0af99575903'
    //const res = axios.get(`http://api-pets.fituapp.com/api/v1/pets?${token}`, {
    //headers: {
    //authorization: '7b64d09060db17ca6b96c0af99575903',
    // Content-Type: 'application/json',
    //}
    //})
    //console.log(res)

    // {
    // headers: {
    //authorization: '7b64d09060db17ca6b96c0af99575903',
    // authorization: "Access-Control-Allow-Origin ",
    // },
    //}

    // console.log(this.pets.data);
  },
};
</script>

<style>
</style>