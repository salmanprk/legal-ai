<script>
  import Button from "$lib/components/ui/button/button.svelte";

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
  class="btn flex items-center gap-2 bg-orange-700"
>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    class="h-5 w-5"
    viewBox="0 0 20 20"
    fill="currentColor"
  >
    <path
      fill-rule="evenodd"
      d="M3 17a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM6.293 6.707a1 1 0 010-1.414l3-3a1 1 0 011.414 0l3 3a1 1 0 01-1.414 1.414L11 5.414V13a1 1 0 11-2 0V5.414L7.707 6.707a1 1 0 01-1.414 0z"
      clip-rule="evenodd"
    />
  </svg>
  Upload
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
