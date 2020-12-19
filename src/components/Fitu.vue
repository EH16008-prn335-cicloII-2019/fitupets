<template>
  <v-app>
    <v-card>
      <v-app-bar app class="line" flat md="4" cols="12">
        <v-spacer></v-spacer>

        <p class="title teal--text text--darken-4">Admin</p>

        <v-btn class="d-flex mt-8" color="teal accent-4" dark small
          >Fitu Pets</v-btn
        >
      </v-app-bar>
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
    <p class="title ml-10 mt-6 teal--text text--darken-4">Pets</p>

    <v-card class="mx-auto" width="96%" outlined>
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
            mdi-pencil-outline
          </v-icon>
          <v-icon small @click="accionaEliminar(item)">
            mdi-trash-can-outline
          </v-icon>
        </template>
        <template v-slot:item.color="{ item }">
          <v-avatar :color="getColor(item.color)" dark size="15"> </v-avatar>
        </template>
      </v-data-table>
    </v-card>

    <v-main>
      <div class="text-center" v-if="formAgregar">
        <v-row justify="center">
          <v-dialog
            scrollable
            v-model="dialogCreatePet"
            width="500px"
            persistent
          >
            <v-card hover light max-height="500">
              <v-card-title class="headline success">
                Create a Pet
              </v-card-title>

              <v-card-text>
                <v-form ref="form" v-model="valid">
                  <v-text-field
                    append-icon="mdi-paw"
                    v-model="name"
                    label="Name:"
                    required
                    :rules="nameRules"
                    :counter="10"
                  ></v-text-field>
                  <v-select
                    v-model="kind"
                    :items="kinds"
                    append-icon="mdi-paw"
                    menu-props="auto"
                    required
                    hide-details
                    label="Type:"
                    single-line
                    :rules="[(v) => !!v || 'Type is required']"
                  ></v-select>

                  <v-text-field
                    append-icon="mdi-paw"
                    v-model="breed"
                    label="Breed:"
                    required
                    :rules="breedRules"
                    :counter="20"
                  ></v-text-field>
                  <v-text-field
                    append-icon="mdi-calendar-check"
                    v-model="age"
                    label="Age:"
                    required
                    type="number"
                    min="0"
                    :rules="ageRules"
                  ></v-text-field>

                  <v-select
                    v-model="gender"
                    :items="genders"
                    append-icon="mdi-gender-male-female"
                    menu-props="auto"
                    hide-details
                    label="Gender:"
                    single-line
                    required
                    :rules="[(v) => !!v || 'Gender is required']"
                  ></v-select>

                  <div class="text-center">
                    <v-col>
                      <v-chip class="ma-2" color="success">
                        <v-icon left> mdi-palette</v-icon>
                        Pick a Color:
                      </v-chip>

                      <v-color-picker
                        append-icon="mdi-palette"
                        dot-size="27"
                        v-model="color"
                        required
                        mode="hexa"
                      ></v-color-picker>
                    </v-col>
                  </div>
                </v-form>
              </v-card-text>
              <v-divider></v-divider>

              <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn
                  color="error"
                  elevation="24"
                  @click="dialogCreatePet = false"
                  >Cancel</v-btn
                >
                <v-btn
                  color="success"
                  :disabled="!valid"
                  @click="createPet()"
                  elevation="24"
                  >Create</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </div>

      <div class="text-center" v-if="!formAgregar">
        <v-row justify="center">
          <v-dialog
            scrollable
            v-model="dialogCreatePet"
            width="500px"
            persistent
          >
            <v-card hover light max-height="500">
              <v-card-title class="headline indigo darken-4 white--text"> Edit a Pet </v-card-title>

              <v-card-text>
                <v-form ref="form" v-model="validEdit">
                  <v-text-field
                    append-icon="mdi-paw"
                    v-model="name"
                    label="Name:"
                    required
                    :rules="nameRules"
                    :counter="10"
                  ></v-text-field>
                  <v-select
                    v-model="kind"
                    :items="kinds"
                    append-icon="mdi-paw"
                    menu-props="auto"
                    required
                    hide-details
                    label="Type:"
                    single-line
                    :rules="[(v) => !!v || 'Type is required']"
                  ></v-select>

                  <v-text-field
                    append-icon="mdi-paw"
                    v-model="breed"
                    label="Breed:"
                    required
                    :rules="breedRules"
                    :counter="20"
                  ></v-text-field>
                  <v-text-field
                    append-icon="mdi-calendar-check"
                    v-model="age"
                    label="Age:"
                    required
                    type="number"
                    min="0"
                    :rules="[(v) => !!v || 'Gender is required']"
                  ></v-text-field>

                  <v-select
                    v-model="gender"
                    :items="genders"
                    append-icon="mdi-gender-male-female"
                    menu-props="auto"
                    hide-details
                    label="Gender:"
                    single-line
                    required
                    :rules="[(v) => !!v || 'Gender is required']"
                  ></v-select>

                  <div class="text-center">
                    <v-col>
                      <v-chip class="ma-2" color="success">
                        <v-icon left> mdi-palette</v-icon>
                        Pick a Color:
                      </v-chip>

                      <v-color-picker
                        append-icon="mdi-palette"
                        dot-size="27"
                        v-model="color"
                        required
                        mode="hexa"
                      ></v-color-picker>
                    </v-col>
                  </div>
                </v-form>
              </v-card-text>
              <v-divider></v-divider>

              <v-card-actions>
                <v-spacer></v-spacer>

                <v-btn color="warning" elevation="24" dark @click="cancelarEdit()"
                  >Cancel</v-btn
                >
                <v-btn
                  dark
                  color="teal accent-4"
                  :disabled="!validEdit"
                  @click="editarFinal()"
                  elevation="24"
                  >Edit</v-btn
                >
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-row>
      </div>
      <v-dialog v-model="dialogDelete" max-width="500px">
        <v-card color="indigo darken-4">
          <v-card-title class="headline white--text"
            >Are you sure you want to delete this Pet?</v-card-title
          >
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="warning" dark @click="dialogDelete = false">Cancel</v-btn>
            <v-btn color="teal accent-4" dark @click="deletePet()">Delete</v-btn>
            <v-spacer></v-spacer>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <div class="text-center ma-2">
        <v-snackbar v-model="snackbarNew" color="success">
          {{ textSnackNew }}
          <v-btn class="ml-16" color="error" text @click="snackNewcerrada">
            <svg style="width: 24px; height: 24px" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2C6.47,2 2,6.47 2,12C2,17.53 6.47,22 12,22C17.53,22 22,17.53 22,12C22,6.47 17.53,2 12,2M14.59,8L12,10.59L9.41,8L8,9.41L10.59,12L8,14.59L9.41,16L12,13.41L14.59,16L16,14.59L13.41,12L16,9.41L14.59,8Z"
              />
            </svg>
          </v-btn>
        </v-snackbar>
      </div>

      <div class="text-center ma-2">
        <v-snackbar v-model="snackbarDelete" color="error">
          {{ textSnackDelete }}
          <v-btn class="ml-16" color="success" text @click="snackDeletecerrada">
            <svg style="width: 24px; height: 24px" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2C6.47,2 2,6.47 2,12C2,17.53 6.47,22 12,22C17.53,22 22,17.53 22,12C22,6.47 17.53,2 12,2M14.59,8L12,10.59L9.41,8L8,9.41L10.59,12L8,14.59L9.41,16L12,13.41L14.59,16L16,14.59L13.41,12L16,9.41L14.59,8Z"
              />
            </svg>
          </v-btn>
        </v-snackbar>
      </div>

      <div class="text-center ma-2">
        <v-snackbar v-model="snackbarEdit" color="warning">
          {{ textSnackEdit }}
          <v-btn class="ml-16" color="error" text @click="snackEditcerrada">
            <svg style="width: 24px; height: 24px" viewBox="0 0 24 24">
              <path
                fill="currentColor"
                d="M12,20C7.59,20 4,16.41 4,12C4,7.59 7.59,4 12,4C16.41,4 20,7.59 20,12C20,16.41 16.41,20 12,20M12,2C6.47,2 2,6.47 2,12C2,17.53 6.47,22 12,22C17.53,22 22,17.53 22,12C22,6.47 17.53,2 12,2M14.59,8L12,10.59L9.41,8L8,9.41L10.59,12L8,14.59L9.41,16L12,13.41L14.59,16L16,14.59L13.41,12L16,9.41L14.59,8Z"
              />
            </svg>
          </v-btn>
        </v-snackbar>
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
      nameRules: [
        (v) => !!v || "The Name Is Required",
        (v) => (v && v.length <= 10) || "Name must be less than 10 characters",
      ],
      breedRules: [
        (v) => !!v || "The Breed Is Required",
        (v) => (v && v.length <= 20) || "Breed must be less than 20 characters",
      ],
      ageRules: [
        (v) => !!v || "The Age Is Required",
        (v) => (v && v.length <= 2) || "Age must be less than 99 years",
      ],
      date: new Date().toISOString().substr(0, 10),

      valid: false,
      validEdit: false,
      snackbarNew: false,
      snackbarDelete: false,
      snackbarEdit: false,
      textSnackNew: "Pet Created!!! Thanks ",
      textSnackDelete: "Pet Deleted!!! Thanks ",
      textSnackEdit: "Pet Edited!!! Thanks ",
      itemEdit: "",
      itemEliminar: "",
      formAgregar: true,
      name: "",
      kind: "",
      breed: "",
      age: "",
      gender: "",
      color: "",
      dateCreatedAt: "",

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
    getColor(color) {
      return color;
    },

    getFecha(fecha) {
      return fecha;
    },
    validate() {
      this.$refs.form.validate();
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },

    getPets() {
      const params = new URLSearchParams([
        ["token", "7b64d09060db17ca6b96c0af99575903"],
      ]);
      axios
        .get("http://api-pets.fituapp.com/api/v1/pets", { params })
        .then((result) => {
          this.getColor();
          this.getFecha();
          this.pets = result.data;

          result.data.forEach((element) => {
            console.log(element.created_at);

            element.created_at =
              new Date(element.created_at).toDateString() +
              "," +
              new Date(element.created_at)
                .toLocaleTimeString([], {
                  hour: "2-digit",
                  minute: "2-digit",
                  hour12: true,
                })
                .toLowerCase();
            console.log(typeof element.created_at);
          });
         
        });
    },

    snackNewcerrada() {
      this.snackbarNew = false;
    },
    snackDeletecerrada() {
      this.snackbarDelete = false;
    },
    snackEditcerrada() {
      this.snackbarEdit = false;
    },

    accionaEliminar(item) {
      this.itemEliminar = item.id;
      console.log(this.itemEliminar);
      this.dialogDelete = true;
    },

    deletePet() {
      const params = new URLSearchParams([
        ["token", "7b64d09060db17ca6b96c0af99575903"],
      ]);
      axios
        .delete(
          `http://api-pets.fituapp.com/api/v1/pets/${this.itemEliminar}`,
          {
            params,
          }
        )
        .then((result) => {
          this.getPets();
          console.log(result);
          this.dialogDelete = false;
          this.snackbarDelete = true;
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
          this.reset();
          this.snackbarNew = true;
        });
    },
    cancelar() {
      this.dialogCreatePet = false;
    },
    cancelarEdit() {
      this.dialogCreatePet = false;
      this.formAgregar = true;
      
    },
    cancelarEliminar() {
      this.dialogDelete = false;
    },
    editItem(item) {
      this.formAgregar = false;
      this.addPet();
      console.log(item);
      this.name = item.name;
      this.kind = item.kind;
      this.breed = item.breed;
      this.age = item.age;
      this.gender = item.gender;
      this.color = item.color;
      this.itemEdit = item;
      console.log(this.itemEdit);
    },

    editarFinal() {
      console.log(this.itemEdit.id);
      this.itemEdit.name = this.name;
      this.itemEdit.kind = this.kind;
      this.itemEdit.breed = this.breed;
      this.itemEdit.age = this.age;
      this.itemEdit.gender = this.gender;
      this.itemEdit.color = this.color;
      console.log(this.itemEdit.name);
      console.log(this.name);
      const params = new URLSearchParams([
        ["token", "7b64d09060db17ca6b96c0af99575903"],
      ]);

      let editPet = {
        name: this.name,
        kind: this.kind,
        breed: this.breed,
        age: this.age,
        gender: this.gender,
        color: this.color,
      };
      axios
        .put(
          `http://api-pets.fituapp.com/api/v1/pets/${this.itemEdit.id}`,
          editPet,
          { params }
        )
        .then((result) => {
          console.log(result);
          this.formAgregar = true;
          this.cancelar();
          this.getPets();
          this.snackbarEdit = true;
          
          this.name = "";
          this.kind = "";
          this.breed = "";
          this.age = "";
          this.gender = "";
        });
    },
  },
  watch: {},
  created() {
    this.getPets();
  },
};
</script>

<style>
</style>