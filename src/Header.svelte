<script>
  import { createEventDispatcher } from 'svelte'

  const dispatch = createEventDispatcher()

  function handleChange(e) {
    dispatch('message', { filter: e.target.value })
  }
</script>

<nav class="navbar navbar-inverse navbar-fixed-top">
  <div class="container-fluid">
    <div class="navbar-header">
      <span class="navbar-brand">GoCore Monitor</span>
    </div>

    <div class="navbar-collapse">
      <!-- Search -->
      <form class="navbar-form navbar-right" role="search">
        <div class="form-group">
          <input
            type="text"
            placeholder="Filter..."
            class="form-control"
            on:input={handleChange} />
        </div>
      </form>
    </div>
  </div>
</nav>
