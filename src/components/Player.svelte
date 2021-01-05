<script>
  import ResourcePicker from "./ResourcePicker.svelte";
  export let obsFilesDir;
  export let maps = [];
  export let civs = [];
  export let number = 1;
  const fs = require("fs");

  $: enabled = {
    map1: false,
    map2: false,
    ban: false,
  };
  const player = {
    map1: {},
    map2: {},
    ban: {},
  };

  async function resourceSelected({ detail, type }) {
    player[type] = detail;
    const playerDir = obsFilesDir + "/player" + number;
    console.log(playerDir);
    //findOrCreateDir(playerDir);
    fs.copyFileSync(detail.fullPath, playerDir + "/" + type + ".png", (err) => {
      if (err) throw err;
      console.log("source.txt was copied to destination.txt");
    });
    enabled[type] = false;
  }
</script>

<style>
  .form-section {
    border-bottom: 2px solid #e8bc54;
    margin: 1em;
    padding: 1em;
  }
  .player {
    background: rgba(50, 50, 50, 0.7);
    width: 30%;
    border: 1px solid #e8bc54;
    display: flex;
    align-items: center;
    justify-content: center;
    text-align: center;
    flex-direction: column;
  }
  b {
    display: block;
    text-transform: uppercase;
  }
  img {
    width: 100px;
  }
</style>

<div class="player">
  <h1>Player {number}</h1>
  {#each Object.keys(player) as key}
    <div class="form-section">
      <p>{key}</p>
      {#if enabled[key]}
        <ResourcePicker
          resource={key.includes('map') ? maps : civs}
          on:resourceSelected={({ detail }) => resourceSelected({
              detail,
              type: key,
            })} />
      {/if}

      <div
        on:click={() => {
          enabled[key] = true;
        }}>
        <img src={player[key].fullPath} alt="" />
        <b>{player[key].name || 'Click to add'}</b>
      </div>
    </div>
  {/each}
</div>
