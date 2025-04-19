<script>
  let { files = $bindable([]) } = $props();
  let listOfFiles = $derived(files);
  import { File } from "@lucide/svelte";
  import { fade, fly } from "svelte/transition";
</script>

<!-- {#if hoveredFile}
  <div class="absolute top-0 left-0">
    <p>{hoveredFile.name}</p>
  </div>
{:else}
  <p>No file hovered</p>
{/if} -->

<div
  class="flex justify-between {listOfFiles.length > 0 ? 'py-1' : ''} rounded-xl"
>
  {#if listOfFiles.length > 0}
    <div class=" flex flex-wrap items-center gap-2 text-xs">
      {#each listOfFiles as file}
        <div
          in:fade={{ duration: 150 }}
          out:fade={{ duration: 150 }}
          class="relative flex flex-row gap-2 items-center px-3 py-2 max-w-40 bg-gray-800 rounded-xl"
        >
          <button
            onclick={() => {
              //   remove the file from the files array
              listOfFiles = files.filter((f) => f.name !== file.name);
              files = listOfFiles;
            }}
            class="absolute h-6 w-6 -top-2 -right-2 bg-gray-200 hover:bg-gray-500 text-gray-900 rounded-full flex items-center justify-center p-1"
          >
            X
          </button>
          <div class="flex flex-col gap-1 items-center">
            <File class="w-6 h-6 " />
            <p>{file.name.split(".")[1].toUpperCase()}</p>
          </div>
          <div class="flex flex-wrap">
            <p>{file.name.slice(0, 20)}...</p>
          </div>
        </div>

        <!-- <p>{file.name.slice(0, 40)}...</p> -->
      {/each}
    </div>
    <!-- {:else}
    <p>No files uploaded</p> -->
  {/if}
</div>
