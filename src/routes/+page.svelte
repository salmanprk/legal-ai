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
  import { marked } from "marked";

  import { Carta, Markdown } from "carta-md";
  import "carta-md/default.css"; /* Default theme */

  const carta = new Carta();

  let value = $state("");

  let model = $state("gemini-2.0-flash");

  const ai = new GoogleGenAI({ apiKey: PUBLIC_GEMINI_API_KEY });
  let listOfFiles = $state([]);
  let answer = $state("Hello World");
  let count = $state("");
  let files = $state(null);
  // State Management
  let messages = $state([
    // {
    //   role: "user",
    //   text: "Here's a breakdown of Canadian immigration law, focusing on key aspects relevant to RCICs",
    //   files: "OP",
    // },
    // {
    //   role: "model",
    //   text: "Here's a breakdown of Canadian immigration law",
    //   files: null,
    // },
  ]); // Store chat history
  let input = $state(""); // User input
  let loading = $state(false); // Loading state
  let chat; // Store chat instance
  let messagesContainer; // Reference to the messages div
  let systemInstructions = $state(
    "You are an advanced AI assistant specialized in analyzing Canadian immigration case documents (specifically GCMS notes and Rule 9 Reasons for Decision) to support Regulated Canadian Immigration Consultants (RCICs). Your purpose is to help RCICs identify potential grounds for Judicial Review (JR) and assess the preliminary likelihood of success based only on the provided documentation and established JR principles"
  );
  let responseFormat = $state(
    "As an AI assistant for this application, your primary goal is to provide clear, helpful, and neatly presented information. To achieve this, ALWAYS format your entire response using Markdown. Structure your answers logically using appropriate Markdown elements like headings (##, ###), lists (*, 1.), bold (**), italics (*), code formatting (`code`, ```), and tables when suitable. Well-structured Markdown is crucial for the user experience in the app, so prioritize clean, readable, and consistent formatting in every response."
  );

  let temp = `Here's a breakdown of Canadian immigration law, focusing on key aspects relevant to RCICs:

**I. Key Legislation and Regulations:**

*   **Immigration and Refugee Protection Act (IRPA):** The primary law governing immigration to Canada. It outlines the objectives, admissibility criteria, and procedures for various immigration streams.
*   **Immigration and Refugee Protection Regulations (IRPR):**  Detailed regulations that supplement the IRPA, providing specifics on how the Act is implemented. These regulations clarify eligibility requirements, processing procedures, and definitions.
*   **Citizenship Act:** Governs the acquisition, retention, and loss of Canadian citizenship.
*   **Citizenship Regulations:** Regulations that supplement the Citizenship Act.

**II. Core Principles:**

*   **Sovereign Right:** Canada, as a sovereign nation, has the right to determine who is allowed to enter and remain within its borders.
*   **Non-Discrimination:** While Canada can set criteria, it must not discriminate based on prohibited grounds under the *Canadian Charter of Rights and Freedoms*. However, distinctions are permitted based on legitimate immigration goals.
*   **Family Reunification:** A key objective of IRPA is to facilitate the reunification of families.
*   **Economic Benefit:**  Attracting immigrants who will contribute to Canada's economy is a central consideration.
*   **Refugee Protection:** Canada has international obligations to protect refugees.
*   **Fairness and Due Process:**  Immigration decisions must be made fairly and according to proper procedures.

**III. Immigration Categories:**

*   **Economic Class:**
    *   **Federal Skilled Worker Program (FSWP):**  For skilled workers meeting specific criteria (education, experience, language, etc.).
    *   **Canadian Experience Class (CEC):**  For individuals with Canadian work experience.
    *   **Federal Skilled Trades Program (FSTP):** For qualified tradespeople.
    *   **Provincial Nominee Programs (PNPs):**  Provinces nominate individuals who meet their specific economic needs.
    *   **Atlantic Immigration Program (AIP):** A pathway to permanent residence for skilled workers and international graduates from designated Atlantic institutions who want to work and live in Atlantic Canada.
    *   **Self-Employed Persons Program:** For individuals with relevant experience who intend and are able to be self-employed in Canada.
    *   **Start-Up Visa Program:** For entrepreneurs who can secure funding from a designated Canadian venture capital fund or angel investor group.
*   **Family Class:**
    *   **Spousal Sponsorship:**  Sponsorship of a spouse, common-law partner, or conjugal partner.
    *   **Parent and Grandparent Sponsorship:** Sponsorship of parents and grandparents.
    *   **Dependent Child Sponsorship:** Sponsorship of dependent children.
*   **Refugee Protection:**
    *   **Convention Refugees:** Individuals who fear persecution in their country of origin.
    *   **Persons in Need of Protection:** Individuals who face a risk of torture, cruel and unusual treatment, or punishment if returned to their country.
*   **Temporary Residence:**
    *   **Visitors:** Tourists, business visitors.
    *   **Students:** Individuals authorized to study in Canada.
    *   **Temporary Foreign Workers:** Workers authorized to work in Canada under various programs (e.g., LMIA-based, International Mobility Program).

**IV. Admissibility:**

*   **Criminality:** Past criminal convictions can render an applicant inadmissible.
*   **Security:** Concerns related to national security, terrorism, or organized crime.
*   **Medical:**  Medical conditions that could pose a danger to public health or safety, or cause excessive demand on health or social services.
*   **Misrepresentation:** Providing false information or withholding relevant information.
*   **Other Factors:** Financial reasons, family circumstances, etc.

**V. Procedural Fairness:**

*   **Right to be Heard:** Applicants must be given a reasonable opportunity to present their case.
*   **Right to Disclosure:** Applicants are generally entitled to know the information being used against them.
*   **Impartiality:** Decision-makers must be impartial and unbiased.
*   **Reasonable Decisions:** Decisions must be based on evidence and logical reasoning.

**VI. Judicial Review:**

*   **Grounds for Review:**  Decisions can be challenged in the Federal Court on grounds of procedural unfairness, errors of law, or unreasonable findings of fact.
*   **Standard of Review:** The level of deference the court gives to the decision-maker varies depending on the issue.
    *   **Correctness:** Used for questions of law, where the court determines the "correct" answer.
    *   **Reasonableness:** Used for questions of fact, policy, and mixed questions of fact and law. The court assesses whether the decision falls within a range of reasonable outcomes.

**VII. Sources of Information:**

*   **IRCC Website:** The Immigration, Refugees and Citizenship Canada (IRCC) website is the primary source for official information.
*   **Immigration and Refugee Board (IRB) Website:**  Information on refugee claims and appeals.
*   **Federal Court Website:**  Access to court decisions related to immigration matters.
*   **Case Law:** Review relevant case law to understand how the law has been interpreted by the courts.
*   **Policy Manuals:** IRCC publishes policy manuals (e.g., ENF, IP) that provide guidance to officers.

**VIII. Professional Ethics and Responsibilities (for RCICs):**

*   **College of Immigration and Citizenship Consultants (CICC):**  RCICs are regulated by the CICC and must adhere to their Code of Professional Conduct.
*   **Duty of Competence:**  RCICs must provide competent representation.
*   **Confidentiality:**  RCICs must maintain client confidentiality.
*   **Honesty and Integrity:**  RCICs must act honestly and with integrity.
*   **Avoiding Conflicts of Interest:**  RCICs must avoid conflicts of interest.

**IX. Key Considerations for RCICs:**

*   **Thorough Assessment:**  Carefully assess each client's situation to determine the most appropriate immigration pathway.
*   **Accurate Information:** Provide clients with accurate and up-to-date information.
*   **Complete Applications:**  Prepare complete and well-documented applications.
*   **Understanding of Procedures:**  Be familiar with IRCC processing procedures.
*   **Ethical Conduct:**  Adhere to the CICC Code of Professional Conduct.
*   **Continuing Education:**  Stay informed about changes in immigration law and policy.`;

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

  $effect(() => {
    if (files) {
      //   uploadFile();
      console.log("Caught files", files);
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
    if (!input.trim() && !files) return;
    scrollToBottom();

    loading = true;
    const userMessage = input;
    let displayMessage = userMessage;

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
      if (files) {
        const uploadedFile = await uploadFile();
        messageContent = await fullContent(
          uploadedFile,
          userMessage || "Please analyze this document"
        );
        // displayMessage += `[File: ${files[0].name}]`;
      } else {
        messageContent = createUserContent(userMessage);
      }

      // Add message to chat history
      messages = [
        ...messages,
        { role: "user", text: displayMessage, files: files },
      ];
      input = "";
      scrollToBottom();

      // Add initial empty message for streaming
      messages = [...messages, { role: "model", text: "", files: "" }];
      console.log("Message content", messageContent);
      const stream = await chat.sendMessageStream({
        message: messageContent,
      });

      let fullResponse = "";

      //   Handle streaming chunks
      for await (const chunk of stream) {
        // console.log("Chunk received:", chunk.text);
        fullResponse += chunk.text;
        messages = messages.map((msg, index) =>
          index === messages.length - 1 ? { ...msg, text: fullResponse } : msg
        );
        scrollToBottom();
      }
      //   messages = messages.map((msg, index) =>
      //     index === messages.length - 1 ? { ...msg, text: stream.text } : msg
      //   );
      //   scrollToBottom();
      console.log("Messages", fullResponse);
      console.log("HTML", renderMarkdown(fullResponse));

      // Clear the file after sending
      files = null;
    } catch (error) {
      console.error("Error:", error);
      messages = [
        ...messages,
        { role: "error", text: `Failed to get response: ${error.message}` },
      ];
      scrollToBottom();
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
<div class="grid grid-flow-col grid-rows-5 rounded-lg h-screen p-4">
  <!-- Messages area -->
  <div class="row-span-4 grid grid-cols-12 p-4">
    <div
      class="p-4 col-span-12 scroller overflow-y-auto space-y-4 scrollbar-thin scrollbar-thumb-teal-600 scrollbar-track-teal-950/50"
      bind:this={messagesContainer}
    >
      {#if messages.length > 0}
        {#each messages as msg}
          <div
            class="flex {msg.role === 'user' ? 'justify-end' : 'justify-start'}"
          >
            <div class="flex flex-col max-w-[70%]">
              <div
                class="markdown prose prose-invert prose-teal max-w-none rounded-full p-4 {msg.role ===
                'user'
                  ? 'bg-teal-700 text-gray-200'
                  : 'text-gray-200'}"
              >
                {@html renderMarkdown(msg.text)}

                <!-- <Markdown {carta} value={msg.text} /> -->

                <!-- {#key msg.text}
                  <Markdown {carta} value={msg.text} />
                {/key} -->
              </div>
              <div class="flex flex-row justify-end gap-2 my-1 pr-4">
                {#if msg.files}
                  <p class="text-gray-200 text-sm">Attachment</p>
                {/if}
              </div>
            </div>
          </div>
        {/each}

        {#if loading}
          <div class="flex justify-start">
            <div class="rounded-lg p-3">
              <p class="animate-pulse">Thinking...</p>
            </div>
          </div>
        {/if}
      {:else}
        <div class="flex h-full">
          <!-- <p class="text-gray-400 text-3xl font-semibold">
            Let's help you with your case
          </p> -->
          <p
            class="text-black markdown prose prose-invert prose-teal max-w-none"
          >
            {@html renderMarkdown(temp)}
          </p>
          <!-- 
          <Markdown {carta} value={temp} /> -->
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
      <Button
        onclick={() => {
          input =
            "Give me good breakdown of how to think about Canadian immigration law in general. You are not giving opinions just information";
        }}>Hello</Button
      >
      <Button onclick={sendMessage} disabled={loading} class="btn">Ask</Button>
      <Uploader bind:file={files} />

      <!-- <Input type="file" class="file-input border-4  border-teal-700" /> -->
    </div>
  </div>
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

  :global(.carta-font-code) {
    font-family: "...", monospace;
    font-size: 1.1rem;
    line-height: 1.1rem;
    letter-spacing: normal;
  }
</style>
