<template>
  <v-app>


    <v-main>
      <Fitu></Fitu>
    </v-main>
  </v-app>
</template>

<script>
import Fitu from "./components/Fitu.vue"
export default {
  name: "App",

  components: {
    Fitu
  },

  data: () => ({
    //
  })
};
</script>
