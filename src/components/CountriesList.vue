<template>
    <div class="container">
        <!-- Bootstrap row wrapper div -->
        <div class="row">
            <div class="col-5" style="max-height: 90vh; overflow: scroll">
                <div class="list-group">
                    <router-link v-for="item of countries" class="list-group-item list-group-item-action" :to="{path:`/list/${item.alpha3Code}`}">
                        <img :src="`https://flagpedia.net/data/flags/icon/72x54/${item.alpha2Code.toLowerCase()}.png`" />
                        <p>{{item.name.common}}</p>
                    </router-link>
                    
                </div>
            </div>

            <router-view />
        </div>
    </div>
</template>

<script>
import countries from "/public/countries.json";
export default {
    data() {
        return {
            countries: countries,
        };
    }

}
</script>
