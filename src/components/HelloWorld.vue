<template>
  <main class="d-flex justify-content-center align-items-center text-center min-vh-100 m-0 my-auto">
    <div class="col-12 col-sm-10 col-md-8">
      <div v-if="success">
        <a href="/">
          <img
            alt="sucessfully submitted"
            src="https://cdn3.iconfinder.com/data/icons/flat-actions-icons-9/792/Tick_Mark_Dark-512.png"
          />
        </a>
      </div>

      <form v-else @submit.prevent="submitForm" @reset="resetForm">
        <header>
          <img
            src="https://cdn.hashnode.com/res/hashnode/image/upload/v1594059995543/vwTYbmSQ9.png"
            width="250"
          />
        </header>

        <section>
          <div class="row my-3">
            <label class="col-3 lead my-auto">Country:</label>

            <select
              required
              @change="fetchStates()"
              v-model="FORM.country"
              searchable="Search here.."
              class="col custom-select custom-selects-lg"
            >
              <option disabled selected>Select country</option>
              <option v-for="item in countries" v-bind:value="item" v-bind:key="item">{{ item }}</option>
            </select>
          </div>

          <div class="row align-center">
            <label class="col-3 lead my-auto">State:</label>

            <select
              required
              :disabled="!FORM.country"
              v-model="FORM.state"
              class="col custom-select custom-selects-lg"
            >
              <option value disabled selected>Select state</option>
              <option
                v-for="item in states"
                v-bind:value="item.name"
                v-bind:key="item.id"
              >{{ item.name }}</option>
            </select>
          </div>

          <div class="row mt-3">
            <label class="col-3 lead my-auto">Capital:</label>

            <select required :disabled="!FORM.state" class="col custom-select custom-selects-lg">
              <option value disabled selected>Select capital</option>

              <template v-for="state in states">
                <template v-if=" state.name == FORM.state">
                  <option
                    v-for="city in state.cities"
                    v-bind:value="city.name"
                    v-bind:key="state.id + '-' + city.id"
                  >{{ city.name }}</option>
                </template>
              </template>
            </select>
          </div>
        </section>

        <footer class="row">
          <div class="col-10 col-sm-6 mt-4">
            <button class="btn btn-outline-primary w-100" type="reset">Reset</button>
          </div>
          <div class="col-10 col-sm-6 mt-4">
            <button class="btn btn-primary w-100" type="submit">Submit form</button>
          </div>
        </footer>
      </form>
    </div>
  </main>
</template>


<script>
import axios from "axios";

export default {
  data() {
    return {
      success: false,

      states: {},
      countries: {},

      FORM: { country: "", state: "" },
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
          `http://allcountries.us-east-2.elasticbeanstalk.com/countries/details/${this.FORM.country}`
        )
        .then((response) => {
          // JSON responses are automatically parsed.
          this.states = response.data.states;
        })
        .catch((e) => {
          console.log(e);
        });
    },

    submitForm() {
      // Show the success
      this.success = true;

      console.log("Form submit successfully");
    },

    resetForm() {
      this.FORM.state = "";
      this.FORM.country = "";
    },
  },
};
</script>  