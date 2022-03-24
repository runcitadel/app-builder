<script>
  import a11yDark from "svelte-highlight/src/styles/a11y-dark";
  import YAML from "yaml";
  import { Highlight } from "svelte-highlight";
  import yamlHighlight from "svelte-highlight/src/languages/typescript";
  let name = "";
  let tagline = "";
  let version = "";
  let category = "";
  let developer = "";
  let website = "";
  let repository = "";
  let defaultPassword = "";
  let mainImage = "";
  let mainPort = 0;
  $: appData = YAML.stringify({
    version: 2,
    metadata: {
      name,
      tagline,
      version,
      category,
      developer,
      website,
      repository,
      ...(defaultPassword ? { defaultPassword } : {}),
    },
    containers: [{
      name: "main",
      image: mainImage,
      port: parseInt(mainPort),
      permissions: ["lnd"]
    }],
  });
</script>

<svelte:head>
  {@html a11yDark}
</svelte:head>
<main>
  <div class="left-view half">
    <h2>Metadata</h2>
    <label for="appName">App name</label><input
      bind:value={name}
      id="appName"
      placeholder="The best app"
    />
    <label for="appTagline">App tagline</label><input
      bind:value={tagline}
      id="appTagline"
      placeholder="The only app you'll ever need on your Citadel"
    />
    <label for="appVersion">App version</label><input
      bind:value={version}
      id="appVersion"
      placeholder="1.0.0"
    />
    <label for="appCategory">App category</label><input
      bind:value={category}
      id="appCategory"
      placeholder="Lightning node management"
    />
    <label for="appDeveloper">App developer</label><input
      bind:value={developer}
      id="appDeveloper"
      placeholder="Your name"
    />
    <label for="appWebsite">App website</label><input
      bind:value={website}
      id="appWebsite"
      placeholder="https://example.com"
    />
    <label for="appRepo">App source code repository</label><input
      bind:value={repository}
      id="appRepo"
      placeholder="https://github.com/yourapp/source-code"
    />
    <label for="appPw">App default password (optional)</label><input
      bind:value={defaultPassword}
      id="appPw"
      placeholder="password123!"
    />
    <h2>Containers</h2>
    <h4>Main container</h4>
    <label for="mainImage">Docker container</label><input
      bind:value={mainImage}
      id="mainImage"
      placeholder="ghcr.io/runcitadel/lnbits-legends:main"
    />
    <label for="mainPort">Port the container exposes</label><input
      bind:value={mainPort}
      id="mainPort"
      placeholder="3000"
    />
  </div>
  <div class="right-view half highlight">
    <Highlight language={yamlHighlight} code={appData} />
  </div>
</main>

<style>
  main {
    display: flex;
    text-align: center;
    padding: 1em;
    max-height: 95vh;
  }

  .left-view {
    border-right: 1px solid grey;
    min-width: 30vw;
    max-width: 500px;
    margin-right: 2rem;
    overflow-y: scroll;
    max-height: 92vh;
  }

  .right-view {
    min-width: 47%;
    max-width: calc(95vw - 500px);
    width: 100%;
  }

  label {
    margin-bottom: 0.5rem;
  }

  input {
    width: 75%;
  }
</style>
