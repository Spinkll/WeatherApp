<template>
  <div
    v-for="(day, index) in selectedForecastDays"
    :key="index"
    class="q-mt-lg row justify-between"
  >
    <div class="col-sm-12 data-wrap-forecast">
      <p class="q-mt-lg text-body1 text-center text-secondary">
        {{ haveData ? getFormattedDate(index) : 'Loading...' }}
      </p>
      <div class="row q-my-lg justify-around">
        <div class="col-sm-12 col-md-5">
          <div class="row justify-between items-center">
            <div class="col-12 col-md-6 text-center">
             <img src="../assets/Temperature.svg" alt="Icon" class="icon" />
            </div>
            <div class="col-12 col-md-6">
              <div class="text-accent text-center">
                <p class="text-body2">Temperature</p>
              </div>
              <div class="text-secondary text-center">
                <p class="text-body1">
                  {{ day.day?.avgtemp_c + ' °C' || 'N/A' }}
                </p>
              </div>
            </div>
          </div>
        </div>
        <div class="col-sm-12 col-md-5">
          <div class="row justify-between items-center">
            <div class="col-12 col-md-6 text-center">
              <img
                :src="day.day?.condition?.icon || 'N/A'"
                :alt="day.day?.condition?.text || 'N/A'"
              />
            </div>
            <div class="col-12 col-md-6">
              <div class="text-accent text-center">
                <p class="text-body2">General Condition</p>
              </div>
              <div class="text-secondary text-center">
                <p class="text-body1">
                  {{ day.day?.condition?.text || 'N/A' }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="row q-my-lg justify-around">
        <div class="col-sm-12 col-md-5">
          <div class="row justify-between items-center">
            <div class="col-12 col-md-6 text-center">
           <img src="../assets/Wind_Speed.svg" alt="Icon" class="icon" />
              </div>
            <div class="col-12 col-md-6">
              <div class="text-accent text-center">
                <p class="text-body2">Wind Speed</p>
              </div>
              <div class="text-secondary text-center">
                <p class="text-body1">
                  {{ day.day?.maxwind_kph + ' km/h' || 'N/A' }}
                </p>
              </div>
            </div>
          </div>
        </div>
        <div class="col-sm-12 col-md-5">
          <div class="row justify-between items-center">
            <div class="col-12 col-md-6 text-center">
           <img src="../assets/Humidity.svg" alt="Icon" class="icon" />
            </div>
            <div class="col-12 col-md-6">
              <div class="text-accent text-center">
                <p class="text-body2">Humidity</p>
              </div>
              <div class="text-secondary text-center">
                <p class="text-body1">
                  {{ day.day?.avghumidity + ' %' || 'N/A' }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script setup>
import { storeToRefs } from 'pinia';
import { useWeatherStore } from '../stores/WeatherStore';
import { parse, format } from 'date-fns';
import { computed } from 'vue';

const { weatherData, haveData } = storeToRefs(useWeatherStore());

const weatherDataAny = weatherData;
const selectedForecastDays = computed(() => {
  if (haveData.value && weatherDataAny.value.forecast?.forecastday) {
    return weatherDataAny.value.forecast.forecastday.slice(1, 3);
  }
  return [];
});

const getFormattedDate = (index) => {
  if (
    haveData.value &&
    weatherDataAny.value.forecast &&
    weatherDataAny.value.forecast.forecastday[index + 1]?.date
  ) {
    const date = parse(
      weatherDataAny.value.forecast.forecastday[index + 1].date,
      'yyyy-MM-dd',
      new Date()
    );
    return format(date, 'dd MMMM, EEEE');
  }
  return 'N/A';
};
</script>

<style lang="scss" scoped>
.small-icon {
  font-size: 2rem;
  color: $secondary;
}
</style>
