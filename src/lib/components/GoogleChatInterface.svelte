<script>
  import FilesViewer from "./FilesViewer.svelte";

  //   import { Icon, ArrowUp, ArrowTurnDownLeft } from "svelte-hero-icons";
  import {
    CornerDownLeft,
    Command,
    Paperclip,
    MessageCircleQuestion,
  } from "@lucide/svelte";

  import { PUBLIC_GEMINI_API_KEY } from "$env/static/public";
  import Button from "$lib/components/ui/button/button.svelte";
  import Uploader from "$lib/components/custom/uploader.svelte";
  import WelcomeScreen from "$lib/components/WelcomeScreen.svelte";
  import { onMount } from "svelte";
  import {
    GoogleGenAI,
    createUserContent,
    createPartFromUri,
  } from "@google/genai";
  import { marked } from "marked";
  import Textarea from "$lib/components/ui/textarea/textarea.svelte";
  import systemInstructionsText from "$lib/prompts/system-instructions.md?raw";
  import responseFormatText from "$lib/prompts/response-format.md?raw";
  import immigrationLawText from "$lib/prompts/immigration-law-breakdown.md?raw";
  import AnimatedLoader from "./custom/animatedLoader.svelte";

  let value = $state("");

  let model = $state("gemini-2.5-pro-preview-03-25");
  //   let model = $state("gemini-2.0-flash");
  let llmState = $state("Thinking...");
  const ai = new GoogleGenAI({ apiKey: PUBLIC_GEMINI_API_KEY });
  let listOfFiles = $state([]);
  let files = $state([]);
  // State Management
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
    {
      role: "model",
      text: immigrationLawText,
      files: [],
      model: model,
    },
    {
      role: "user",
      text: "I've attached my study permit application. Can you check if anything is missing?",
      files: [],
      model: "user",
    },
    {
      role: "model",
      text: "I've reviewed your study permit application and found a few items that need attention:\n\n✅ **Personal information** is complete\n✅ **Educational background** is well documented\n\n❌ **Missing:** Financial proof documents\n❌ **Incomplete:** Letter of acceptance (missing signature)\n\nI recommend attaching your bank statements from the last 6 months and requesting a properly signed letter from your educational institution.",
      files: [],
      model: model,
    },
  ]); // Store chat history
  let input = $state(""); // User input
  let loading = $state(false); // Loading state
  let chat; // Store chat instance
  let messagesContainer; // Reference to the messages div
  let systemInstructions = $state(systemInstructionsText);
  let responseFormat = $state(responseFormatText);
  let temp = immigrationLawText;
  let showWelcomeScreen = $state(true);

  $effect(() => {
    if (files) {
      console.log("Selected files", files);
      //   uploadFile();
      //   console.log("Added files?", files);
      showWelcomeScreen = false;
    }
  });
  // Configure marked for security
  marked.setOptions({
    gfm: true, // GitHub Flavored Markdown
    breaks: true, // Convert \n to <br>
    sanitize: true, // Sanitize HTML tags
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
  // List all files uploaded to Google
  async function listAllFiles() {
    let tempArray = [];
    let listResponses = await ai.files.list({ config: { pageSize: 10 } });
    for await (const file of listResponses) {
      console.log("File", file);
      tempArray.push(file);
    }
    return tempArray;
  }
  // Upload file
  async function uploadFile() {
    console.log("Uploading to Google Files API", files);

    // Create an array of upload promises
    const uploadPromises = Array.from(files).map(async (file) => {
      console.log("Preparing to upload file", file.name);
      try {
        const uploadedFile = await ai.files.upload({
          file: file,
          config: { mimeType: file.type },
        });
        console.log("Uploaded file", uploadedFile);
        return {
          name: file.name,
          uri: uploadedFile.uri,
          mimeType: uploadedFile.mimeType,
        };
      } catch (error) {
        console.error(`Error uploading file ${file.name}:`, error);
        throw error; // Re-throw to be caught by Promise.all if needed
      }
    });

    // Wait for all uploads to complete
    const listOfUploadedFiles = await Promise.all(uploadPromises);
    console.log("Result from Google Files API", listOfUploadedFiles);
    return listOfUploadedFiles;
  }
  // Create full content if there is a file
  async function fullContent(uploadedFile, userMessage) {
    let parts = [];
    uploadedFile.forEach((file) => {
      parts.push(createPartFromUri(file.uri, file.mimeType));
    });
    parts.push(userMessage);
    const content = createUserContent(parts);
    return content;
  }
  // Message sending function
  async function sendMessage(templatedPrompt = "") {
    const userText = templatedPrompt || input;
    if (!userText.trim() && !files) return;
    let filesToInclude = files;
    showWelcomeScreen = false;
    loading = true;
    const userMessage = userText;
    llmState = "Let me think...";
    let displayMessage = userMessage;
    setTimeout(() => scrollToBottom(), 0);

    try {
      if (!chat) {
        chat = ai.chats.create({
          model: model, //"gemini-2.0-flash",
          config: {
            systemInstruction: `System Instructions: ${systemInstructions}, Response Format: ${responseFormat}`,
          },
        });
      }

      let messageContent;

      // If there's a file, use your existing functions
      if (filesToInclude.length > 0) {
        llmState = "Uploading your documents...";
        const uploadedFile = await uploadFile();
        files = [];
        console.log("Uploaded file", uploadedFile);
        messageContent = await fullContent(
          uploadedFile,
          userMessage || "Please analyze this document"
        );
        console.log("Message content", messageContent);
        // displayMessage += `[File: ${files[0].name}]`;
      } else {
        messageContent = createUserContent(userMessage);
      }

      // Add message to chat history
      messages = [
        ...messages,
        {
          role: "user",
          text: displayMessage,
          files: filesToInclude,
          model: "user",
        },
      ];
      input = "";

      // Scroll after user message is added (give DOM time to update)
      setTimeout(() => scrollToBottom(), 0);

      // Add initial empty message for streaming
      messages = [
        ...messages,
        { role: "model", text: "", files: [], model: model },
      ];

      // Scroll again after model placeholder is added
      setTimeout(() => scrollToBottom(), 0);

      console.log("Sending message to Google Chat", messageContent);
      if (filesToInclude.length > 0) {
        llmState = "Reading your documents...";
      }
      const stream = await chat.sendMessageStream({
        message: messageContent,
      });
      //   loading = false;
      llmState = "Generating your response...";
      let fullResponse = "";

      //   Handle streaming chunks
      for await (const chunk of stream) {
        // console.log("Chunk received:", chunk.text);
        fullResponse += chunk.text;
        messages = messages.map((msg, index) =>
          index === messages.length - 1 ? { ...msg, text: fullResponse } : msg
        );
        // Using setTimeout to ensure DOM updates before scrolling
        setTimeout(() => scrollToBottom(), 0);
      }

      //   console.log("Messages", fullResponse);
      //   console.log("HTML", renderMarkdown(fullResponse));

      // Clear the file after sending
      files = [];
    } catch (error) {
      console.error("Error:", error);

      // Safely access error message
      const errorMessage =
        error instanceof Error ? error.message : "Unknown error occurred";

      messages = [
        ...messages,
        {
          role: "error",
          text: `Failed to get response: ${errorMessage}`,
          files: [],
          model: model,
        },
      ];
      setTimeout(() => scrollToBottom(), 0);
    } finally {
      loading = false;
      llmState = "Thinking deeply...";
    }
  }

  // Handle template prompt selection
  function handleSelectPrompt(event) {
    const { prompt } = event.detail;
    sendMessage(prompt);
  }

  // Handle document upload button
  function handleUploadDocuments() {
    // Focus on the file upload element
    const fileInput = document.querySelector('input[type="file"]');
    if (fileInput) {
      fileInput.click();
    }
  }

  // Handle blank conversation start
  function handleStartConversation() {
    showWelcomeScreen = false;
  }

  onMount(async () => {
    // let listResponses = [];
    // listResponses = await listAllFiles();
    // console.log("List of files", listResponses);
  });
</script>

{#snippet chatMessage(msg)}
  <div
    class="markdown prose prose-invert prose-teal rounded-xl px-4 py-2 {msg.role ===
    'user'
      ? 'bg-teal-900'
      : ''}  text-gray-200"
  >
    {@html renderMarkdown(msg.text)}
  </div>
{/snippet}

<div class=" bg-zinc-900 flex flex-col h-screen mx-auto">
  <!-- Messages area -->
  {#if messages.length > 0}
    <section
      bind:this={messagesContainer}
      class=" grid grid-cols-12 scroller overflow-y-auto"
    >
      <div class="py-8 px-4 md:px-16 lg:px-[15%] col-span-12">
        <div class="flex flex-col gap-8">
          {#each messages as msg}
            <div
              class=" flex {msg.role === 'user'
                ? 'justify-end'
                : 'justify-start'}"
            >
              {#if msg.role === "user"}
                <div class="flex flex-col gap-2 items-end">
                  {@render chatMessage(msg)}

                  <div class="flex flex-col gap-1 justify-end pr-4">
                    {#if msg.files && msg.files.length > 0}
                      {#each msg.files as file}
                        <p
                          class="hover:underline underline-offset-2 flex items-center justify-end gap-1 text-gray-200 text-xs"
                        >
                          <Paperclip class="text-teal-600" size={16} />
                          {file?.name.slice(0, 40) + "..."}
                        </p>
                      {/each}
                    {/if}
                  </div>
                </div>
              {/if}

              {#if msg.role === "model"}
                <div class="flex flex-col gap-2 items-start">
                  {@render chatMessage(msg)}

                  {#if msg.text.length > 0}
                    <div
                      class="text-gray-200 text-xs bg-blue-800 rounded-full px-2 py-1 w-fit z-10"
                    >
                      <p>
                        {msg.model.charAt(0).toUpperCase() + msg.model.slice(1)}
                      </p>
                    </div>
                  {/if}
                </div>
              {/if}
            </div>
          {/each}
        </div>

        {#if loading}
          <div class="flex justify-center">
            <AnimatedLoader text={llmState} />
          </div>
        {/if}
      </div>
      <!-- <div class="col-span-3 bg-teal-700/30 rounded-xl shadow-lg p-16">
        {#if listOfFiles}
          {#each listOfFiles as file, index}
            <p class="text-white">{index + 1}. {file.name}</p>
          {/each}
        {:else}
          <p>No files uploaded</p>
        {/if}
  
      </div> -->
    </section>

    <!-- Input area -->
    <section
      class="grow row-span-3 px-4 lg:px-32 flex flex-col text-white min-w-full lg:min-w-[80%] lg:max-w-[85%] mx-auto pb-8 gap-2"
    >
      <FilesViewer bind:files></FilesViewer>
      <div class="p-4 bg-teal-950 shadow rounded-2xl">
        <Textarea
          bind:value={input}
          onkeydown={(e) =>
            e.key === "Enter" && e.shiftKey && !loading && sendMessage()}
          placeholder="Ask me anything..."
          class="bg-teal-950 text-gray-200 scroller-2 flex-1 min-h-[80px] leading-relaxed focus:outline-none border-none focus:border-0 focus:ring-0 focus:ring-offset-0 ring-0 outline-none placeholder:text-gray-300"
          disabled={loading}
        />
      </div>

      <div class="flex justify-end gap-2">
        <Button
          onclick={() => sendMessage()}
          disabled={input.length === 0 || loading}
          class="btn"
          >Ask<Command /><CornerDownLeft strokeWidth={2.5} />
        </Button>
        <Uploader bind:files />
      </div>
    </section>

    <!-- {:else if showWelcomeScreen} -->
  {:else}
    <WelcomeScreen
      on:selectPrompt={handleSelectPrompt}
      on:uploadDocuments={handleUploadDocuments}
      on:startConversation={handleStartConversation}
    />
  {/if}
</div>

<!-- </div> -->

<style>
  /* Add styles for markdown elements */

  :global(.markdown code) {
    background-color: rgba(17, 94, 89, 0.5); /* bg-teal-800/50 */
    padding: 0.125rem 0.25rem;
    border-radius: 0.25rem;
  }

  :global(.markdown pre) {
    background-color: rgba(17, 94, 89, 0.5);
    padding: 1rem;
    border-radius: 0.25rem;
    margin-bottom: 1rem;
    overflow-x: auto;
  }

  :global(.markdown blockquote) {
    border-left: 4px solid rgb(20, 184, 166); /* border-teal-500 */
    padding-left: 1rem;
    font-style: italic;
    margin-bottom: 1rem;
  }

  :global(.markdown a) {
    color: rgb(45, 212, 191); /* text-teal-400 */
    text-decoration: underline;
  }

  :global(.markdown a:hover) {
    color: rgb(94, 234, 212); /* text-teal-300 */
  }

  /* Textarea specific styles */
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
