<script lang="ts">
  import './tailwind.svelte'
  import { onMount } from 'svelte'
  import Clipboard from './components/clipboard.svelte'
  import slugify from 'slugify'
  import { keyCodes } from './constants'

  let isLowercase = true

  let isStrict = true

  let showCopied = false

  let currentValueInput

  $: currentValue = ''

  $: resultValue = slugify(currentValue, {
    lower: isLowercase,
    strict: isStrict,
  })

  const onCopy = () => {
    if (!resultValue) return

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

  const onHandleWindowKeyDown = event => {
    event = event || window.event // IE support

    var keyCode = event.keyCode

    var ctrlDown = event.ctrlKey || event.metaKey // Mac support

    const focusInInput = document.activeElement === currentValueInput

    if (ctrlDown && keyCode === keyCodes.C && !focusInInput) {
      onCopy()

      currentValueInput.focus()
    } else if (keyCode === keyCodes.F && !focusInInput) {
      currentValueInput.focus()

      event.preventDefault()
    }
  }
</script>

<svelte:window on:keydown={onHandleWindowKeyDown} />

<div class="flex flex-col items-center p-6 max-w-xl mx-auto min-h-full">
  <div class="flex justify-center my-2 lg:my-5">
    <img
      class="w-10 h-10 lg:w-14 lg:h-14"
      src="/isotipo.png"
      alt="URL Slug Generator - Slugger"
    />
  </div>

  <h1 class="text-xl lg:text-4xl mb-6 text-center font-bold text-white">
    URL Slug Generator
  </h1>

  <div class="flex flex-col w-full space-y-6 mb-6">
    <form class="flex flex-col w-full" spellcheck="false">
      <label for="text" class="mb-2 font-bold text-white">
        Introduce your string
      </label>
      <textarea
        id="text"
        name="text"
        rows={4}
        autofocus
        class="rounded-md text-dark p-4 focus:ring-4 ring-red-300 outline-none"
        autocomplete="off"
        placeholder="Enter your text"
        bind:value={currentValue}
        bind:this={currentValueInput}
      />

      <div class="mt-5">
        <label class="text-white block">
          <input type="checkbox" bind:checked={isLowercase} />
          <span class="ml-2">Convert to lower case </span>
        </label>
        <label class="text-white block mt-2">
          <input type="checkbox" bind:checked={isStrict} />
          <span class="ml-2">Strip special characters except replacement</span>
        </label>
      </div>

      <div class="mt-5 bg-white rounded-md overflow-hidden">
        <div class="p-3 lg:p-4 flex justify-between items-center bg-green-300">
          <h2 class="font-bold">Generated Slug URL</h2>
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
        <div class="p-3 lg:px-4 lg:py-6">
          {#if resultValue}
            <span class="break-all">
              {resultValue}
            </span>
          {:else}
            <p class="lg:text-lg text-center text-green-400">
              waiting for you to write something...
            </p>
          {/if}
        </div>
      </div>

      {#if showCopied}
        <p class="text-white font-bold lg:text-lg text-center my-5">
          The result was copied...
        </p>
      {/if}
    </form>
  </div>

  <!-- Footer -->
  <div class="text-white mt-auto lg:text-lg">
    <p>
      Created with <span class="px-1 text-red-500"
        ><svg viewBox="0 0 512 512" class="h-4 inline-block" fill="currentColor"
          ><path
            d="M462.3 62.6C407.5 15.9 326 24.3 275.7 76.2L256 96.5l-19.7-20.3C186.1 24.3 104.5 15.9 49.7 62.6c-62.8 53.6-66.1 149.8-9.9 207.9l193.5 199.8c12.5 12.9 32.8 12.9 45.3 0l193.5-199.8c56.3-58.1 53-154.3-9.8-207.9z"
          /></svg
        ></span
      >
      by
      <a
        href="https://yonycalsin.com"
        target="_blank"
        class="font-bold hover:underline"
        rel="noopener noreferrer">@yonycalsin</a
      >
    </p>
  </div>
</div>

<div id="clipboard" />

<style>
  :global(body) {
    @apply bg-dark;
  }
</style>
