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
  let activeModels = $state([
    {
      id: "gemini-2.5-flash-preview-04-17",
      name: "Gemini 2.5 Flash",
      isRunning: false,
    },
    {
      id: "gemini-2.5-pro-preview-03-25",
      name: "Gemini 2.5 Pro",
      isRunning: false,
    },
    {
      id: "gemini-2.0-flash",
      name: "Gemini 2.0 Flash",
      isRunning: false,
    },
  ]);

  // Shared state
  let input = $state("");
  let files = $state([]);
  let sendTrigger = $state(false);
  //   let sendingComplete = $state(0);
  let uploadedFiles = $state([]);
  let loading = $state(false);
  let llmState = $state("Thinking...");
  let isUploading = $state(false);

  // Add the uploadFiles function to MultiChatInterface
  async function uploadToFilesAPI(docs) {
    console.log("Uploading files centrally...");
    // loading = true;
    llmState = "Uploading documents...";
    const uploadPromises = Array.from(docs).map(async (doc) => {
      try {
        // llmState = `Uploading ${file.name}...`;
        const uploadedFile = await ai.files.upload({
          file: doc.file,
          config: { mimeType: doc.file.type },
        });
        return {
          name: doc.file.name,
          uri: uploadedFile.uri,
          mimeType: uploadedFile.mimeType,
        };
      } catch (error) {
        console.error(`Error uploading file ${doc.file.name}:`, error);
        throw error;
      }
    });

    return await Promise.all(uploadPromises);
  }

  let allModelsFinished = $derived(
    activeModels.every((model) => !model.isRunning)
  );
  $effect(() => {
    // console.log("Model State", modelState);
    console.log("All Models Finished", allModelsFinished);
  });

  // Trigger message send to all components
  async function sendMessageToAll() {
    // if (!input.trim() && files.length === 0) return;

    // Upload files if needed
    if (files.length > 0) {
      try {
        isUploading = true;
        loading = true;
        llmState = "Uploading documents...";
        uploadedFiles = await uploadToFilesAPI(files);
        console.log("Files uploaded centrally:", uploadedFiles);
      } catch (error) {
        console.error("Error uploading files:", error);
        loading = false;
        isUploading = false;
        return; // Prevent proceeding if file upload fails
      } finally {
        loading = false;
        isUploading = false;
      }
    } else {
      uploadedFiles = [];
      console.log("No files to upload");
    }

    // Trigger the models to process message
    sendTrigger = true;
    // activeModels.forEach((model) => {
    //   model.isRunning = true;
    // });

    // Reset trigger on next tick to ensure components have time to read it
    setTimeout(() => {
      console.log("Resetting trigger");
      sendTrigger = false;
      input = "";
      files = [];
    }, 10);
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
              {systemInstructions}
              {responseFormat}
              {ai}
              sharedInput={input}
              sharedFiles={files}
              {uploadedFiles}
              sendMessageTrigger={sendTrigger}
              {loading}
              {llmState}
              bind:modelState={activeModels[index]}
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
  <section class=" p-4 lg:p-8 grow text-white min-w-full mx-auto gap-2">
    <div class="bg-teal-950 p-4 rounded-2xl">
      <FilesViewer {isUploading} bind:files />
      <div class="grid lg:grid-cols-12 gap-4 items-center">
        <div class="lg:col-span-9">
          <div class="p-4 bg-teal-950 rounded-2xl">
            <Textarea
              onkeydown={(e) => {
                if (e.key === "Enter" && e.metaKey) {
                  if (loading || !allModelsFinished || !input.trim()) {
                    return;
                  } else {
                    e.preventDefault();
                    sendMessageToAll();
                  }
                }
              }}
              bind:value={input}
              placeholder="Ask anything..."
              class="bg-teal-950 text-gray-200 scroller-2 flex-1 min-h-[80px] leading-relaxed focus:ring-none border-none focus:border-none focus:border-0 focus:outline-none focus:ring-0 focus:ring-offset-0 ring-0 outline-none placeholder:text-gray-300"
            />
          </div>
        </div>
        <!-- <div class="flex w-full justify-end"> -->
        <div
          class=" w-full lg:col-span-3 flex lg:flex-col justify-end lg:justify-end lg:items-end gap-1"
        >
          <Button
            onclick={() => {
              if (!input.trim() && allModelsFinished) {
                sendMessageToAll();
              }
            }}
            disabled={(!input.trim() && files.length === 0) ||
              sendTrigger ||
              !allModelsFinished ||
              isUploading}
            class="btn w-fit"
          >
            Let's go <Command /><CornerDownLeft strokeWidth={2.5} />
          </Button>
          <Uploader bind:files />
        </div>
      </div>
    </div>
    <!-- </div> -->
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
