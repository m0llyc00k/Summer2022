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
    columns: 1;
    min-height: 200px;
  }

  .county-list-item {
    font-weight: normal;
    font-size: 20px;
  }
</style>
