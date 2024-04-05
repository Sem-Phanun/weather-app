<template>
    <main class="container text-white">
        <div class="pt-4 mb-8 relative">
            <input type="text"
                v-model="searchQuery"
                @input="getSearchResult"
                placeholder="search for city"
                class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
            />
            <ul class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
                v-if="openWeaterSearchResults"
            >
                <p v-if="searchError">Sorry, some thing when wrong please try again.</p>
                <p v-if="!searchError && openWeaterSearchResults.length === 0">
                    No results match your query, try a different term.
                </p>
                
                <template v-else>
                    <li v-for="searchResult in openWeaterSearchResults" :key="searchResult.id"
                        @click="previewCity(searchResult)"
                        class="py-2 cursor-pointer"
                    >
                        {{ searchResult.name }}
                    </li>
                </template>
            </ul>
        </div>
    </main>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import { useRouter } from 'vue-router';

const router = useRouter()
const openWeaterApiKey = "87827d49e501833a411130e6b0e300a1"

const searchQuery = ref("")
const queryTimeout = ref(null)
const openWeaterSearchResults = ref(null)
const searchError = ref(null)

const previewCity = (searchResult) => {
    console.log(searchResult)
    const [city, state] = searchResult.name;
    router.push({
        name: "city",
        params: {state: state, city: city},
        query: {
            lat: searchResult.lat,
            lon: searchResult.lon,
            preview: true
        }
    })
}

const getSearchResult = () => {
    clearTimeout(queryTimeout.value)
    queryTimeout.value = setTimeout(async()=> {
        if(searchQuery.value !== ""){
            try {
                const result = await axios.get(`http://api.openweathermap.org/geo/1.0/direct?q=${searchQuery.value}&limit=5&appid=${openWeaterApiKey}`)
                openWeaterSearchResults.value = result.data
                console.log(openWeaterSearchResults.value)
            } catch (error) {
                searchError.value(error)
            }
            return;
        }
        openWeaterSearchResults.value = null
    }, 300)
}
</script>
