<script>
  import Button from "$lib/components/ui/button/button.svelte";
  import { Paperclip } from "@lucide/svelte";

  // Add this function to handle file input click
  let { file = $bindable() } = $props(); // Reference to hidden file input
  let fileInput;
  function triggerFileInput() {
    console.log("Triggering file input", fileInput);
    fileInput.click();
  }

  let selectedFileName = $state("");

  function handleFileSelect(event) {
    // console.log("File selected", file);
    const files = event.target.files;
    if (files.length > 0) {
      selectedFileName = files[0].name;
      file = files;
      console.log("Files selected:", files);
    }
  }
</script>

<!-- Replace your current file input with this -->

<!-- Hidden file input -->
<input
  type="file"
  bind:this={fileInput}
  on:change={handleFileSelect}
  class="hidden"
  accept=".pdf,.doc,.docx"
/>

<!-- Custom upload button -->
<Button
  onclick={triggerFileInput}
  class="btn flex items-center gap-2 bg-orange-700 hover:bg-orange-600"
>
  Add files <Paperclip class="h-5 w-5" />
</Button>

<!-- Add this above the buttons if you want to show selected file -->
<!-- {#if selectedFileName}
  <div class="text-sm text-gray-200 mb-2 flex items-center gap-2">
    <svg
      xmlns="http://www.w3.org/2000/svg"
      class="h-4 w-4"
      viewBox="0 0 20 20"
      fill="currentColor"
    >
      <path
        fill-rule="evenodd"
        d="M4 4a2 2 0 012-2h4.586A2 2 0 0112 2.586L15.414 6A2 2 0 0116 7.414V16a2 2 0 01-2 2H6a2 2 0 01-2-2V4z"
        clip-rule="evenodd"
      />
    </svg>
    {selectedFileName}
  </div>
{/if} -->
