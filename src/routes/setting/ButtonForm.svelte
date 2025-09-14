<script lang="ts">
  let { label = $bindable(""), text = $bindable("") }: {
    label: string;
    text: string;
  } = $props();
</script>

<div class="row">
  <div class="text-container">
    <p><input class="form-item" type="text" placeholder="ボタン名" bind:value={label}></p>
  </div>
  <button onclick={() => { label = ""; }}>Ｘ</button>
</div>
<div class="row">
  <div class="text-container">
    <p><input class="form-item" type="text" placeholder="送信されるテキスト" bind:value={text}></p>
  </div>
  <div>
    <button onclick={() => { text = ""; }}>Ｘ</button>
  </div>
</div>

<style>
  .row button {
    aspect-ratio: 1 / 1;
    font-size: 7vw;
    width: 10vw;
    flex-shrink: 0;
  }
  .row .text-container {
    width: 100%;
    margin-right: 8px;
  }
</style>
