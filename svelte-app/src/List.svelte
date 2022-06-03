<script>
  export let list = [];
</script>

<div class="county-list">
  {#each list as county}
    <div class="county-list-item">{county}</div>
  {/each}
</div>

<style>
  .county-list {
    columns: 3;
    min-height: 200px;
  }

  .county-list-item {
    font-weight: bold;
    font-size: 24px;
  }
</style>
