<script lang="ts">
  import { invoke } from "@tauri-apps/api/tauri";
  import MarkdownIt from "markdown-it";
  import sanitizeHtml from "sanitize-html";
  import { afterUpdate } from "svelte";

  const md = new MarkdownIt();

  let markdown = "";
  export let msg = "";

  afterUpdate(() => {
    msg = sanitizeHtml(md.render(markdown), {
      allowedTags: sanitizeHtml.defaults.allowedTags.concat([
        "h1",
        "h2",
        "img",
      ]),
    });
  });

  async function send() {
    // Learn more about Tauri commands at https://tauri.app/v1/guides/features/command
    msg = await invoke("send", { msg: markdown });
  }
</script>

{@html msg}
<textarea
  class="greet-input"
  placeholder="Enter a name..."
  bind:value={markdown}
/>
<button on:click={send}> Greet </button>
