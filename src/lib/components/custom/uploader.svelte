<script>
  import Button from "$lib/components/ui/button/button.svelte";
  import { CirclePlus, Paperclip } from "@lucide/svelte";

  // Add this function to handle file input click
  let { files = $bindable([]) } = $props(); // Reference to hidden file input
  let fileInput;

  function triggerFileInput() {
    // console.log("Triggering file input", fileInput);
    fileInput.click();
    console.log("File input clicked", fileInput);
    console.log("Files", files);
  }
  function handleFileSelect(event) {
    let existingFiles = files;
    console.log("Existing files", existingFiles);
    // console.log("Event", event);
    // Add unique ID to each new file
    let newFiles = Array.from(event.target.files).map((file) => ({
      id: crypto.randomUUID(), // or Date.now() + Math.random()
      file: file,
    }));
    console.log("New files", newFiles);
    if (newFiles.length > 0) {
      // Convert FileList to array for easier handling

      files = [...existingFiles, ...newFiles];
      event.target.value = "";
      // if (existingFiles.length > 0) {
      //   const existingFileNames = new Set(
      //     Array.from(existingFiles).map((f) => f.name)
      //   );
      //   console.log("Existing file names:", existingFileNames);
      //   const uniqueNewFiles = newFiles.filter(
      //     (f) => !existingFileNames.has(f.name)
      //   );
      //   if (uniqueNewFiles.length > 0) {
      //     // Update file list with existing files plus unique new ones
      //     files = [...existingFiles, ...uniqueNewFiles];
      //   }
      // } else {
      //   files = newFiles;
      // }

      // console.log("Files selected:", files);
    }
  }
</script>

<!-- Replace your current file input with this -->

<!-- Hidden file input -->
<input
  type="file"
  bind:this={fileInput}
  oninput={handleFileSelect}
  class="hidden"
  accept=".pdf,.doc,.docx"
  multiple
/>

<!-- Custom upload button -->
<div class="flex flex-col">
  <Button
    onclick={triggerFileInput}
    class="rounded-lg w-fit btn flex items-center gap-2 bg-teal-500 hover:bg-teal-400"
    >Documents
    <CirclePlus class="h-5 w-5" />
  </Button>
</div>
