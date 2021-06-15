<script>
  import { createEventDispatcher } from "svelte";
  import { slide } from "svelte/transition";

  const dispatch = createEventDispatcher();
  export let city;
  let timer;
  let loading = false;
  let cityList = [];
  let showOptions = false;

  const debounce = (v) => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fetchCity(v);
    }, 750);
  };

  async function fetchCity(cityPrefix) {
    showOptions = true;
    loading = true;
    cityList = await fetch(
      `http://geodb-free-service.wirefreethought.com/v1/geo/cities?namePrefix=${cityPrefix}&hateoasMode=false&limit=5&offset=0`
    )
      .then((res) => res.json())
      .then((res) => res.data);
    loading = false;
  }

  function selectCity(cityInfo) {
    dispatch("selectcity", { cityInfo });
    city = cityInfo;
    showOptions = false;
  }
</script>

<div class="city-input-wapper">
  <input type="text" on:keyup={({ target: { value } }) => debounce(value)} />
  {#if showOptions}
    <div class="city-list-wrapper" transition:slide|local>
      {#if loading}
        <p>Loading...</p>
      {:else if cityList.length === 0}
        <p>No City Found</p>
      {:else}
        {#each cityList as cityInfo}
          <div
            class="city-option"
            transition:slide|local
            on:click={() => selectCity(cityInfo)}
          >
            {cityInfo.city},
            {cityInfo.region},
            {cityInfo.country}
          </div>
        {/each}
      {/if}
    </div>
  {/if}
</div>

<style>
  .city-input-wapper {
    position: relative;
  }
  .city-list-wrapper {
    min-height: 28px;
    width: 350px;
    position: absolute;
    background-color: white;
    border: 1px solid #efefef;
  }
  .city-option {
    cursor: pointer;
    padding: 0.8rem;
    border-bottom: 1px solid #efefef;
  }
  .city-option:last-child {
    border-bottom: none;
  }
</style>
