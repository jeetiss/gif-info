<script>
  export let label;
  export let value;
</script>

<div>
  <span class="label">{label}</span>
  <strong>{value}</strong>
</div>

<style>
  div {
    display: flex;
    justify-content: space-between;

    padding: 8px 0;
  }

  .label {
    color: #757575;
  }

  strong {
    font-weight: 600;
  }
</style>
