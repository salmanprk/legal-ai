<script lang="ts">
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher<{
    selectPrompt: { prompt: string };
    uploadDocuments: void;
    startConversation: void;
  }>();

  // Sample use cases with templated prompts
  const useCases = [
    {
      title: "TRV Refusal Analysis",
      description:
        "Get a detailed breakdown of your Temporary Resident Visa refusal and chances for judicial review",
      icon: "M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z",
      prompt:
        "I need help analyzing my TRV refusal. Can you help me understand why it was refused and if I have grounds for judicial review?",
      color: "bg-teal-100 text-teal-800 dark:bg-teal-900 dark:text-teal-300",
    },
    {
      title: "GCMS Notes Interpretation",
      description:
        "Understand the key factors in your GCMS notes and how they impact your case",
      icon: "M19.5 14.25v-2.625a3.375 3.375 0 00-3.375-3.375h-1.5A1.125 1.125 0 0113.5 7.125v-1.5a3.375 3.375 0 00-3.375-3.375H8.25m0 12.75h7.5m-7.5 3H12M10.5 2.25H5.625c-.621 0-1.125.504-1.125 1.125v17.25c0 .621.504 1.125 1.125 1.125h12.75c.621 0 1.125-.504 1.125-1.125V11.25a9 9 0 00-9-9z",
      prompt:
        "I've received my GCMS notes. Can you analyze them and tell me what they mean for my immigration case?",
      color:
        "bg-indigo-100 text-indigo-800 dark:bg-indigo-900 dark:text-indigo-300",
    },
    {
      title: "Procedural Fairness Assessment",
      description:
        "Evaluate if procedural fairness issues exist in your case that could support judicial review",
      icon: "M12 3v17.25m0 0c-1.472 0-2.882.265-4.185.75M12 20.25c1.472 0 2.882.265 4.185.75M18.75 4.97A48.416 48.416 0 0012 4.5c-2.291 0-4.545.16-6.75.47m13.5 0c1.01.143 2.01.317 3 .52m-3-.52l2.62 10.726c.122.499-.106 1.028-.589 1.202a5.988 5.988 0 01-2.031.352 5.988 5.988 0 01-2.031-.352c-.483-.174-.711-.703-.59-1.202L18.75 4.971zm-16.5.52c.99-.203 1.99-.377 3-.52m0 0l2.62 10.726c.122.499-.106 1.028-.589 1.202a5.989 5.989 0 01-2.031.352 5.989 5.989 0 01-2.031-.352c-.483-.174-.711-.703-.59-1.202L5.25 4.971z",
      prompt:
        "I believe there were procedural fairness issues in my immigration case. Can you help me identify them and determine if I have grounds for judicial review?",
      color:
        "bg-amber-100 text-amber-800 dark:bg-amber-900 dark:text-amber-300",
    },
    {
      title: "Case Viability Score",
      description:
        "Get an objective assessment of your case's viability for judicial review",
      icon: "M3 13.125C3 12.504 3.504 12 4.125 12h2.25c.621 0 1.125.504 1.125 1.125v6.75C7.5 20.496 6.996 21 6.375 21h-2.25A1.125 1.125 0 013 19.875v-6.75zM9.75 8.625c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125v11.25c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 01-1.125-1.125V8.625zM16.5 4.125c0-.621.504-1.125 1.125-1.125h2.25c.621 0 1.125.504 1.125 1.125v15.75c0 .621-.504 1.125-1.125 1.125h-2.25a1.125 1.125 0 01-1.125-1.125V4.125z",
      prompt:
        "Can you analyze my case and provide a viability score for judicial review based on the information I provide?",
      color: "bg-rose-100 text-rose-800 dark:bg-rose-900 dark:text-rose-300",
    },
  ];

  // Features highlighting what the product can do
  const features = [
    {
      title: "Document Analysis",
      description:
        "Upload GCMS notes, refusal letters, and supporting documents for AI-powered analysis",
    },
    {
      title: "Case Assessment",
      description:
        "Get objective scoring of your case's viability for judicial review",
    },
    {
      title: "Legal Insights",
      description:
        "Receive explanations of key legal concepts relevant to your case",
    },
    {
      title: "Action Items",
      description:
        "Get clear recommendations on next steps for your immigration journey",
    },
  ];

  // Handler for clicking a templated prompt button
  function selectPrompt(prompt: string): void {
    dispatch("selectPrompt", { prompt });
  }

  // Handler for the upload documents button
  function uploadDocuments(): void {
    dispatch("uploadDocuments");
  }

  // Handler for the blank conversation button
  function startConversation(): void {
    dispatch("startConversation");
  }
