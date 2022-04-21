<template>
  <v-app>

    <v-main>
      <v-tabs grow v-model="tab">
        <v-tab key="zipcode">Zipcode Lookup</v-tab>
        <!-- <v-tab key="statebrowser">Browse by State</v-tab> -->
        <v-tab key="about">About</v-tab>
      </v-tabs>
      <v-tabs-items v-model="tab">
      <v-tab-item
        key="zipcode"
      >
        <ZipLookup/>
      </v-tab-item>
      <!-- <v-tab-item
        key="statebrowser"
      >
        <StateBrowser/>
      </v-tab-item> -->
      <v-tab-item
        key="about"
      >
        <About/>
      </v-tab-item>
    </v-tabs-items>
    </v-main>
  </v-app>
</template>

<script>
import ZipLookup from './components/ZipLookup';
// import StateBrowser from './components/StateBrowser';
import About from './components/About';

export default {
  name: 'App',

  components: {
    ZipLookup,About
  },

  data: () => ({
    tab: null,
    //
  }),
};
</script>
