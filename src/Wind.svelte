<script>
  import { fade } from "svelte/transition";
  import { weatherStore } from "./store";

  let wind;

  const unsubscribe = weatherStore.subscribe((val) => {
    wind = val.wind;
  });
</script>

<div transition:fade>
  <h3>Wind</h3>
  <div>Speed: {wind.speed}</div>
  <div>Gust: {wind.gust}</div>
  <div>Deg: {wind.deg}</div>
</div>
