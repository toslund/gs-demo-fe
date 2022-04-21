<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <v-card
          color="green lighten-1"
          dark
        >
          <v-card-title class="text-h5 green lighten-2 text-center">
            Search for Zipcode
          </v-card-title>
          <v-card-text>
            Explore basic zipcode statistics
          </v-card-text>
          <v-card-text>
            <v-autocomplete
              v-model="selectedZip"
              :items="items"
              :loading="isLoading"
              :search-input.sync="search"
              color="white"
              hide-no-data
              hide-selected
              item-text="Description"
              item-value="API"
              label="Zipcode"
              placeholder="Start typing to Search"
              prepend-icon="mdi-database-search"
              return-object
            ></v-autocomplete>
          </v-card-text>
          <v-divider></v-divider>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              :disabled="!selectedZip"
              color="red lighten-2"
              @click="selectedZip = null"
            >
              Clear
              <v-icon right>
                mdi-close-circle
              </v-icon>
            </v-btn>
            <v-btn
              color="black darken-3"
              @click="getZip(selectedZip.zip_code)"
            >
              Search
              <v-icon right>
                mdi-close-circle
              </v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
        <v-card v-if="zipcodeData" class="mt-5">
          <v-card-title class="justify-center">
           {{zipcodeData.zip_code}}
         </v-card-title>
         <v-simple-table>
          <template v-slot:default>
            <tbody>
              <tr>
                <td>Population</td>
                <td>{{ zipcodeData.pop ?  zipcodeData.pop : 'No Data' }}</td>
              </tr>
              <tr>
                <td>Median Family Income</td>
                <td>{{ zipcodeData.med_fam_incm ?  `$${zipcodeData.med_fam_incm.toLocaleString("en-US")}` : 'No Data' }}</td>
              </tr>
              <tr>
                <td>Median Household Income</td>
                <td>{{ zipcodeData.med_house_incm ?  `$${zipcodeData.med_house_incm.toLocaleString("en-US")}` : 'No Data' }}</td>
              </tr>
              <tr>
                <td>Mean Population Center</td>
                <td>{{ zipcodeData.pop_x ?  `${zipcodeData.pop_y}, ${zipcodeData.pop_x}` : 'No Data' }}</td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
        </v-card>
      </v-col>

    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

  export default {
    name: 'ZipLookup',

    data: () => ({
      zipEndpoint: 'https://infinite-harbor-68640.herokuapp.com/api/zipcodes',
      descriptionLimit: 60,
      zipcodes: [],
      zipcodeData: null,
      isLoading: false,
      selectedZip: null,
    }),
    computed: {
      items () {
        return this.zipcodes.map(entry => {
          const Description = entry.zip_code.length > this.descriptionLimit
            ? entry.zip_code.slice(0, this.descriptionLimit) + '...'
            : entry.zip_code

          return Object.assign({}, entry, { Description })
        })
      },
    },
    watch: {
      selectedZip (val) {
        console.log(val);

      },
      // search (val) {
      //   // Items have already been loaded
      //   if (this.items.length > 0) return

      //   // Items have already been requested
      //   if (this.isLoading) return

      //   this.isLoading = true

      //   // Lazily load input items
      //   fetch(this.zipEndpoint)
      //     .then(res => res.json())
      //     .then(res => {
      //       const { count, entries } = res
      //       this.count = count
      //       this.entries = entries
      //     })
      //     .catch(err => {
      //       console.log(err)
      //     })
      //     .finally(() => (this.isLoading = false))
      // },
    },
    methods: {
      async getZips() {
        console.log('getting zips');
        const url = `${this.zipEndpoint}`
        console.log(url);
        // const url = new URL(this.deckEndpoint);
        const response = await axios.get(url);
        console.log(`length of zips from fetched deck: ${response.data.length}`);
        this.zipcodes = response.data
        console.log(response);
        return response;
      },
      async getZip(zipcode) {
        console.log('getting zips');
        const url = `${this.zipEndpoint}?zipname=${zipcode}`
        console.log(url);
        // const url = new URL(this.deckEndpoint);
        const response = await axios.get(url);
        this.zipcodeData = response.data
        console.log(response);
        return response;
      },
    },
    mounted() {
      this.getZips();
    }
  }
</script>
