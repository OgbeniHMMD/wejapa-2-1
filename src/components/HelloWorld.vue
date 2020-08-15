<template>
  <div class="d-flex justify-content-center align-items-center text-center min-vh-100 m-0 my-auto">
    <div>
      <header>
        <img
          src="https://cdn.hashnode.com/res/hashnode/image/upload/v1594059995543/vwTYbmSQ9.png"
          width="300"
        />
      </header>
      {{states[name='Aba'] }}
      <div class="d-flex align-items-center mb-3">
        <div class="mr-3">country:</div>
        <select
          v-model="selectedCountry"
          @change="fetchStates()"
          class="form-control form-control-lg"
          placeholder="Select Your Country"
        >
          <option value disabled selected>Select your country</option>
          <option v-for="item in countries" v-bind:value="item" v-bind:key="item">{{ item }}</option>
        </select>
      </div>

      <div class="d-flex align-items-center mb-3">
        <div class="mr-3">State:</div>
        <select
          v-model="selectedState"
          class="form-control form-control-lg"
          placeholder="Select Your countries"
          @change="fetchCities()"
        >
          <option
            v-for="item in states"
            v-bind:value="item.name"
            v-bind:key="item.id"
          >{{ item.name }}</option>
        </select>
      </div>

      <div class="d-flex align-items-center">
        <div class="mr-3">City:</div>
        <select
          v-model="selectedCity"
          class="form-control form-control-lg"
          placeholder="Select Your City/Region"
        >
          <option
            v-for="item in cities"
            v-bind:value="item.name"
            v-bind:key="item.id"
          >{{ item.name }}</option>
        </select>
      </div>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  data() {
    return {
      JSON,

      selectedCity: "",
      selectedState: "",
      selectedCountry: "",

      cities: {},
      states: {},

      countries: {},
    };
  },

  // Fetches posts when the component is created.
  async created() {
    axios
      .get(`http://allcountries.us-east-2.elasticbeanstalk.com/countries/list`)
      .then((response) => {
        // JSON responses are automatically parsed.
        this.countries = response.data;
      })
      .catch((e) => {
        console.log(e);
      });
  },

  methods: {
    fetchStates() {
      this.states = {};

      axios
        .get(
          `http://allcountries.us-east-2.elasticbeanstalk.com/countries/details/${this.selectedCountry}`
        )
        .then((response) => {
          // JSON responses are automatically parsed.
          this.states = response.data.states;
        })
        .catch((e) => {
          console.log(e);
        });
    },

    fetchCities() {
      try {
        this.states = this.states[this.selectedState];
      } catch (error) {
        //
      }
    },
  },
};
</script>  