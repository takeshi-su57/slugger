<script lang="ts">
  import './tailwind.svelte'
  export let name: string

  let currentInputValue = ''
</script>

<div class="flex flex-col items-center p-6 max-w-xl mx-auto min-h-full">
  <h1 class="text-4xl mb-6 text-center font-bold">Slugger the plain text</h1>

  <div class="flex flex-col w-full space-y-6 mb-6">
    <form class="flex flex-col w-full" spellcheck="false">
      <label for="text" class="mb-2 font-bold">
        Introduce a text to slug
      </label>
      <textarea
        id="text"
        name="text"
        rows={3}
        class="rounded-md text-dark p-4 focus:ring-4 ring-indigo-300 outline-none"
        autocomplete="off"
        bind:value={currentInputValue}
        placeholder="Enter your text"
      />
    </form>
  </div>
</div>

<style>
  :global(body) {
    @apply bg-dark text-white;
  }
</style>
