<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();

  export let resource;

  let searchWord = "";
  $: filteredResource = resource.filter(
    (r) =>
      searchWord != "" &&
      r.name.toLowerCase().includes(searchWord.toLowerCase())
  );

  function selectResource(resource) {
    dispatch("resourceSelected", resource);
    searchWord = "";
  }
  function keydown({ keyCode }) {
    if (keyCode == "13" && filteredResource.length === 1) {
      selectResource(filteredResource[0]);
    }
  }
</script>

<style>
  .resource-wrapper {
    display: flex;
    align-items: flex-start;
    justify-content: center;
    flex-wrap: wrap;
    padding: 0;
  }

  .resource {
    display: inline-block;
    text-align: center;
    width: 10em;
    height: auto;
    padding: 0.5em;
    margin: 0.5em;
    text-transform: uppercase;
    font-weight: 800;
    font-size: 10px;
    border: 1px black solid;
    cursor: pointer;
  }

  img {
    width: 50px;
  }

  .input-wrapper {
    display: flex;
    justify-content: center;
  }

  input {
    padding: 1em 0.5em 1em 0.5em;
    border-radius: 5px;
    border: 1px solid gray;
  }
</style>

<div>
  <input type="text" bind:value={searchWord} on:keydown={keydown} />
  <ul class="resource-wrapper">
    {#each filteredResource as r}
      <li on:click={selectResource(r)} class="resource">
        <img src={r.fullPath} alt="" />
        <div>{r.name}</div>
      </li>
    {/each}
  </ul>
</div>
