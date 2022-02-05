  <template>
    <div class="wayfinder">
      <h1>Search Star Wars Characters</h1>
      <v-form
      ref="form"
      v-model="valid"
      lazy-validation
    >
      <v-text-field
        v-model="name"
        label="Name"
        required
      ></v-text-field>

      <v-text-field
        v-model="homeworld"
        label="Homeworld"
        required
      ></v-text-field>

      <v-text-field
        v-model="films"
        label="Films"
        required
      ></v-text-field>
      <h1>Results</h1>
      <p>Name: {{this.name}}</p>
      <p>Birth Year: {{this.birthyear}}</p>
      <p>Homeworld: {{this.homeworld}}</p>
      <p>Films: {{this.films}}</p>
      <p>Species: {{this.species}}</p>
      <p>Date Last Edited: {{this.dateLastEdited}}</p>
      <v-btn
        :disabled="!valid"
        color="success"
        class="mr-4"
        @click="search"
      >
        Search
      </v-btn>

      <v-btn
        color="error"
        class="mr-4"
        @click="reset"
      >
        Reset Form
      </v-btn>
    </v-form>
    </div>
  </template>

  <script>
    const apiURL = "https://swapi.dev/api/";
  export default {
    name: 'WayFinder',
    props: {
      msg: String
    },
    data: () => ({
      valid: true,
      name: '',
      homeworld: '',
      films: '',
      birthyear: '',
      species: '',
      dateLastEdited: ''
    }),

    methods: {
      async setAllElements() {
        console.log('we have called set all elements');
        const response = await fetch(`${apiURL}people/`);
        //console.log(response.json());
        if (response.status >= 200 && response.status <= 299) {
          const data = await response.json();
          console.log(data);
        }
      },
      search() {
        console.log('hello, welcome to the search method!!!');
        this.setAllElements();
        //const response = fetch(`${apiURL}people/`);
        //console.log(response);
        

      },
      reset () {
        this.$refs.form.reset()
      },
      resetValidation () {
        this.$refs.form.resetValidation()
      },
      },
  }
  </script>

  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  </style>
