
<template>
    <div>
        
    <input type="text" v-model="searchQuery" placeholder="Search by Country Name" />
    
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
          <tr v-for="country in filteredCountries" :key="country.cca2">          

            <td><img :src="country.flags.png" alt="Flag" /></td>
            <td>
              <span v-if="country.name && country.name.official">{{ country.name.official }}</span>
            </td>
            <td>{{ country.cca2 }}</td>
            <td>{{ country.cca3 }}</td>
            <td>
              <span v-if="country.name && country.name.nativeName && Object.keys(country.name.nativeName).length > 0">{{ country.name.nativeName[Object.keys(country.name.nativeName)[0]].official }}</span>
            </td>
            <td>
              <span v-if="country.altSpellings && country.altSpellings.length > 0">{{ country.altSpellings.join(', ') }}</span>
            </td>
            <td>
              <span v-if="country.idd && country.idd.root">{{ country.idd.root }}{{ country.idd.suffixes.join(', ') }}</span>
            </td>

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
            searchQuery: "",
        };
    },
    computed: {
        displayedCountries() {
            const start = (this.currentPage - 1) * this.itemsPerPage;
            const end = start + this.itemsPerPage;
            return this.countries.slice(start, end);
        },
        filteredCountries() {
            const query = this.searchQuery.toLowerCase().trim();
            if (query === "") {
                return this.displayedCountries;
            } else {
                return this.displayedCountries.filter((country) => {
                const countryName = country.name.official.toLowerCase();
                return countryName.includes(query);
                });
            }
        },

    },
    mounted() {
        this.fetchData();
    },
    methods: {
        async fetchData() {
            try {
                const response = await axios.get('https://restcountries.com/v3.1/all');

                console.log(response.data);
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
  