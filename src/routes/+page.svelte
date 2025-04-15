<script>
  import { PUBLIC_GEMINI_API_KEY } from "$env/static/public";

  import { GoogleGenAI } from "@google/genai";

  const ai = new GoogleGenAI({ apiKey: PUBLIC_GEMINI_API_KEY });

  let answer = $state("Hello World");
  let count = $state("");

  // State Management
  let messages = $state([]); // Store chat history
  let input = $state(""); // User input
  let loading = $state(false); // Loading state
  let chat; // Store chat instance

  // Message sending function
  async function sendMessage() {
    if (!input.trim()) return;

    loading = true;
    const userMessage = input;
    messages = [...messages, { role: "user", text: userMessage }];
    input = ""; // Clear input early for better UX

    try {
      if (!chat) {
        chat = ai.chats.create({
          model: "gemini-2.0-flash",
        });
      }

      // Add an initial empty message for streaming
      messages = [...messages, { role: "model", text: "" }];

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
      }
    } catch (error) {
      console.error("Error:", error);
      messages = [
        ...messages,
        { role: "error", text: `Failed to get response: ${error.message}` },
      ];
    } finally {
      loading = false;
    }
  }
</script>

<div class=" max-w-2xl p-4 marker-1 h-screen">
  <div
    class="bg-white rounded-lg relative
              before:absolute before:-inset-1 before:blur-xl before:bg-gradient-to-r
              before:from-teal-300 before:to-blue-300 before:opacity-50 before:-z-10
              ring-4 ring-teal-100/30"
  >
    <!-- Messages area -->
    <div class="h-[650px] overflow-y-auto p-4 space-y-4">
      {#each messages as msg}
        <div
          class="flex {msg.role === 'user' ? 'justify-end' : 'justify-start'}"
        >
          <div
            class="max-w-[70%] rounded-lg p-3 {msg.role === 'user'
              ? 'bg-teal-500 text-white'
              : 'bg-gray-100 text-gray-800'}"
          >
            <p>{msg.text}</p>
          </div>
        </div>
      {/each}

      {#if loading}
        <div class="flex justify-start">
          <div class="bg-gray-100 rounded-lg p-3">
            <p class="animate-pulse">Thinking...</p>
          </div>
        </div>
      {/if}
    </div>

    <!-- Input area -->
    <div class="border-t p-4 flex gap-2">
      <input
        type="text"
        bind:value={input}
        onkeydown={(e) => e.key === "Enter" && !loading && sendMessage()}
        placeholder="Type your message..."
        class="flex-1 p-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-teal-500"
        disabled={loading}
      />
      <button
        onclick={sendMessage}
        disabled={loading}
        class="px-4 py-2 bg-teal-500 text-white rounded-lg disabled:opacity-50 hover:bg-teal-600 transition-colors"
      >
        Send
      </button>
    </div>
  </div>
</div>
