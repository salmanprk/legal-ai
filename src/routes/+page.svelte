<script>
  import { PUBLIC_GEMINI_API_KEY } from "$env/static/public";
  import Button from "$lib/components/ui/button/button.svelte";
  import Uploader from "$lib/components/uploader/uploader.svelte";
  import { onMount } from "svelte";
  import {
    GoogleGenAI,
    createUserContent,
    createPartFromUri,
  } from "@google/genai";

  const ai = new GoogleGenAI({ apiKey: PUBLIC_GEMINI_API_KEY });
  let listOfFiles = $state([]);
  let answer = $state("Hello World");
  let count = $state("");
  let files = $state(null);
  // State Management
  let messages = $state([]); // Store chat history
  let input = $state(""); // User input
  let loading = $state(false); // Loading state
  let chat; // Store chat instance
  let messagesContainer; // Reference to the messages div
  let systemInstructions = $state(
    "You are an advanced AI assistant specialized in analyzing Canadian immigration case documents (specifically GCMS notes and Rule 9 Reasons for Decision) to support Regulated Canadian Immigration Consultants (RCICs). Your purpose is to help RCICs identify potential grounds for Judicial Review (JR) and assess the preliminary likelihood of success based only on the provided documentation and established JR principles"
  );
  let responseFormat = $state(
    "You should respond in a concise and professional manner, providing clear and actionable advice. Your response should be in the following format: <response> <reasoning> <conclusion>"
  );

  $effect(() => {
    console.log("Uploaded files", files);
    if (files) {
      uploadFile();
    }
  });

  // Add this scroll function
  function scrollToBottom() {
    if (messagesContainer) {
      messagesContainer.scrollTo({
        top: messagesContainer.scrollHeight,
        behavior: "smooth",
      });
    }
  }
  async function uploadFile() {
    console.log("Uploading to Google", files);
    const uploadedFile = await ai.files.upload({
      file: files[0],
      config: { mimeType: files[0].type },
    });
    console.log("???", uploadedFile);
    return uploadedFile;
  }

  async function fullContent(uploadedFile, userMessage) {
    const content = createUserContent([
      createPartFromUri(uploadedFile.uri, uploadedFile.mimeType),
      userMessage,
    ]);
    return content;
  }
  // Message sending function
  async function sendMessage() {
    if (!input.trim()) return;

    loading = true;
    const userMessage = input;
    messages = [...messages, { role: "user", text: userMessage }];
    input = ""; // Clear input early for better UX
    scrollToBottom(); // Scroll after user message

    try {
      if (!chat) {
        chat = ai.chats.create({
          model: "gemini-2.0-flash",
          config: {
            systemInstruction: `System Instructions: ${systemInstructions}, Response Format: ${responseFormat}`,
          },
        });
      }

      // Add an initial empty message for streaming
      messages = [...messages, { role: "model", text: "" }];
      //   scrollToBottom(); // Scroll after adding empty model message

      const stream = await chat.sendMessageStream({
        message: userMessage,
      });

      let fullResponse = "";

      // Handle streaming chunks
      for await (const chunk of stream) {
        console.log("Chunk received:", chunk.text);
        fullResponse += chunk.text;
        // Update the last message in real-time
        messages = messages.map((msg, index) =>
          index === messages.length - 1 ? { ...msg, text: fullResponse } : msg
        );
        scrollToBottom(); // Scroll after each chunk
      }
    } catch (error) {
      console.error("Error:", error);
      messages = [
        ...messages,
        { role: "error", text: `Failed to get response: ${error.message}` },
      ];
      scrollToBottom(); // Scroll after error message
    } finally {
      loading = false;
    }
  }

  onMount(async () => {
    let listResponses = [];
    listResponses = await ai.files.list({ config: { pageSize: 10 } });
    for await (const file of listResponses) {
      console.log("File", file);
      listOfFiles.push(file);
    }
    // listResponses.forEach((file) => {
    //   listOfFiles.push(file);
    // });
  });
</script>

<!-- <div class=""> -->
<div class="grid grid-flow-col grid-rows-5 rounded-lg h-screen p-16">
  <!-- Messages area -->
  <div class="row-span-4 grid grid-cols-12 p-4">
    <div
      class="col-span-9 scroller overflow-y-auto space-y-4 scrollbar-thin scrollbar-thumb-teal-600 scrollbar-track-teal-950/50"
      bind:this={messagesContainer}
    >
      {#if messages.length > 0}
        {#each messages as msg}
          <div
            class="flex {msg.role === 'user' ? 'justify-end' : 'justify-start'}"
          >
            <div
              class="max-w-[70%] rounded-lg p-3 {msg.role === 'user'
                ? 'bg-teal-700 text-gray-200'
                : 'text-gray-200'}"
            >
              <p>{msg.text}</p>
            </div>
          </div>
        {/each}

        {#if loading}
          <div class="flex justify-start">
            <div class=" rounded-lg p-3">
              <p class="animate-pulse">Thinking...</p>
            </div>
          </div>
        {/if}
      {:else}
        <div class="flex justify-center items-center h-full">
          <p class="text-gray-400 text-3xl font-semibold">
            Tell me how I can help you today?
          </p>
        </div>
      {/if}
    </div>
    <div class="col-span-3 bg-teal-700/30 rounded-xl shadow-lg p-16">
      {#if listOfFiles}
        {#each listOfFiles as file, index}
          <p class="text-white">{index + 1}. {file.name}</p>
        {/each}
      {:else}
        <p>No files uploaded</p>
      {/if}

      <!-- {listOfFiles} -->
    </div>
  </div>

  <!-- Input area -->
  <div
    class="marker-2 w-full row-span-1 p-4 flex flex-col gap-2 bg-teal-700/30 rounded-xl min-h-[150px] shadow-lg mt-2"
  >
    <div class="flex justify-between">
      <!-- Files: -->
      {#if files}
        <p>{files[0].name}</p>
      {:else}
        <!-- <p>No files uploaded</p> -->
      {/if}
    </div>
    <input
      type="text"
      bind:value={input}
      onkeydown={(e) => e.key === "Enter" && !loading && sendMessage()}
      placeholder="Type your message..."
      class="flex-1 p-2 rounded-lg focus:outline-none"
      disabled={loading}
    />
    <div class="flex justify-end gap-2">
      <!-- <div class="grid w-full max-w-sm items-center gap-1.5">
        <Label for="picture">Picture</Label>
        <Input id="picture" type="file" class="file-input" />
      </div> -->
      <Button onclick={sendMessage} disabled={loading} class="btn">Ask</Button>
      <Uploader bind:file={files} />

      <!-- <Input type="file" class="file-input border-4  border-teal-700" /> -->
    </div>
  </div>
</div>

<!-- </div> -->

<style lang="css">
</style>
