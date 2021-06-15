<script>
  import CityInput from "./CityInput.svelte";
  import Weather from "./Weather.svelte";

  let city;
</script>

<main>
  <CityInput bind:city />
  {#if city}
    <p>Selected City: {city.city}, {city.region}, {city.country}</p>
    <Weather {city} />
  {/if}
</main>
