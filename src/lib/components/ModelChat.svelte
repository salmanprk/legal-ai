<script>
  import { Paperclip } from "@lucide/svelte";
  import * as Select from "$lib/components/ui/select/index.js";
  import { marked } from "marked";
  import AnimatedLoader from "./custom/animatedLoader.svelte";
  import { createUserContent, createPartFromUri } from "@google/genai";
  import { fade, fly } from "svelte/transition";
  const models = [
    { value: "gemini-2.5-flash-preview-04-17", label: "Gemini 2.5 Flash" },
    { value: "gemini-2.5-pro-preview-03-25", label: "Gemini 2.5 Pro" },
    { value: "gemini-2.0-flash", label: "Gemini 2.0 Flash" },
    { value: "gemini-2.0-flash-lite", label: "Gemini 2.0 Flash Lite" },
    { value: "gemini-1.5-flash", label: "Gemini 1.5 Flash" },
    { value: "gemini-1.5-flash-8b", label: "Gemini 1.5 Flash 8B" },
    { value: "gemini-1.5-pro", label: "Gemini 1.5 Pro" },
    { value: "gemini-1.0-pro", label: "Gemini 1.0 Pro" },
  ];

  // Props using the new runes syntax
  let {
    modelId = "",
    modelName = "",
    systemInstructions = "",
    responseFormat = "",
    immigrationLawText = "",
    ai,
    sharedInput = "",
    sharedFiles = [],
    uploadedFiles = [],
    sendMessageTrigger = false,
    // messageSent = () => {},
    loading = $bindable(false),
    llmState = "Let me think...",
    modelState = $bindable([]),
    index = 0,
  } = $props();
  let currentModel = $state(modelId);
  let previousModel = $state(modelId);

  // Local state using reactive runes
  let messages = $state([
    {
      role: "user",
      text: "This?", //"Here's a breakdown of Canadian immigration law, focusing on key aspects relevant to RCICs",
      files: [
        {
          name: "Resume - Software Engineer.pdf",
          uri: "https://generativelanguage.googleapis.com/v1beta/files/bdpm4ato6onk",
          mimeType: "application/pdf",
        },
        {
          name: "Salman Alam (2004-2008) - Diplomatic Passport-compressed.pdf",
          uri: "https://generativelanguage.googleapis.com/v1beta/files/g0nrwtcw9jy6",
          mimeType: "application/pdf",
        },
      ],
      model: "user",
    },
    // {
    //   role: "model",
    //   text: immigrationLawText,
    //   files: [],
    //   model: currentModel,
    // },
    // {
    //   role: "user",
    //   text: "I've attached my study permit application. Can you check if anything is missing?",
    //   files: [],
    //   model: "user",
    // },
    // {
    //   role: "model",
    //   text: "I've reviewed your study permit application and found a few items that need attention:\n\n✅ **Personal information** is complete\n✅ **Educational background** is well documented\n\n❌ **Missing:** Financial proof documents\n❌ **Incomplete:** Letter of acceptance (missing signature)\n\nI recommend attaching your bank statements from the last 6 months and requesting a properly signed letter from your educational institution.",
    //   files: [],
    //   model: currentModel,
    // },
  ]);
  let chat = $state(null);
  let messagesContainer;

  // Watch for send message trigger with effect rune
  $effect(() => {
    if (
      sendMessageTrigger === true &&
      sharedInput.trim() !== "" &&
      sharedInput.trim().length > 0
    ) {
      setTimeout(() => {
        console.log(
          "Triggered",
          models.find((m) => m.value === currentModel)?.label
        );

        handleSendMessage(sharedInput, sharedFiles);
      }, 0);
    } else {
      console.log(`${currentModel} Not triggered`);
    }
  });

  // Function to safely render markdown
  function renderMarkdown(text) {
    try {
      return marked(text);
    } catch (error) {
      console.error("Markdown parsing error:", error);
      return text;
    }
  }

  // Helps to scroll to the bottom of the messages container
  function scrollToBottom() {
    if (messagesContainer) {
      messagesContainer.scrollTo({
        top: messagesContainer.scrollHeight,
        behavior: "smooth",
      });
    }
  }

  // Upload files
  // async function uploadFiles(filesToUpload) {
  //   console.log(`Uploading files for ${currentModel}...`);

  //   const uploadPromises = Array.from(filesToUpload).map(async (file) => {
  //     try {
  //       const uploadedFile = await ai.files.upload({
  //         file: file,
  //         config: { mimeType: file.type },
  //       });
  //       return {
  //         name: file.name,
  //         uri: uploadedFile.uri,
  //         mimeType: uploadedFile.mimeType,
  //       };
  //     } catch (error) {
  //       console.error(`Error uploading file ${file.name}:`, error);
  //       throw error;
  //     }
  //   });

  //   const uploadedFiles = await Promise.all(uploadPromises);
  //   return uploadedFiles;
  // }

  // Create content with files
  async function createContentWithFiles(uploadedFiles, text) {
    let parts = [];
    uploadedFiles.forEach((file) => {
      parts.push(createPartFromUri(file.uri, file.mimeType));
    });
    parts.push(text);
    return createUserContent(parts);
  }

  // Convert messages to chat history format
  function convertToChatHistory(msgs) {
    return msgs
      .filter((msg) => msg.role === "user" || msg.role === "model")
      .map((msg) => {
        if (msg.role === "user") {
          if (msg.files && msg.files.length > 0) {
            const parts = [];
            msg.files.forEach((file) => {
              parts.push({
                fileData: { fileUri: file.uri, mimeType: file.mimeType },
              });
            });
            parts.push({ text: msg.text });
            return { role: "user", parts };
          } else {
            return { role: "user", parts: [{ text: msg.text }] };
          }
        } else {
          return { role: "model", parts: [{ text: msg.text }] };
        }
      });
  }

  // Send message
  async function handleSendMessage(text, filesToProcess) {
    if (!text.trim() && filesToProcess.length === 0) return;

    loading = true;
    llmState = "Let me think...";
    // let uploadedFiles = [];

    try {
      // Initialize chat if needed
      if (!chat || previousModel !== currentModel) {
        chat = ai.chats.create({
          model: currentModel,
          config: {
            systemInstruction: `System Instructions: ${systemInstructions}, Response Format: ${responseFormat}`,
          },
          history: convertToChatHistory(messages),
        });
        previousModel = currentModel;
      }

      // Create message content
      let messageContent;
      if (uploadedFiles.length > 0) {
        messageContent = await createContentWithFiles(
          uploadedFiles,
          text || "Please analyze these documents"
        );
      } else {
        messageContent = createUserContent(text);
      }

      // Add user message to chat history
      messages = [
        ...messages,
        {
          role: "user",
          text: text,
          files: uploadedFiles,
          model: "user",
        },
      ];

      setTimeout(() => scrollToBottom(), 0);

      // Add initial empty message for streaming
      messages = [
        ...messages,
        { role: "model", text: "", files: [], model: currentModel },
      ];
      // sharedInput = "";
      setTimeout(() => scrollToBottom(), 0);

      // Send message to model
      llmState =
        uploadedFiles.length > 0 ? "Reading documents..." : "Let me think...";
      const stream = await chat.sendMessageStream({
        message: messageContent,
      });

      // Process streaming response
      llmState = "Generating response...";
      let fullResponse = "";

      for await (const chunk of stream) {
        fullResponse += chunk.text;
        messages = messages.map((msg, index) =>
          index === messages.length - 1 ? { ...msg, text: fullResponse } : msg
        );
        setTimeout(() => scrollToBottom(), 0);
      }
    } catch (error) {
      console.error(`Error in ${currentModel}:`, error);
      const errorMessage =
        error instanceof Error ? error.message : "Unknown error";

      // messages = [
      //   ...messages,
      //   {
      //     role: "error",
      //     text: `Failed to get response: ${errorMessage}`,
      //     files: [],
      //     model: currentModel,
      //   },
      // ];
      setTimeout(() => scrollToBottom(), 0);
    } finally {
      loading = false;
      llmState = "Let me think...";
      modelState[index].isRunning = false;
    }
  }
