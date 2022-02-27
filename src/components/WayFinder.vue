  <template>
    <div class="wayfinder">
      <h1>Search Star Wars Characters (This is the way!)</h1>
      <v-form
      ref="form"
      v-model="valid"
      lazy-validation
    >
      <v-text-field
        v-model="name"
        label="Name"
        :rules="nameRules"
        required
      ></v-text-field>
      
      <v-select
          v-model="select"
          :items="items"
          item-value=items[0]
          label="Homeworld"
          return-object
          data-vv-name="select"
          required
        ></v-select>
        <v-container fluid>
      <h1>Select film he/she was in (leave empty for no filter)</h1>
    <v-checkbox
      v-model="checkboxSelected"
      label="1. A New Hope"
      value="1. A New Hope"
    ></v-checkbox>
    <v-checkbox
      v-model="checkboxSelected"
      label="2. The Empire Strikes Back"
      value="2. The Empire Strikes Back"
    ></v-checkbox>
    <v-checkbox
      v-model="checkboxSelected"
      label="3. Return of the Jedi"
      value="3. Return of the Jedi"
    ></v-checkbox>
    <v-checkbox
      v-model="checkboxSelected"
      label="4. The Phantom Menace"
      value="4. The Phantom Menace"
    ></v-checkbox>
    <v-checkbox
      v-model="checkboxSelected"
      label="5. Clone Wars"
      value="5. Clone Wars"
    ></v-checkbox>
    <v-checkbox
      v-model="checkboxSelected"
      label="6. Revenge of the Sith"
      value="6. Revenge of the Sith"
    ></v-checkbox>
      </v-container>
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
      nameRules: [
        v => !!v || 'Name is required',
      ],
      select: 0,
      items: [
        '0. Any (no homeworld filter)',
        '1. Tatooine',
        '2. Alderaan',
        '3. Yavin IV',
        '4. Hoth',
        '5. Dagobah',
        '6. Bespin',
        '7. Endor',
        '8, Naboo',
        '9. Coruscant',
        '10. Kamino',
        '11. Geonosis',
        '12. Utapau',
        '13. Mustafar',
        '14. Kashyyyk',
        '15. Polis Massa',
        '16. Mygeeto',
        '17. Felucia',
        '18. Cato Neimoidia',
        '19. Saleucami',
        '20. Stewjon',
        '21. Eriadu',
        '22. Corellia',
        '23. Rodia',
        '24. Nal Hutta',
        '25. Dantooine',
        '26. Bestine IV',
        '27. Ord Mantell',
        '28. (Unknown)',
        '29. Trandosha',
        '30. Socorro',
        '31. Mon Cala',
        '32. Chandrila',
        '33. Sullust', 
        '34. Toydaria',
        '35. Malastare',
        '36. Dathomir',
        '37. Ryloth',
        '38. Aleen Minor',
        '39. Vulpter',
        '40. Troiken',
        '41. Tund',
        '42. Haruun Kal',
        '43. Cerea',
        '44. Glee Anselm',
        '45. Iridonia',
        '46. Tholoth',
        '47. Iktotch',
        '48. Quermia',
        '49. Dorin',
        '50. Champala',
        '51. Mirial',
        '52. Serenno',
        '53. Concord Dawn',
        '54. Zolan',
        '55. Ojom',
        '56. Skako',
        '57. Muunilinst',
        '58. Shili',
        '59. Kalee',
        '60. Umbara'
      ],
      homeworld: '',
      homeworldRules: [
        v => !!v || 'Homeworld is required',
      ],
      checkboxSelected: null,
      films: '',
      filmRules: [
        v => !!v || 'Films list is required',
      ],
      birthyear: '',
      species: '',
      dateLastEdited: '',
      
    }),
    methods: {
      async setAllElements() {
        const response = await fetch(`${apiURL}/people/?search=${this.name}`);
        if (response.status >= 200 && response.status <= 299) {
          const data = await response.json();
          const results = data.results;
          let filteredList = results;
          filteredList = filteredList.filter(result => result.name.match(this.name));
          let index = this.select.substr(0, this.select.indexOf('.'));
          if(index != 0) {
            filteredList = filteredList.filter(result => result.homeworld.match(`${apiURL}planets/${index}`));
          }
          if(this.checkboxSelected !== null) {
            let checkboxIndex = this.checkboxSelected.substr(0, this.checkboxSelected.indexOf('.'));
            filteredList = filteredList.filter(result => result.films.includes(`${apiURL}films/${checkboxIndex}/`));
          }
          

          if(filteredList.length === 1)
          {
            console.log('RESULT FOUND');
            this.name = filteredList[0].name;
            this.birthyear = filteredList[0].birth_year;
            this.homeworld = filteredList[0].homeworld;
            this.films = filteredList[0].films.join(';');
            this.species = filteredList[0].species.join('');
            this.dateLastEdited = filteredList[0].edited;

          } else {
            console.log('SEARCH FAILED');
            
            this.name = 'NO MATCHES FOUND. Try again.';
          }
        }
        
        
        
      },
      search() {
        if (this.$refs.form.validate()) {
          this.setAllElements();
        }
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
