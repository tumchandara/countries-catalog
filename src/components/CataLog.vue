
<template>
    <div>
      <table class="bordered-table">
        <thead>
          <tr>
            <th>Flags</th>
            <th>Country Name</th>
            <th>2 character Country Code</th>
            <th>3 character Country Code</th>
            <th>Native Country Name</th>
            <th>Alternative Country Name</th>
            <th>Country Calling Codes</th>

          </tr>
        </thead>
        <tbody>
          <tr v-for="country in displayedCountries" :key="country.cca2">          

            <td><img :src="country.flags.png" alt="Flag" /></td>
            <td>{{ country.name.official }}</td>
            <td>{{ country.cca2 }}</td>
            <td>{{ country.cca3 }}</td>
            <td>{{ country.name.nativeName[Object.keys(country.name.nativeName)[0]].official }}</td>
            <td>{{ country.altSpellings.join(', ') }}</td>
            <td>{{ country.idd.root }}{{ country.idd.suffixes.join(', ') }}</td>


          </tr>
        </tbody>
      </table>
      
      <div>
        <button @click="prevPage">Previous</button>
        <span>{{ currentPage }}</span>
        <button @click="nextPage">Next</button>
      </div>
    </div>
  </template>
  
  <style>
  .bordered-table {
    border-collapse: collapse;
    width: 100%;
  }
  
  .bordered-table th, .bordered-table td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
  }
  
  .bordered-table th {
    background-color: #f2f2f2;
  }
  </style>
  
  
  
  
  <script>
    import axios from 'axios';

    export default {
    data() {
        return {
            countries: [], // Your data
            currentPage: 1,
            itemsPerPage: 25,
        };
    },
    computed: {
        displayedCountries() {
            const start = (this.currentPage - 1) * this.itemsPerPage;
            const end = start + this.itemsPerPage;
            return this.countries.slice(start, end);
        },
    },
    mounted() {
        this.fetchData();
    },
    methods: {
        async fetchData() {
            try {
                const response = await axios.get('https://restcountries.com/v3.1/all');
                this.countries = response.data;
            } catch (error) {
                console.error('Error fetching data:', error);
            }

        },

        nextPage() {
            if (this.countries.length === 0) {
                return;
            }

            this.currentPage++;
            this.fetchData(this.currentPage);
        },

        prevPage() {
        if (this.currentPage > 1) {
            this.currentPage--;
        }
        },

    },
    };
</script>

  
  <style scoped>
  /* Add component-specific styles here */
  </style>
  