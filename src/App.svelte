<script>
  import { onMount } from 'svelte'

  import Header from './Header.svelte'
  import Service from './Service.svelte'

  const apiURL = '/gocore'
  let data = []

  async function getData() {
    const response = await fetch(apiURL)
    return await response.json()
  }

  onMount(function() {
    setInterval(async () => {
      data = await getData()
    }, 2000)
  })

  async function removeMe(json) {
    try {
      const url = `${apiURL}/${json.host}/${json.address}/${json.serviceName}`
      await fetch(url, { method: 'delete' })
      data = await getData()
    } catch (err) {
      console.log(err)
    }
  }
</script>

<svelte:head>
  <link
    rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" />
  <link
    rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap-theme.min.css" />
</svelte:head>

<main style="margin-top: 6rem;">
  <Header />

  <div class="container">
    {#each data as d}
      <Service {removeMe} json={d} />
    {/each}
  </div>
</main>
