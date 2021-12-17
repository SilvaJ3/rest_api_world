<template>
  <div class="home w-full flex flex-col justify-start items-center py-20 px-32 h-screen">
    <div class="w-full flex justify-start items-center">
      <router-link to="/">
        <div class="flex justify-center items-center cursor-pointer rounded-md shadow-md w-32 py-2">
          <i class="fas fa-long-arrow-alt-left pointer-events-none"></i>
          <span class="pointer-events-none ml-4">Back</span>
        </div>
      </router-link>
    </div>
    <div class="flex justify-center w-full mt-12">
      <div class="w-1/2">
        <img :src="country.flag" alt="">
      </div>
      <div class="w-1/2 flex flex-col justify-evenly items-start px-10 text-left">
        <h1 class="font-bold text-xl">
          {{ country.name }}
        </h1>
        <div class="flex justify-between items-start w-full">
          <div class="flex flex-col justify-center items-start w-full">
            <p class="font-bold">Native Name: <span class="font-normal">{{ country.nativeName }}</span></p>
            <p class="font-bold">Top Level Domain: <span class="font-normal">{{ country.topLevelDomain[0] }}</span></p>
            <p class="font-bold">Region: <span class="font-normal">{{ country.region }}</span></p>
            <p class="font-bold">Languages: <span class="font-normal">{{ country.languages[0].name}}</span></p>
            <p class="font-bold">Sub Region: <span class="font-normal">{{ country.subregion }}</span></p>
          </div>
          <div class="flex flex-col justify-center items-start w-full">
            <p class="font-bold">Population: <span class="font-normal">{{ country.population }}</span></p>
            <p class="font-bold">Currencies: <span class="font-normal">{{ country.currencies[0].name }}</span></p>
            <p class="font-bold">Capital: <span class="font-normal">{{ country.capital }}</span></p>
          </div>
        </div>
        <div class="flex items-center">
          <p class="font-bold">Border Countries</p>
          <router-link :to="`/` + border.name" v-for="border in borderCountry" :key="border">
            <div class="rounded-sm shadow-md flex justify-center items-center mx-3 py-1 px-2 cursor-pointer">
              <span class="pointer-events-none">{{ border.name }}</span>
            </div>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios"

export default {
  name: "Country",
  data(){
    return {
      country: null,
      countries: null,
      borderCountry: []
    }
  },
  methods: {
    filter: function () {
      this.borderCountry = [];
      for (let i = 0; i < this.country.borders.length; i++) {
        this.countries.forEach(element => {
          if(element.cioc == this.country.borders[i]) {
            this.borderCountry.push(element);
          }
        });
      }
      return this.borderCountry;
    }
  },
  computed:{
    countrySelected: function() {
      return `https://restcountries.com/v2/name/${this.$route.params.name}?fullText=true`
    },
    // borderCountry: function(){
    //   this.filter();
    //   return this.filteredArray;
    // },
  },
  created(){
    axios.get(this.countrySelected)
      .then(response => {
        this.country = response.data[0];
      });
    axios.get("https://restcountries.com/v2/all")
      .then(response => {
        this.countries = response.data;
        this.filter();
      })
  }
}
</script>

<style>

</style>