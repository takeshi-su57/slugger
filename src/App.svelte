<script lang="ts">
  import './tailwind.svelte'
  import Clipboard from './components/clipboard.svelte'
  import slugify from 'slugify'

  let isLowercase = true

  let isStrict = true

  let showCopied = false

  $: resultValue = ''

  const onCopy = () => {
    const app = new Clipboard({
      target: document.getElementById('clipboard'),
      props: {
        value: resultValue,
      },
    })

    app.$destroy()

    showCopied = true

    setTimeout(() => {
      showCopied = false
    }, 1000)
  }
</script>

<div class="flex flex-col items-center p-6 max-w-xl mx-auto min-h-full">
  <h1 class="text-4xl mb-6 text-center font-bold text-white">
    Slugger the plain text
  </h1>

  <div class="flex flex-col w-full space-y-6 mb-6">
    <form class="flex flex-col w-full" spellcheck="false">
      <label for="text" class="mb-2 font-bold text-white">
        Introduce a text to slug
      </label>
      <textarea
        id="text"
        name="text"
        rows={5}
        class="rounded-md text-dark p-4 focus:ring-4 ring-indigo-300 outline-none"
        autocomplete="off"
        placeholder="Enter your text"
        on:input={event => {
          resultValue = slugify(event.target.value, {
            lower: isLowercase,
            strict: isStrict,
          })
        }}
      />

      <div class="mt-5">
        <label class="text-white block">
          <input type="checkbox" bind:checked={isLowercase} />
          Convert to lower case
        </label>
        <label class="text-white block mt-2">
          <input type="checkbox" bind:checked={isStrict} />
          Strip special characters except replacement
        </label>
      </div>

      <div class="mt-5 bg-white rounded-md overflow-hidden">
        <div class="flex justify-between items-center p-4 bg-green-300">
          <h2 class="font-bold">Result</h2>
          <span
            class="w-4 h-4 cursor-pointer hover:bg-blue-300"
            on:click={onCopy}
          >
            <svg
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="3"
              stroke-linecap="round"
              ><rect x="9" y="9" width="13" height="13" rx="2" ry="2" /><path
                d="M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1"
              /></svg
            >
          </span>
        </div>
        <div class="px-4 py-6">
          {#if resultValue}
            <span>
              {resultValue}
            </span>
          {:else}
            <p class="text-lg text-center text-green-400">
              waiting for you to write something...
            </p>
          {/if}
        </div>
      </div>

      {#if showCopied}
        <p class="text-white font-bold text-lg text-center my-5">
          The result was copied...
        </p>
      {/if}
    </form>
  </div>
</div>

<div id="clipboard" />

<style>
  :global(body) {
    @apply bg-dark;
  }
</style>
