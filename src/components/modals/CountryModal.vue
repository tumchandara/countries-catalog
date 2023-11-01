<template>
    <div class="country-modal modal-container" >
      <div class="modal-dialog "> <!-- Center the modal vertically -->
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title">Other Information</h5>
            <button type="button" class="btn-close" @click="closeModal" aria-label="Close"></button>
          </div>
          <div class="modal-body">
            <!-- Check if the countryData is defined before accessing its properties -->
            <div v-if="countryData">
              <h2>Country Name: {{ countryData.name.official }}</h2> <!-- Access the official name of the country -->

              <div class="row">
                <div class="col-md-5">

                    <table class="bordered-table">

                    <tbody>
                    <tr>
                        <td>TLD</td>
                        <td>
                            <span v-if="countryData.tld && countryData.tld.length > 0">{{ countryData.tld.join(', ') }}</span>
                        </td>
                    </tr>

                    <tr>
                        <td>CCN3</td>
                        <td>
                        <span v-if="countryData.ccn3 && countryData.ccn3">{{ countryData.ccn3 }}</span>
                    </td>
                    </tr>

                    <tr>
                        <td>
                            Independent
                        </td>
                        <td>
                            <span v-if="countryData.independent && countryData.independent">{{ countryData.independent }}</span>
                        </td>
                    </tr>

                    <tr>
                        <td>Status</td>
                        <td>
                        <span v-if="countryData.status && countryData.status">{{ countryData.status }}</span>
                        </td>

                    </tr>

                    <tr>
                        <td>Currency</td>
                        <td>
                            <div v-if="countryData.currencies">
                            
                                <ul>
                                    <li v-for="(currency, code) in countryData.currencies" :key="code">
                                        {{ currency.name }} ({{ currency.symbol }})
                                    </li>

                                </ul>
                            </div>
                        </td>

                    </tr>

                    <tr>
                        <td>Capital</td>
                        
                        <td>
                            <span v-if="countryData.capital && countryData.capital.length > 0">{{ countryData.capital.join(', ') }}</span>
                        </td>


                    </tr>

                    <tr>
                        <td>Region</td>
                        
                        <td>
                            <span v-if="countryData.region && countryData.region">{{ countryData.region }}</span>
                        </td>

                    </tr>

                    <tr>
                        <td>Subregion</td>
                        
                        
                        <td>
                            <span v-if="countryData.subregion && countryData.subregion">{{ countryData.subregion }}</span>
                        </td>
                    </tr>

                    <tr>
                        <td>Languages</td>
                        
                    
                        <td>
                            <div v-if="countryData.languages">
                            
                            <ul>
                                <li v-for="(language, code) in countryData.languages" :key="code">
                                    {{ language }}
                                </li>

                            </ul>
                        </div>
                        </td>

                    </tr>

                    
                        

                    </tbody>
                    </table>

                    </div>

                    <div class="col-md-7">

                    <table class="bordered-table">

                    <tbody>
                    
                        <tr>
                            <td>Latlng</td>
                            <td>
                                <span v-if="countryData.latlng && countryData.latlng.length > 0">{{ countryData.latlng.join(', ') }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>Landlocked</td>
                        
                            <td>
                                <span v-if="countryData.landlocked && countryData.landlocked">{{ countryData.landlocked }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>Area</td>
                        
                            <td>
                                <span v-if="countryData.area && countryData.area">{{ countryData.area }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>Area</td>
                        
                            <td>
                                <span v-if="countryData.area && countryData.area">{{ countryData.area }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>Flag</td>
                        
                            <td>
                                <span v-if="countryData.flag && countryData.flag">{{ countryData.flag }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>Maps</td>
                        
                            <td>
                            <div v-if="countryData.maps">
                                
                                <ul>
                                    <li v-for="(map, code) in countryData.maps" :key="code">
                                        {{ map }}
                                    </li>

                                </ul>
                            </div>
                            </td>

                        </tr>

                        <tr>
                            <td>Population</td>
                            <td>
                                <span v-if="countryData.population && countryData.population">{{ countryData.population }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>Fifa</td>
                            <td>
                                <span v-if="countryData.fifa && countryData.fifa">{{ countryData.fifa }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>Car</td>
                            
                            <td>
                                <div v-if="countryData.car">
                                    
                                    <ul>
                                        <li v-for="(car, code) in countryData.car" :key="code">
                                            {{ car[0] }}
                                            
                                        </li>

                                    </ul>
                                </div>
                            </td>

                        </tr>

                        
                        <tr>
                            <td>Timezones</td>
                            <td>
                                <span v-if="countryData.timezones && countryData.timezones.length > 0">{{ countryData.timezones.join(', ') }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>Continents</td>
                            <td>
                                <span v-if="countryData.continents && countryData.continents.length > 0">{{ countryData.continents.join(', ') }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>StartOfWeek</td>
                            <td>
                                <span v-if="countryData.startOfWeek && countryData.startOfWeek">{{ countryData.startOfWeek }}</span>

                            </td>

                        </tr>

                        <tr>
                            <td>CapitalInfo</td>
                            <td>
                                <div v-if="countryData.capitalInfo">
                                    
                                    <ul>
                                        <li v-for="(info, code) in countryData.capitalInfo" :key="code">
                                            {{ info }}
                                            
                                        </li>

                                    </ul>
                                </div>
                            </td>

                        </tr>

                        <tr>
                            <td>PostalCode</td>
                            <td>
                                <div v-if="countryData.postalCode">
                                    
                                    <ul>
                                        <li v-for="(postal, code) in countryData.postalCode" :key="code">
                                            {{ postal }}
                                            
                                        </li>

                                    </ul>
                                </div>
                            </td>

                        </tr>
                    

                    </tbody>
                    </table>
                    </div>
              </div>

            



              <!-- Display other country information here -->
            </div>
            <div v-else>
              <p>No country information available.</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      countryData: Object, // Receive the country data as a prop
    },
    methods: {
      closeModal() {
        this.$emit('close'); // Emit a custom event to close the modal
      },
    },
  };
  </script>
  
  <style>
    .country-modal {
        background-color: lightblue;
    }

    .modal-container {
        position: fixed; /* Fix the position of the container */
        top: 0; /* Position from the top of the viewport */
        left: 0; /* Position from the left of the viewport */
        width: 100%; /* Cover the entire width */
        height: 100%; /* Cover the entire height */
        background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent background overlay */
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 1000; /* Set a high z-index value to ensure it's on top of other content */
    }

    .modal {
        background-color: white;
        padding: 20px;
        border: 1px solid #ccc;
        z-index: 1001; /* Set a higher z-index value than the modal container */
    }

    .modal-content{
        background-color: #fff !important;
    }

    .table-responsive {
        overflow-x: auto;
    }

    .table {
        width: 100%;
        max-width: 100%;
    }

    .scrollable-content {
        max-height: 300px; /* Set a maximum height or adjust as needed */
        overflow-y: auto;
    }

  </style>
  