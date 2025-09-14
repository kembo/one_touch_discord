<script lang="ts">
	import { onMount } from "svelte";

  let buttons = $state<Array<[label: string, text: string]>>([]);
  let webhookUrl = $state<string>("");
  let webhookName = $state<string>("");
  let webhookIconUrl = $state<string>("");

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
    }
  });

  function sendMessage(text: string) {
    if (!webhookUrl) {
      alert("ウェブフックURLが設定されていません。");
      return;
    }

    const payload = {
      content: text,
      username: webhookName || undefined,
      avatar_url: webhookIconUrl || undefined,
    };

    fetch(webhookUrl, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify(payload),
    })
      .then((response) => {
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        console.log("Message sent successfully", text);
      })
      .catch((error) => {
        console.error("Error sending message:", error);
        alert("メッセージの送信に失敗しました。");
      });

  }
</script>

{#each buttons as [label, text] }
  <p><button class="form-item" onclick={() => sendMessage(text || label)}>{label}</button></p>
{/each}
<br />
<br />
<a href="/setting">設定</a>

<style>
  button {
    text-overflow: ellipsis;
  }
</style>
