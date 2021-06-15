<script>
  import { fade } from "svelte/transition";
  import { weatherStore } from "./store";

  let temperature;

  const unsubscribe = weatherStore.subscribe((val) => {
    temperature = val.main;
  });
</script>

<div transition:fade>
  <h3>Temperature</h3>
  <div>Main: {temperature.temp}</div>
  <div>Max: {temperature.temp}</div>
  <div>Min: {temperature.temp}</div>
  <div>Feels like: {temperature.feels_like}</div>
</div>
