<script>
  import Player from "./components/Player.svelte";
  const { readdir, mkdir, existsSync, copyFile } = require("fs-promise");
  const path = require("path");

  const rootDir = path.join(__dirname, "..");
  const staticDir = rootDir + "/src/static";

  let civs = [];
  let maps = [];

  async function readStatic(dir) {
    const fullPath = `${staticDir}/${dir}`;
    const files = await readdir(fullPath);
    return files
      .map((fileName) => {
        const name = fileName.replace(".png", "");
        const resourceType = name.split("_")[0];

        return {
          fullPath: `${fullPath}/${fileName}`,
          type: resourceType,
          name: name
            .replace(resourceType + "_", "")
            .replace("-", " ")
            .replace("_", " "),
        };
      })
      .sort((a, b) => a.name.localeCompare(b.name));
  }

  async function init() {
    civs = await readStatic("Civilizations");
    maps = (await readStatic("Maps")).filter((map) => map.type !== "frame");
  }

  function findOrCreateDir(dir) {
    if (!existsSync(dir)) {
      console.log("Creating obs file dir");
      mkdir(dir);
    }
  }
  const obsFilesDir = rootDir + "/obs-files";

  findOrCreateDir(obsFilesDir);
  init();
</script>

<style>
  .form-wrapper {
    display: flex;
    justify-content: space-around;
    align-items: flex-start;
  }
</style>

<div class="form-wrapper">
  <Player {obsFilesDir} number="1" {maps} {civs} />
  <Player {obsFilesDir} number="2" {maps} {civs} />
</div>
