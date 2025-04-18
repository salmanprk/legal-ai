<script>
  import Button from "$lib/components/ui/button/button.svelte";
  import { Paperclip } from "@lucide/svelte";

  // Add this function to handle file input click
  let { files = $bindable([]) } = $props(); // Reference to hidden file input
  let fileInput;

  function triggerFileInput() {
    console.log("Triggering file input", fileInput);
    fileInput.click();
  }

  function handleFileSelect(event) {
    let existingFiles = files;
    const newFiles = Array.from(event.target.files);

    if (newFiles.length > 0) {
      // Convert FileList to array for easier handling

      if (existingFiles.length > 0) {
        const existingFileNames = new Set(
          Array.from(existingFiles).map((f) => f.name)
        );
        console.log("Existing file names:", existingFileNames);
        const uniqueNewFiles = newFiles.filter(
          (f) => !existingFileNames.has(f.name)
        );
        // file = [...existingFiles, ...uniqueNewFiles];
        if (uniqueNewFiles.length > 0) {
          // Update file list with existing files plus unique new ones
          files = [...existingFiles, ...uniqueNewFiles];
        }
      } else {
        files = newFiles;
      }

      // console.log("Files selected:", files);
    }
  }
</script>

<!-- Replace your current file input with this -->

<!-- Hidden file input -->
<input
  type="file"
  bind:this={fileInput}
  onchange={handleFileSelect}
  class="hidden"
  accept=".pdf,.doc,.docx"
  multiple
/>

<!-- Custom upload button -->
<div class="flex flex-col gap-2">
  <Button
    onclick={triggerFileInput}
    class="btn flex items-center gap-2 bg-orange-700 hover:bg-orange-600"
  >
    {"Add files"}
    <Paperclip class="h-5 w-5" />
  </Button>
</div>
