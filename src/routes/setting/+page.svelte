<script lang="ts">
	import { onMount } from "svelte";
	import ButtonForm from "./ButtonForm.svelte";

  let webhookUrl = $state<string>("");
  let webhookName = $state<string>("");
  let webhookIconUrl = $state<string>("");
  let buttons = $state<Array<[label: string, text: string]>>([["", ""]]);

  onMount(() => {
    const savedWebhookUrl = localStorage.getItem("discord_webhook_url");
    const savedWebhookName = localStorage.getItem("discord_webhook_name");
    const savedWebhookIconUrl = localStorage.getItem("discord_webhook_icon_url");
    const savedButtons = localStorage.getItem("discord_buttons");

    if (savedWebhookUrl) webhookUrl = savedWebhookUrl;
    if (savedWebhookName) webhookName = savedWebhookName;
    if (savedWebhookIconUrl) webhookIconUrl = savedWebhookIconUrl;
    if (savedButtons) {
      try {
        buttons = JSON.parse(savedButtons);
      } catch (e) {
        console.error("Failed to parse saved buttons:", e);
      }
      if (buttons.length === 0) buttons = [["", ""]];
    }
  });

  function addButton() {
    buttons = [...buttons, ["", ""]];
  }

  function saveAndReturn() {
    localStorage.setItem("discord_webhook_url", webhookUrl);
    localStorage.setItem("discord_webhook_name", webhookName);
    localStorage.setItem("discord_webhook_icon_url", webhookIconUrl);
    localStorage.setItem("discord_buttons", JSON.stringify(buttons.filter(b => b[0] !== "")));
    window.location.href = "/";
  }
</script>

<h1>設定</h1>
<hr />
<div class="container">
  <p><input class="form-item" type="text" bind:value={webhookUrl} placeholder="ウェブフックURL" /></p>
  <p>※サーバ設定 &gt; 連携サービス &gt; ウェブフック</p>
  <p><input class="form-item" type="text" bind:value={webhookName} placeholder="名前" /></p>
  <p><input class="form-item" type="text" bind:value={webhookIconUrl} placeholder="アイコンURL(空欄可)" /></p>
</div>
<hr />
{#each buttons as _, i }
  <div class="container">
    <ButtonForm bind:label={buttons[i][0]} bind:text={buttons[i][1]} />
  </div>
  <hr />
{/each}
<p><button class="form-item" onclick={addButton}>追加</button></p>
<p>※ボタン名が空だとボタンは自動的に削除されます。</p>
<p>※テキストが空だとボタン名をテキストとして送信します。</p>
<p><button class="form-item" style="margin-top: 1rem;" onclick={saveAndReturn}>保存してもどる</button></p>

<style>
  :global(.row) {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  :global(p) {
    margin: 8px 0;
    font-size: 5vw;
  }
  .container {
    padding: 0 8px;
  }
  input.form-item {
    font-size: 5vw;
  }
</style>
