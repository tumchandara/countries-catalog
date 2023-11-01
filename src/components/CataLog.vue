
<template>

  



    <div>
      
      
    <CountryModal
      v-if="showModal"
      :countryData="selectedCountry"
      @close="showModal = false"
    />
        
    <input type="text" v-model="searchQuery" placeholder="Search by Country Name" />
    
      <table class="bordered-table">
        <thead>
          <tr>
            <th>Flags</th>
            <th @click="sortCountries('name.official')">Country Name</th>

            <th>2 character Country Code</th>
            <th>3 character Country Code</th>
            <th>Native Country Name</th>
            <th>Alternative Country Name</th>
            <th>Country Calling Codes</th>

          </tr>
        </thead>
        <tbody>
          <tr v-for="country in filteredAndSortedCountries" :key="country.cca2">          

            <td><img :src="country.flags.png" alt="Flag" /></td>
            <td @click="openModal(country)">
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
    import CountryModal from './modals/CountryModal.vue'; // Adjust the path to match the actual location


    export default {
    components: {
      CountryModal,
    },
      
    data() {
        return {
            countries: [], // Your data
            currentPage: 1,
            itemsPerPage: 25,
            searchQuery: "",
            sortField: "name.official", // Default sort field
            sortDirection: 1, // 1 for ascending, -1 for descending
            showModal: false,
            selectedCountry: null,
        };
    },
    computed: {
        displayedCountries() {
            const start = (this.currentPage - 1) * this.itemsPerPage;
            const end = start + this.itemsPerPage;
            return this.countries.slice(start, end);
        },
        sortedCountries() {
          return this.displayedCountries.slice().sort((a, b) => {
            const fieldA = (a[this.sortField] || '').toLowerCase();
            const fieldB = (b[this.sortField] || '').toLowerCase();
            return (fieldA < fieldB ? -1 : 1) * this.sortDirection;
          });
        },

        filteredAndSortedCountries() {
          return this.displayedCountries.slice().filter((country) => {
            const query = this.searchQuery.toLowerCase().trim();
            if (query === '') {
              return true;
            }
            const countryName = country.name.official.toLowerCase();
            return countryName.includes(query);
          }).sort((a, b) => {
            const fieldA = (a[this.sortField] || '').toLowerCase();
            const fieldB = (b[this.sortField] || '').toLowerCase();
            return (fieldA < fieldB ? -1 : 1) * this.sortDirection;
          });
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

        sortCountries(field) {
          if (field === this.sortField) {
            this.sortDirection = -this.sortDirection;
          } else {
            this.sortField = field;
            this.sortDirection = 1;
          }

            console.log(this.sortDirection);
        },
        
        openModal(country) {
          this.selectedCountry = country;
          this.showModal = true; // Set showModal to true to open the modal

          console.log('Modal should be open:', this.showModal);

        }
        
    },
    };
</script>

  
  <style scoped>
  /* Add component-specific styles here */
  </style>
  