</script>

<div
  class="flex flex-col w-full h-full border-0 border-teal-900/30 overflow-hidden scroller"
>
  <span class="text-white">{modelState[index].isRunning}</span>
  <div class="bg-teal-900/30 p-4">
    <!-- <h3 class="text-white font-medium">{modelName}</h3> -->

    <Select.Root
      onValueChange={(v) => {
        // console.log("Selected", v);
        // console.log(
        //   "Current Model",
        //   currentModel,
        //   "Previous Model",
        //   previousModel
        // );
      }}
      type="single"
      name="favoriteFruit"
      bind:value={currentModel}
    >
      <Select.Trigger
        class="w-48 border-1 border-teal-700 text-white focus:ring-none focus:border-1 focus:outline-none focus:ring-0 focus:ring-offset-0 ring-0 outline-none placeholder:text-gray-300"
      >
        <h3 class="text-white font-medium">
          {models.find((m) => m.value === currentModel)?.label}
        </h3>
        <!-- {models.find((m) => m.value === currentModel)?.label} -->
      </Select.Trigger>
      <Select.Content class="z-100 bg-teal-950">
        <div transition:fade={{ duration: 150 }}>
          <Select.Group>
            <Select.GroupHeading class="text-gray-200 text-base font-semibold">
              Models
            </Select.GroupHeading>
            {#each models as model}
              <Select.Item
                class="text-gray-200 hover:bg-teal-600"
                value={model.value}
                label={model.label}>{model.label}</Select.Item
              >
            {/each}
          </Select.Group>
        </div>
      </Select.Content>
    </Select.Root>
  </div>

  <div
    bind:this={messagesContainer}
    class="flex-grow overflow-y-auto py-4 px-2"
  >
    <div class="flex flex-col gap-6">
      {#each messages as msg}
        <div
          class="flex {msg.role === 'user' ? 'justify-end' : 'justify-start'}"
        >
          <div
            class="flex flex-col gap-2 {msg.role === 'user'
              ? 'items-end'
              : 'items-start'}"
          >
            <div
              class="markdown prose prose-invert prose-teal rounded-xl px-4 py-2 {msg.role ===
              'user'
                ? 'bg-teal-900'
                : ''} text-gray-200 max-w-[90%]"
            >
              {@html renderMarkdown(msg.text)}
            </div>

            {#if msg.role === "user" && msg.files && msg.files.length > 0}
              <div class="flex flex-col gap-1 pr-4">
                {#each msg.files as file}
                  <p
                    class="hover:underline underline-offset-2 flex items-center gap-1 text-gray-200 text-xs"
                  >
                    <Paperclip class="text-teal-600" size={16} />
                    {file?.name.slice(0, 30) + "..."}
                  </p>
                {/each}
              </div>
            {/if}

            {#if msg.role === "model" && msg.text.length > 0}
              <div
                class="text-gray-200 text-xs bg-blue-800 rounded-full px-2 py-1 w-fit"
              >
                <p>{msg.model}</p>
              </div>
            {/if}
          </div>
        </div>
      {/each}

      {#if loading}
        <div class="flex justify-center">
          <AnimatedLoader text={llmState} />
        </div>
      {/if}
    </div>
  </div>
</div>
