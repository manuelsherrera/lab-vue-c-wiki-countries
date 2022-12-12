<template>
    <div v-if="selectedCountry" class="col-7">
        <img :src="`https://www.geonames.org/flags/x/${selectedCountry.alpha2Code.toLowerCase()}.gif`"
            :alt="`${selectedCountry.country} flag`" style="width: 300px" />
        <h1>{{ selectedCountry.name.common }}</h1>
        <table class="table">
            <thead></thead>
            <tbody>
                <tr>
                    <td style="width: 30%">Capital</td>
                    <td>{{ selectedCountry.capital[0] }}</td>
                </tr>
                <tr>
                    <td>Area</td>
                    <td>
                        {{ selectedCountry.area }} km <sup>2</sup>
                    </td>
                </tr>
                <tr>
                    <td>Borders</td>
                    <td>
                        <ul>
                            <li
                                v-for="(borderCountryName, index) in getBorderCountryNames(this.countries, this.countryId)">
                                <router-link :to="`/list/${this.selectedCountry.borders[index]}`">
                                    {{ borderCountryName }}
                                </router-link>
                            </li>
                        </ul>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <div class="col-7" v-else>Loading...</div>
</template>
<script>
const API_URL = "https://ih-countries-api.herokuapp.com/countries";
// import COUNTRIES from "/public/countries.json";
export default {
    data() {
        return {
            // countries: COUNTRIES,
            countries: [],
        }
    },
    computed: {
        countryId() {
            return this.$route.params.Id
        },
        countryName() {
            return this.countries
        },
        selectedCountry() {
            return this.countries.find(country => country.alpha3Code === this.countryId);
        },
    },
    methods: {
        async fetchProjects() {
            const response = await fetch(API_URL)
            this.countries = await response.json()
        },
        getBorderCountryNames(countries, countryCode) {
            const country = countries.find(c => c.alpha3Code === countryCode);
            if (!country) {
                return [];
            }
            return country.borders.map(borderCountryCode => {
                const borderCountry = countries.find(c => c.alpha3Code === borderCountryCode);
                return borderCountry ? borderCountry.name.common : null;
            });
        }
    },
    mounted() {
        this.fetchProjects()
    },
}
</script>