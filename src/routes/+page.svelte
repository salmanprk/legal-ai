<script>
  import { PUBLIC_GEMINI_API_KEY } from "$env/static/public";
  import Button from "$lib/components/ui/button/button.svelte";

  import { GoogleGenAI } from "@google/genai";

  const ai = new GoogleGenAI({ apiKey: PUBLIC_GEMINI_API_KEY });

  let answer = $state("Hello World");
  let count = $state("");

  // State Management
  let messages = $state([]); // Store chat history
  let input = $state(""); // User input
  let loading = $state(false); // Loading state
  let chat; // Store chat instance
  let messagesContainer; // Reference to the messages div

  // Add this scroll function
  function scrollToBottom() {
    if (messagesContainer) {
      messagesContainer.scrollTo({
        top: messagesContainer.scrollHeight,
        behavior: "smooth",
      });
    }
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
</script>

<div class=" p-4">
  <div class=" rounded-lg relative">
    <!-- Messages area -->
    <div
      class="h-[650px] scroller overflow-y-auto p-4 space-y-4 scrollbar-thin scrollbar-thumb-teal-600 scrollbar-track-teal-950/50"
      bind:this={messagesContainer}
    >
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
    </div>

    <!-- Input area -->
    <div
      class="p-4 flex flex-col gap-2 bg-teal-700/30 rounded-xl min-h-[100px] shadow-lg mt-2"
    >
      <input
        type="text"
        bind:value={input}
        onkeydown={(e) => e.key === "Enter" && !loading && sendMessage()}
        placeholder="Type your message..."
        class="flex-1 p-2 rounded-lg focus:outline-none"
        disabled={loading}
      />
      <div class="flex justify-end">
        <Button
          onclick={sendMessage}
          disabled={loading}
          class="px-4 py-4 bg-teal-700 text-white rounded-lg disabled:opacity-50 hover:bg-teal-600 transition-colors"
        >
          Ask
        </Button>
      </div>
    </div>
  </div>
</div>

<style lang="css">
</style>
