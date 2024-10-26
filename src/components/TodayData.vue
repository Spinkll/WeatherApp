<template>
  <div class="q-mt-lg row justify-around items-center data-wrap">
    <div class="col-sm-11 col-md-3 text-center date">
      <h4 class="text-h4 text-secondary current-date">
        {{ haveData ? formattedDate : 'Loading...' }}
      </h4>
    </div>
    <div class="col-sm-11 col-md-3 text-center city">
      <h3 class="text-h3 text-secondary text-bold">
        {{ haveData ? weatherData?.location?.name || 'N/A' : 'Loading...' }}
      </h3>
    </div>

    <div class="col-sm-11 col-md-3 text-center country">
      <h4 class="text-h4 text-secondary">
        {{ haveData ? weatherData?.location?.country || 'N/A' : 'Loading...' }}
      </h4>
    </div>
  </div>
  <div class="q-mt-lg row justify-between items-center">
    <div class="col-sm-12 col-md-6 single-data-box q-my-xs q-pa-lg">
      <div class="row items-center">
        <div class="col-12 col-md-6 text-center">
            <img src="../assets/Temperature.svg" alt="Icon" class="icon" />
        </div>
        <div class="col-12 col-md-6">
          <div class="text-accent text-center">
            <h5 class="text-h5 text-bold temperature_label">Temperature</h5>
          </div>
          <div class="text-secondary text-center">
            <h4 class="text-h4 temperature_value">
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
            <h5 class="text-h5 text-bold cond_label">Current Condition</h5>
          </div>
          <div class="text-secondary text-center">
            <h4 class="text-h4 cond_value">
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
           <img src="../assets/Wind_Speed.svg" alt="Icon" class="icon" />
        </div>
        <div class="col-12 col-md-6">
          <div class="text-accent text-center">
            <h5 class="text-h5 wind_label">Wind Speed</h5>
          </div>
          <div class="text-secondary text-center">
            <h4 class="text-h4 wind_value">
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
           <img src="../assets/Humidity.svg" alt="Icon" class="icon" />
        </div>
        <div class="col-12 col-md-6">
          <div class="text-accent text-center">
            <h5 class="text-h5 humidity_label">Humidity</h5>
          </div>
          <div class="text-secondary text-center">
            <h4 class="text-h4 humidity_value">
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

.single-data-box {
  // min-width: 240px;
  @media (min-width: 1024px) {
    max-width: 32vw !important;
  }

  @media (max-width: 599px) {
    width: 100%;
    display: inline-block;
  }
}

.temperature_label, temperature_value,
.wind_label, .wind_value,
.cond_label, .cond_value,
.himidity_label, .humidity_value{
  white-space: nowrap !important;
  overflow: hidden !important;
  text-overflow: ellipsis !important;
}


.city{
  hyphens: auto;
  word-wrap: break-word;

  padding-bottom: 5px !important;
  @media (max-width: 599px) {
    width: 100%;
    display: inline-block;
  }
}

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

</style>
