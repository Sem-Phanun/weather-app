<template>
    <div class="flex flex-col flex-1 items-center">
        <div v-if="route.query.preview" class="text-white p-4 bg-weather-secondary w-full text-center">
            <p>
                You are currently preview this city, click the "+" icon to start tracking this city.
            </p>
        </div>
        <div class="flex flex-col items-center text-white py-12">
            <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
            <!-- <p class="text-sm mb-12">
                {{ 
                    new Date(weatherData.currentTime).toLocaleDateString(
                        "en-us",
                        {
                            weekday: "short",
                            day: "2-digit",
                            month: "long"
                        }
                    )
                }}
                {{ 
                    new Date(weatherData.currentTime).toLocaleTimeString(
                        "en-us",
                        {
                            timeStyle: "short"
                        }
                    )
                }}
            </p> -->
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const openWeaterApiKey = "5077beb552d783b812f1ee9fea3187a7"
const getWeaterData = async () => {
    try {
        const weatherData = await axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.lon}&appid=${openWeaterApiKey}&units=imperial`)
        
        //calculate current date & time

        const localOffset = new Date().getTimezoneOffset() * 60000
        const utc = weatherData.data.cureent.dt * 1000 + localOffset
        weatherData.data.currentTime = utc + 1000 * weatherData.data.timezone_offset

        //calculate hourly weather offset
        weatherData.data.hourly.forEach((hour) => {
            const utc = hour.dt * 1000 + localOffset
            hour.currentTime = utc + 1000 * weatherData.data.timezone_offset
        });
        
    } catch (error) {
        console.log(error)
    }
}

const weatherData = await getWeaterData()
console.log(weatherData)
</script>