</script>

<div class="w-full max-w-6xl mx-auto px-4 py-8">
  <!-- Hero Section -->
  <div class="text-center mb-16">
    <h1
      class="text-4xl font-extrabold tracking-tight text-gray-800 dark:text-white sm:text-5xl font-ibm-plex-serif mb-6"
    >
      Immigration Case Analysis <span class="text-teal-600 dark:text-teal-500"
        >Assistant</span
      >
    </h1>

    <p
      class="max-w-3xl mx-auto text-xl text-gray-600 dark:text-gray-300 leading-relaxed"
    >
      Our AI-powered platform helps immigration consultants and applicants
      analyze cases, interpret GCMS notes, and assess judicial review viability
      with confidence.
    </p>

    <div class="mt-8 flex flex-wrap justify-center gap-4">
      <button
        class="btn flex items-center justify-center gap-2"
        on:click={uploadDocuments}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M3 16.5v2.25A2.25 2.25 0 005.25 21h13.5A2.25 2.25 0 0021 18.75V16.5m-13.5-9L12 3m0 0l4.5 4.5M12 3v13.5"
          />
        </svg>
        Upload Documents
      </button>

      <!-- <button
        class="btn-tertiary flex items-center justify-center gap-2"
        on:click={startConversation}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M12 20.25c4.97 0 9-3.694 9-8.25s-4.03-8.25-9-8.25S3 7.444 3 12c0 2.104.859 4.023 2.273 5.48.432.447.74 1.04.586 1.641a4.483 4.483 0 01-.923 1.785A5.969 5.969 0 006 21c1.282 0 2.47-.402 3.445-1.087.81.22 1.668.337 2.555.337z"
          />
        </svg>
        Start New Conversation
      </button> -->
    </div>
  </div>

  <!-- How It Works Section -->
  <!-- <div class="mb-16">
    <h2
      class="text-3xl font-bold text-gray-800 dark:text-white text-center mb-12 font-ibm-plex-serif"
    >
      How It <span class="text-teal-600 dark:text-teal-500">Works</span>
    </h2>

    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
      <div
        class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700 text-center"
      >
        <div
          class="flex items-center justify-center w-12 h-12 rounded-full bg-teal-100 dark:bg-teal-900 text-teal-600 dark:text-teal-300 font-bold text-lg mb-4 mx-auto"
        >
          1
        </div>
        <h3
          class="text-xl font-bold text-gray-900 dark:text-white mb-2 font-ibm-plex"
        >
          Upload Documents
        </h3>
        <p class="text-gray-600 dark:text-gray-400">
          Upload your GCMS notes, refusal letters, and other case documents.
        </p>
      </div>

      <div
        class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700 text-center"
      >
        <div
          class="flex items-center justify-center w-12 h-12 rounded-full bg-teal-100 dark:bg-teal-900 text-teal-600 dark:text-teal-300 font-bold text-lg mb-4 mx-auto"
        >
          2
        </div>
        <h3
          class="text-xl font-bold text-gray-900 dark:text-white mb-2 font-ibm-plex"
        >
          Ask Questions
        </h3>
        <p class="text-gray-600 dark:text-gray-400">
          Ask specific questions about your case or use our templated prompts.
        </p>
      </div>

      <div
        class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700 text-center"
      >
        <div
          class="flex items-center justify-center w-12 h-12 rounded-full bg-teal-100 dark:bg-teal-900 text-teal-600 dark:text-teal-300 font-bold text-lg mb-4 mx-auto"
        >
          3
        </div>
        <h3
          class="text-xl font-bold text-gray-900 dark:text-white mb-2 font-ibm-plex"
        >
          Review Analysis
        </h3>
        <p class="text-gray-600 dark:text-gray-400">
          Get detailed analysis with key insights, legal references, and case
          viability.
        </p>
      </div>

      <div
        class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700 text-center"
      >
        <div
          class="flex items-center justify-center w-12 h-12 rounded-full bg-teal-100 dark:bg-teal-900 text-teal-600 dark:text-teal-300 font-bold text-lg mb-4 mx-auto"
        >
          4
        </div>
        <h3
          class="text-xl font-bold text-gray-900 dark:text-white mb-2 font-ibm-plex"
        >
          Take Action
        </h3>
        <p class="text-gray-600 dark:text-gray-400">
          Make informed decisions based on objective assessment and
          recommendations.
        </p>
      </div>
    </div>
  </div> -->

  <!-- Features Grid -->
  <!-- <div class="mb-16">
    <h2
      class="text-3xl font-bold text-gray-800 dark:text-white text-center mb-8 font-ibm-plex-serif"
    >
      What Our Assistant Can <span class="text-teal-600 dark:text-teal-500"
        >Do</span
      >
    </h2>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      {#each features as feature}
        <div
          class="bg-white dark:bg-gray-800 p-5 rounded-lg border border-gray-200 dark:border-gray-700 flex items-start"
        >
          <div class="mr-4 mt-1 text-teal-600 dark:text-teal-500">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-6 h-6"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
          </div>
          <div>
            <h3
              class="text-lg font-bold text-gray-900 dark:text-white mb-1 font-ibm-plex"
            >
              {feature.title}
            </h3>
            <p class="text-gray-600 dark:text-gray-400">
              {feature.description}
            </p>
          </div>
        </div>
      {/each}
    </div>
  </div> -->

  <!-- Sample Prompts Section -->
  <div>
    <section class="flex gap-2 my-4">
      <div
        class="flex w-fit items-center justify-center space-x-3 rounded-full p-4 bg-gray-700/50"
      >
        <div class="flex space-x-2 gap-1 items-center">
          <div
            class="w-3 h-3 rounded-full bg-orange-400 animate-ping"
            style="animation-duration: 1.2s; animation-delay: 0s;"
          ></div>
          <div
            class="w-3 h-3 rounded-full bg-orange-300 animate-ping"
            style="animation-duration: 1.2s; animation-delay: 0.2s;"
          ></div>
          <div
            class="w-3 h-3 rounded-full bg-orange-200 animate-ping"
            style="animation-duration: 1.2s; animation-delay: 0.4s;"
          ></div>
          <span
            class="text-white font-medium animate-pulse"
            style="animation-duration: 1.2s; animation-delay: 0.6s;"
            >Thinking</span
          >
        </div>
      </div>
    </section>
    <h2
      class="text-3xl font-bold text-gray-800 dark:text-white text-center mb-8 font-ibm-plex-serif"
    >
      Get Started with These <span class="text-teal-600 dark:text-teal-500"
        >Prompts</span
      >
    </h2>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      {#each useCases as useCase}
        <button
          class="bg-teal-800 p-6 rounded-lg shadow-sm border border-teal-200 hover:shadow-md transition-shadow text-left flex flex-col h-full"
          on:click={() => selectPrompt(useCase.prompt)}
        >
          <div class="flex items-start mb-4">
            <!-- <div
              class="w-12 h-12 {useCase.color} rounded-lg flex items-center justify-center mr-4 flex-shrink-0"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="w-6 h-6"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d={useCase.icon}
                />
              </svg>
            </div> -->
            <h3 class="text-xl font-bold text-gray-100 font-ibm-plex">
              {useCase.title}
            </h3>
          </div>
          <p class="text-gray-200 mb-4 flex-grow">
            {useCase.description}
          </p>
          <div class="text-teal-100 text-sm font-medium flex items-center">
            Try this prompt
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-4 h-4 ml-1"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3"
              />
            </svg>
          </div>
        </button>
      {/each}
    </div>
  </div>

  <!-- Document Upload Reminder -->
  <!-- <div
    class="mt-16 bg-teal-50 dark:bg-teal-900/30 border border-teal-200 dark:border-teal-800 rounded-lg p-6 text-center"
  >
    <div
      class="flex flex-col md:flex-row items-center justify-center md:justify-between"
    >
      <div class="mb-4 md:mb-0 text-left md:mr-6">
        <h3
          class="text-xl font-bold text-gray-900 dark:text-white mb-2 font-ibm-plex"
        >
          Ready to analyze your case?
        </h3>
        <p class="text-gray-600 dark:text-gray-400">
          Upload your documents for a comprehensive analysis of your immigration
          case.
        </p>
      </div>
      <button
        class="btn-secondary flex items-center justify-center gap-2 whitespace-nowrap"
        on:click={uploadDocuments}
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
          stroke-width="1.5"
          stroke="currentColor"
          class="w-6 h-6"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            d="M3 16.5v2.25A2.25 2.25 0 005.25 21h13.5A2.25 2.25 0 0021 18.75V16.5m-13.5-9L12 3m0 0l4.5 4.5M12 3v13.5"
          />
        </svg>
        Upload Your Documents
      </button>
    </div>
  </div> -->
</div>
