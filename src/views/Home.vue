<template>
  <div class="home w-full flex flex-col justify-start items-center py-20 px-32 h-screen">
    <div class="search-filter flex justify-between items-center w-full">
      <div>
        <input type="text" v-model="inputSearch" @keyup.enter="searchCountry" placeholder="Search for a country..." class="px-3 shadow-md rounded-md h-16 w-96">
      </div>
      <div class="filter_list">
        <select name="continent" id="" v-model="continentSelect" @change="searchContinent" class="px-3 shadow-md rounded-md h-12 w-44">
          <option value="africa">Africa</option>
          <option value="americas">America</option>
          <option value="asia">Asia</option>
          <option value="europe">Europe</option>
          <option value="oceania">Oceania</option>
        </select>
      </div>
    </div>

    <div class="countries_list grid grid-cols-4 gap-6 py-10">
      <router-link :to="'/' + country.name" v-for="(country, index) in filterList" :key="index">
        <div class="flex flex-col rounded-lg shadow-xl bg-white overflow-hidden">
          <div class="h-1/2 country_flag w-full">
            <img :src="country.flag" alt="" class="object-cover w-full">
          </div>
          <div class="flex flex-col items-start justify-center h-1/2 p-4 font-bold">
            <h1 class="text-xl">{{ country.name }}</h1>
            <p>Population: <span class="font-normal">{{country.population}}</span></p>
            <p>Region: <span class="font-normal">{{country.region}}</span></p>
            <p>Capital: <span class="font-normal">{{country.capital}}</span></p>
          </div>
        </div>
      </router-link>
    </div>
  </div>

</template>

<script>
import axios from "axios";

export default {
  name: 'Home',
  data(){
    return {
      inputSearch: "",
      continentSelect: "",
      countriesList: null,
      countriesByContinent: null,
      filterCountry: null,
    }
  },
  methods: {
    fetchCountries(){
      axios.get("https://restcountries.com/v2/all")
      .then(response => {
        this.countriesList = response.data;
      })
    },
    searchCountry(){
      if(this.inputSearch) {
        this.countriesByContinent = [];
        axios.get(`https://restcountries.com/v2/all`)
          .then(response => {
            if(response.data[0]) {
              this.filterCountry = response.data.filter(item => item.name.toLowerCase().startsWith(this.inputSearch.toLowerCase()));
            }
          })
      }
    },
    searchContinent(){
      this.countriesByContinent = [];
      if (this.continentSelect) {
        axios.get(`https://restcountries.com/v2/region/${this.continentSelect}`)
          .then(response => {
            this.countriesByContinent = response.data
          })
      } else {
        false
      }
    }
  },
  computed: {
    filterList: function () {
      if (this.continentSelect) {
        return this.countriesByContinent;
      } else if (this.filterCountry){
        return this.filterCountry;
      } else {
          return this.countriesList;
      }
    }
  },
  mounted(){
    this.fetchCountries();
  },
}
</script>

<style scoped>

.country_flag img{
  height: 160px;
}
</style>