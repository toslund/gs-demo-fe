<template>
  <v-container>
    <v-row class=" justify-center">
      <v-col class="content mb-4 flex-grow-0">
        <h3 class="display-2 ">
          Welcome
        </h3>

        <p class="font-weight-regular">
          Because I requested the Python technical assessment, I created this demo to use PHP and MySQL.
        </p>
        <p class="font-weight-regular">
          The database is an Amazon RDS MySQL instance. The backend is a simple read only API built with Laravel. The front end
          is build with Vue.js.
        </p>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

  export default {
    name: 'About',

    data: () => ({
      zipEndpoint: 'https://infinite-harbor-68640.herokuapp.com/api/zipcodes',
      ecosystem: [
        {
          text: 'vuetify-loader',
          href: 'https://github.com/vuetifyjs/vuetify-loader',
        },
        {
          text: 'github',
          href: 'https://github.com/vuetifyjs/vuetify',
        },
        {
          text: 'awesome-vuetify',
          href: 'https://github.com/vuetifyjs/awesome-vuetify',
        },
      ],
    }),
    methods: {
      async getZips() {
        console.log('getting zips');
        const url = `${this.zipEndpoint}`
        console.log(url);
        // const url = new URL(this.deckEndpoint);
        const response = await axios.get(url);
        console.log(`length of zips rom fetched deck: ${response.data.length}`);
        console.log(response);
        return response;
      },
    },
    mounted() {
      this.getZips();
    }
  }
</script>

<style scoped>

.content {
  min-width: 70%;
}
</style>
