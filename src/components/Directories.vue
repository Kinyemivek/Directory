<template>
    <div class="container row">
        
        <!-- my Header -->
        <div class="navbar">
            <a href="index.html">
            </a>
        </div>
        <br><br>
        <h1 class="title">{{ title }}</h1>
        <br>
        <hr>
        <br>

        <div class="column country-settings">
            <form>
                <!-- For theCountries -->
                <div class="form-settings">
                    <label for="countries" class="label">Select Your Country</label>
                    <select class="select" name="countries" id="countries" v-model="selectedCountry" @change="onCountrySelect($event)">
                        <option :value="country.name" v-for="country in countries" :key="country.id" class="count-option">{{ country.name }}</option>
                    </select>
                </div>
                <br><br>

                <!-- For the States -->
                <div class="form-settings">
                    <label for="state" class="label">Select Your State</label>
                    <select class="select" name="states" id="states" v-model="selectedState" @change="onStateSelect($event)">
                        <option :value="state.name" v-for="state in states" :key="state.id" class="count-option">{{ state.name }}</option>
                    </select>
                    <div v-if="stateError" :class="{empty: stateError}">
                        <p>Your Country has no States!</p>
                    </div>
                </div>
                <br><br>

                <!-- For the Cities -->
                <div class="form-settings">
                    <label for="cities" class="label">Select Your City</label>
                    <select class="select" name="cities" id="cities" v-model="selectedCity">
                        <option :value="city.name" v-for="city in cities" :key="city.id" class="count-option">{{ city.name }}</option>
                    </select>
                    <div v-if="cityError" :class="{empty: cityError}">
                        <p>Your State has no Cities!</p>
                    </div>
                </div>      
            </form>
        </div>

        <div class="column-2">
            <div class="selected-options">
                <p v-if="selectedCountry"><span class="selected-options">Your Country:</span> {{ selectedCountry }}</p>
                <br>

                <p v-if="selectedState"><span class="selected-options">Your State:</span> {{ selectedState }}</p>
                <br>

                <p v-if="selectedCity"><span class="selected-options">Your City:</span> {{ selectedCity }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'directories',
    data() {
        return {
            title: 'Wejapa Countries',
            selectedCountry: "",
            selectedState: "",
            selectedCity: "",
            stateError: false,
            cityError: false,
            countries: [],
            states: [],
            cities: []
        }
    },

    created() {
        this.$http.get('https://raw.githubusercontent.com/dr5hn/countries-states-cities-database/master/countries%2Bstates%2Bcities.json').then(
            response=> {
                return response.json()
            }
        ).then(data=> {
            this.countries = data.slice(0, 251);
        })    
    },
    methods: {
        onCountrySelect(event) {
            const state = event.target.value
            const index = this.countries.findIndex((x=>x.name === state))
            const countryState = this.countries[index]['states']
            if (countryState.length === 0) {
                this.stateError = !this.stateError 
                this.states = null   
                this.cities = ""
            } else {
                this.states = countryState
                this.stateError = false
            }
        },
        onStateSelect(event) {
            const state = event.target.value
            const index = this.states.findIndex((x=>x.name === state))
            const city = this.states[index]['cities']
            if (city.length === 0) {
                this.cityError = !this.cityError
            } else {
                this.cities = city
                this.cityError = false
            }
        }
    }
}
</script>

<style scoped>

    @import url('https://fonts.googleapis.com/css2?family=Vidaloka&display=swap');

    .navbar {
        position: relative;
        width: 100%;
        height: auto;
        background: #111;
        margin: 0;
        padding: 0;
    }
    .navbar a {
        text-decoration: none;
    }
    .title {
        text-align: center;
    }
    .country-settings {
        padding: 20px;
        font-family: 'Vidaloka', serif;
    }
    .select  {
        width: 20em;
        height: 2.4em;
        display: block;
        margin-top: 15px;
        padding-left: 1em;
        border: 2px solid #DDD;
        box-shadow: 5px 10px 8px #111;
        line-height: 2em;
        outline: none;
        border-radius: 30px;
        font-family: 'Vidaloka', serif;
        font-size: 18px;
        background: url('https://img.icons8.com/ios-filled/50/000000/chevron-down.png') no-repeat right;
        background-size: 22px 22px;
        background-color: #EEE;
        cursor: pointer;
        -webkit-appearance: none;
        background-position-x: 17.8em;
    }
    .count-option {
        display: block;
        margin-left: 20px;
        font-family: 'Vidaloka', serif;
    }
    .column {
        width: 50%;
        padding-left: 35%;
        margin-bottom: 2em;
        height: auto;
    }
    .column-2 {
        width: 50%;
        padding-left: 35%;
        margin-bottom: 2em;
        height: auto;
    }
    .row:after {
        content: "";
        display: table;
        clear: both;
    }
    .empty {
        border: 2px solid;
        background: rgb(226, 64, 64);
        color: #fff;
        width: 20em;
        height: 3em;
        padding-left: 1em;
        border-radius: 20px;
        margin-top: 20px;
    }
    .empty p {
        position: relative;
        top: 50%;
        transform: translateY(-50%);
    }
    .selected-options {
        margin-top: 10px;
    }
    .label, .selected-options {
        font-size: 20px;  
    }
    /* Media Queries */
    /* 1024px */
    @media screen and (max-width: 1024px) {
        .column {
            float: left;
            width: 50%;
            padding-left: 12%;
        }
        .column-2 {
            float: left;
            width: 50%;
            padding-left: 10%;
        }
    }
    /* 768px */
    @media screen and (max-width: 768px) {
        .column {
            float: left;
            width: 50%;
            padding-left: 8%;
        }
        .column-2 {
            float: left;
            width: 50%;
            padding-left: 10%;
        }
    }
    /* 575px */
    @media screen and (max-width: 575px) {
        .column {
            float: left;
            width: 50%;
            padding-left: 8%;
            margin-bottom: 2em;
            height: auto;
        }
        .column-2 {
            position: relative;
            margin-bottom: 2em;
            padding-left: 8%;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
            height: auto;
        }
    }
     /* 581px */
    @media screen and (max-width: 581px) {
        .column {
            float: left;
            width: 50%;
            padding-left: 8%;
            margin-bottom: 2em;
            height: auto;
        }
        .column-2 {
            position: relative;
            margin-bottom: 2em;
            padding-left: 8%;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
            height: auto;
        }
    }
    /* 400px */
    @media screen and (max-width: 400px) {
        .column {
            float: left;
            width: 50%;
            padding-left: 2%;
            margin-bottom: 2em;
            height: auto;
        }
        .column-2 {
            position: relative;
            margin-bottom: 2em;
            padding-left: 2%;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
            height: auto;
        }
    }
    /* 380px */
    @media screen and (max-width: 380px) {
        .column {
            float: left;
            width: 50%;
            padding-left: 2%;
            margin-bottom: 2em;
            height: auto;
        }
        .column-2 {
            position: relative;
            margin-bottom: 2em;
            padding-left: 2%;
            left: 50%;
            transform: translateX(-50%);
            width: 100%;
            height: auto;
        }
    }
    
    /* 370px */
    @media screen and (max-width: 370px) {
        .column, .column-2 {
            padding-left: 2%;
        }
    }
    /* 320px */
    @media screen and (max-width: 320px) {
        .title {
            font-size: 28px;
        }
        .column, .column-2 {
            padding-left: 8%;
        }
        select {
            width: 17em;
            background-position-x: 14.8em;
        }
        .empty {
            width: 17em;
        }
    }
</style>