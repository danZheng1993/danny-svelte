<script>
  import Temperature from "./Temperature.svelte";
  import Wind from "./Wind.svelte";
  import { weatherStore } from "./store";

  export let city;
  let loading;
  let weather;

  async function fetchWeather(cityInfo) {
    if (cityInfo) {
      loading = true;
      weather = await fetch(
        `https://api.openweathermap.org/data/2.5/weather?q=${cityInfo.city}&appid=3b4026801a3109a1fa1a44b66ecbc3f4`
      ).then((res) => res.json());
      weatherStore.set(weather);
      loading = false;
    }
  }

  $: if (city) {
    fetchWeather(city);
  }
</script>

<div>
  {#if loading}
    <p>Loading weather data...</p>
  {:else if weather}
    <Temperature />
    <Wind />
  {:else}
    <p>No information available</p>
  {/if}
</div>
