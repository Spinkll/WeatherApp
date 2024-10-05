<template>
  <div class="q-mt-lg row justify-around items-center data-wrap">
    <div class="col-sm-11 col-md-3 text-center">
      <h4 class="text-h4 text-secondary current-date">
        {{ haveData ? formattedDate : 'Loading...' }}
      </h4>
    </div>
    <div class="col-sm-11 col-md-3 text-center">
      <h3 class="text-h3 text-secondary text-bold">
        {{ haveData ? weatherData?.location?.name || 'N/A' : 'Loading...' }}
      </h3>
    </div>

    <div class="col-sm-11 col-md-3 text-center">
      <h4 class="text-h4 text-secondary">
        {{ haveData ? weatherData?.location?.country || 'N/A' : 'Loading...' }}
      </h4>
    </div>
  </div>
  <div class="q-mt-lg row justify-between items-center">
    <div class="col-sm-12 col-md-6 single-data-box q-my-xs q-pa-lg">
      <div class="row items-center">
        <div class="col-12 col-md-6 text-center">
          <i class="fa-solid fa-temperature-high icon"></i>
        </div>
        <div class="col-12 col-md-6">
          <div class="text-accent text-center">
            <h5 class="text-h5 text-bold">Temperature</h5>
          </div>
          <div class="text-secondary text-center">
            <h4 class="text-h4">
              {{
                haveData
                  ? weatherData?.current?.feelslike_c + ' °C' || 'N/A'
                  : 'Loading...'
              }}
            </h4>
          </div>
        </div>
      </div>
    </div>
    <div class="col-sm-12 col-md-6 single-data-box q-my-xs q-pa-lg">
      <div class="row items-center">
        <div class="col-12 col-md-6 text-center">
          <img
            :src="
              haveData
                ? weatherData?.current?.condition?.icon || 'N/A'
                : 'Loading...'
            "
            :alt="
              haveData
                ? weatherData?.current?.condition?.text || 'N/A'
                : 'Loading...'
            "
          />
        </div>
        <div class="col-12 col-md-6">
          <div class="text-accent text-center">
            <h5 class="text-h5 text-bold">Current Condition</h5>
          </div>
          <div class="text-secondary text-center">
            <h4 class="text-h4">
              {{
                haveData
                  ? weatherData?.current?.condition?.text || 'N/A'
                  : 'Loading...'
              }}
            </h4>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="row justify-between items-center">
    <div class="col-sm-12 col-md-6 single-data-box q-my-xs q-pa-lg">
      <div class="row items-center">
        <div class="col-12 col-md-6 text-center">
          <i class="fa-solid fa-wind icon"></i>
        </div>
        <div class="col-12 col-md-6">
          <div class="text-accent text-center">
            <h5 class="text-h5">Wind Speed</h5>
          </div>
          <div class="text-secondary text-center">
            <h4 class="text-h4">
              {{
                haveData
                  ? weatherData?.current?.wind_kph || 'N/A'
                  : 'Loading...'
              }}

              km/h
            </h4>
          </div>
        </div>
      </div>
    </div>
    <div class="col-sm-12 col-md-6 single-data-box q-my-xs q-pa-lg">
      <div class="row items-center">
        <div class="col-12 col-md-6 text-center">
          <i class="fa-solid fa-water icon"></i>
        </div>
        <div class="col-12 col-md-6">
          <div class="text-accent text-center">
            <h5 class="text-h5">Humidity</h5>
          </div>
          <div class="text-secondary text-center">
            <h4 class="text-h4">
              {{
                haveData
                  ? weatherData?.current?.humidity + ' %' || 'N/A'
                  : 'Loading...'
              }}
            </h4>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { storeToRefs } from 'pinia';
import { useWeatherStore } from '../stores/WeatherStore';
import { computed } from 'vue';
import { parse, format } from 'date-fns';

const { weatherData, haveData } = storeToRefs(useWeatherStore());
const weatherDataAny = weatherData;

const formattedDate = computed(() => {
  if (
    haveData.value &&
    weatherDataAny.value.location &&
    weatherDataAny.value.location.localtime
  ) {
    const date = parse(
      weatherDataAny.value.location.localtime,
      'yyyy-MM-dd HH:mm',
      new Date()
    );
    return format(date, 'dd MMMM, EEEE HH:mm');
  }
  return 'N/A';
});
</script>

<style lang="scss" scoped>
h4{
  font-weight: 100;
  font-size: 24px;
}

h5{
  font-weight: 400;
  font-size: 20px / 1.33333;
}

.data-wrap {
  padding: 3rem 2rem;
}

.single-data-box {
  min-width: 240px;
  @media (min-width: 767px) {
    max-width: 32vw !important;
  }
}
</style>
