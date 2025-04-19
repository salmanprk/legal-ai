<script>
  // Imports...
  //   import WelcomeScreen from "./WelcomeScreen.svelte";
  import * as Resizable from "$lib/components/ui/resizable/index.js";
  import Button from "$lib/components/ui/button/button.svelte";
  import { Command, CornerDownLeft } from "@lucide/svelte";
  import Textarea from "$lib/components/ui/textarea/textarea.svelte";
  import Uploader from "$lib/components/custom/uploader.svelte";
  import ModelChat from "./ModelChat.svelte";
  import FilesViewer from "./FilesViewer.svelte";
  import systemInstructions from "$lib/prompts/system-instructions.md?raw";
  import responseFormat from "$lib/prompts/response-format.md?raw";
  import immigrationLawText from "$lib/prompts/immigration-law-breakdown.md?raw";
  import { GoogleGenAI } from "@google/genai";
  import { PUBLIC_GEMINI_API_KEY } from "$env/static/public";
  const ai = new GoogleGenAI({ apiKey: PUBLIC_GEMINI_API_KEY });
  // Define active models
  let activeModels = [
    {
      id: "gemini-2.5-flash-preview-04-17",
      name: "Gemini 2.5 Flash",
      ref: null,
    },
    { id: "gemini-2.5-pro-preview-03-25", name: "Gemini 2.5 Pro" },
    { id: "gemini-2.0-flash", name: "Gemini 2.0 Flash" },
    // { id: "gemini-1.5-pro", name: "Gemini 1.5 Pro" },
    // Add more models as needed
  ];

  // Shared state
  let input = $state("");
  let files = $state([]);
  let sendTrigger = $state(false);
  //   let sendingComplete = $state(0);
  let uploadedFiles = $state([]);
  let loading = $state(false);
  let llmState = $state("Thinking...");
  let modelState = $state([
    {
      id: 1,
      name: "Gemini 2.5 Flash",
      isRunning: false,
    },
    {
      id: 2,
      name: "Gemini 2.5 Pro",
      isRunning: false,
    },
    { id: 3, name: "Gemini 2.0 Flash", isRunning: false },
  ]);

  // Add the uploadFiles function to MultiChatInterface
  async function uploadFiles(filesToUpload) {
    console.log("Uploading files centrally...");
    // loading = true;
    llmState = "Uploading documents...";
    const uploadPromises = Array.from(filesToUpload).map(async (file) => {
      try {
        // llmState = `Uploading ${file.name}...`;
        const uploadedFile = await ai.files.upload({
          file: file,
          config: { mimeType: file.type },
        });
        return {
          name: file.name,
          uri: uploadedFile.uri,
          mimeType: uploadedFile.mimeType,
        };
      } catch (error) {
        console.error(`Error uploading file ${file.name}:`, error);
        throw error;
      }
    });

    return await Promise.all(uploadPromises);
  }

  let allModelsFinished = $derived(
    modelState.every((model) => !model.isRunning)
  );
  $effect(() => {
    // console.log("Model State", modelState);
    console.log("All Models Finished", allModelsFinished);
  });

  // Trigger message send to all components
  async function sendMessageToAll() {
    if (!input.trim() && files.length === 0) return;

    // Upload files if needed
    if (files.length > 0) {
      try {
        loading = true;
        llmState = "Uploading documents...";
        uploadedFiles = await uploadFiles(files);
        console.log("Files uploaded centrally:", uploadedFiles);
      } catch (error) {
        console.error("Error uploading files:", error);
        loading = false;
        return; // Prevent proceeding if file upload fails
      } finally {
        loading = false;
      }
    } else {
      uploadedFiles = [];
      console.log("No files to upload");
    }

    // Trigger the models to process message
    sendTrigger = true;
    modelState.forEach((model) => {
      model.isRunning = true;
    });

    // Reset trigger on next tick to ensure components have time to read it
    setTimeout(() => {
      console.log("Resetting trigger");
      sendTrigger = false;
      input = "";
      files = [];
    }, 1);
  }
</script>

<div class="bg-zinc-900 flex flex-col h-screen mx-auto">
  <!-- {#if !showWelcomeScreen} -->
  <!-- Chat grid with all model instances -->
  <!-- <section
  class="marker-2 flex-10/12 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-2 gap-1 p-4 lg:px-16 lg:py-8 overflow-y-auto"
> -->
  <section class="flex-10/12 p-4 lg:p-8 overflow-y-auto">
    <Resizable.PaneGroup
      direction="horizontal"
      class="min-h-[200px] rounded-lg border-none"
    >
      {#each activeModels as model, index}
        <!-- <div class="text-white">{JSON.stringify(model)}</div> -->
        <Resizable.Pane defaultSize={50}>
          <div class="flex h-full items-center justify-center border-none">
            <!-- <span class="font-semibold">Sidebar</span> -->

            <ModelChat
              modelId={model.id}
              modelName={model.name}
              {systemInstructions}
              {responseFormat}
              {immigrationLawText}
              {ai}
              sharedInput={input}
              sharedFiles={files}
              {uploadedFiles}
              sendMessageTrigger={sendTrigger}
              {loading}
              {llmState}
              bind:modelState
              {index}
            />
          </div>
        </Resizable.Pane>
        {#if index < activeModels.length - 1}
          <Resizable.Handle
            class="text-gray-200 bg-teal-900 border-2 border-teal-800"
            withHandle
          />
        {/if}
      {/each}
    </Resizable.PaneGroup>
  </section>

  <!-- Shared input section -->
  <section class="p-4 lg:p-8 grow text-white min-w-full mx-auto gap-2">
    <FilesViewer bind:files />
    <div
      class="grid grid-flow-col grid-cols-12 gap-4 justify-center items-center"
    >
      <div class="col-span-10">
        <div class="p-4 bg-teal-950 shadow rounded-2xl">
          <Textarea
            onkeydown={(e) => {
              if (e.key === "Enter" && e.metaKey) {
                if (loading) {
                  return;
                } else {
                  e.preventDefault();
                  sendMessageToAll();
                }
              }
            }}
            bind:value={input}
            placeholder="Ask ..."
            class="bg-teal-950 text-gray-200 scroller-2 flex-1 min-h-[80px] leading-relaxed focus:ring-none border-none focus:border-none focus:border-0 focus:outline-none focus:ring-0 focus:ring-offset-0 ring-0 outline-none placeholder:text-gray-300"
          />
        </div>
      </div>

      <div class="col-span-2 flex flex-col justify-center gap-1">
        <Button
          onclick={() => {
            if (input.trim() !== "" && input.length > 0) {
              sendMessageToAll();
            }
          }}
          disabled={(!input.trim() && files.length === 0) ||
            sendTrigger ||
            !allModelsFinished}
          class="btn w-24"
        >
          Ask <Command /><CornerDownLeft strokeWidth={2.5} />
        </Button>
        <Uploader bind:files />
      </div>
    </div>
  </section>
  <!-- {/if} -->
</div>

<style>
  :global(textarea) {
    /* font-family: inherit;
    font-size: 0.8rem;
    line-height: 1.5;
    letter-spacing: 0.01em; */
    resize: none;
    word-wrap: break-word;
    white-space: pre-wrap;
    box-shadow: none !important;
  }

  /* Ensure consistent focus styles */
  :global(textarea:focus) {
    outline: none !important;
    box-shadow: none !important;
    border: none !important;
  }
</style>
