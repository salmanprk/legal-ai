<script>
  // Animation on scroll library import would go here in a real implementation
  import { onMount } from "svelte";
  import Button from "./ui/button/button.svelte";
  import { goto } from "$app/navigation";

  // Reactive state for dark mode toggle
  let darkMode = false;

  // Toggle dark mode function
  function toggleDarkMode() {
    darkMode = !darkMode;
    if (darkMode) {
      document.documentElement.classList.add("dark");
    } else {
      document.documentElement.classList.remove("dark");
    }
  }

  // Features list
  const features = [
    {
      title: "AI-Powered Analysis",
      description:
        "Our advanced algorithms process immigration case documents in seconds, providing objective insights.",
      icon: "M3.75 13.5l10.5-11.25L12 10.5h8.25L9.75 21.75 12 13.5H3.75z",
      color: "bg-teal-100 text-teal-800 dark:bg-teal-900 dark:text-teal-300",
    },
    {
      title: "Instant GCMS Processing",
      description:
        "Upload GCMS notes and receive structured analysis with key insights highlighted.",
      icon: "M12 6v6h4.5m4.5 0a9 9 0 11-18 0 9 9 0 0118 0z",
      color:
        "bg-indigo-100 text-indigo-800 dark:bg-indigo-900 dark:text-indigo-300",
    },
    {
      title: "Judicial Review Support",
      description:
        "Get comprehensive case viability scores and detailed breakdowns for TRV refusals.",
      icon: "M12 3v17.25m0 0c-1.472 0-2.882.265-4.185.75M12 20.25c1.472 0 2.882.265 4.185.75M18.75 4.97A48.416 48.416 0 0012 4.5c-2.291 0-4.545.16-6.75.47m13.5 0c1.01.143 2.01.317 3 .52m-3-.52l2.62 10.726c.122.499-.106 1.028-.589 1.202a5.988 5.988 0 01-2.031.352 5.988 5.988 0 01-2.031-.352c-.483-.174-.711-.703-.59-1.202L18.75 4.971zm-16.5.52c.99-.203 1.99-.377 3-.52m0 0l2.62 10.726c.122.499-.106 1.028-.589 1.202a5.989 5.989 0 01-2.031.352 5.989 5.989 0 01-2.031-.352c-.483-.174-.711-.703-.59-1.202L5.25 4.971z",
      color:
        "bg-amber-100 text-amber-800 dark:bg-amber-900 dark:text-amber-300",
    },
    {
      title: "Immigration Expert Dashboard",
      description:
        "Designed specifically for RCICs with intuitive tools to enhance your professional practice.",
      icon: "M6.429 9.75L2.25 12l4.179 2.25m0-4.5l5.571 3 5.571-3m-11.142 0L2.25 7.5 12 2.25l9.75 5.25-4.179 2.25m0 0L21.75 12l-4.179 2.25m0 0l4.179 2.25L12 21.75 2.25 16.5l4.179-2.25m11.142 0l-5.571 3-5.571-3",
      color: "bg-rose-100 text-rose-800 dark:bg-rose-900 dark:text-rose-300",
    },
  ];

  // Testimonials
  const testimonials = [
    {
      quote:
        "This tool has revolutionized how we approach judicial reviews. What used to take hours now takes minutes.",
      author: "Sarah Chen",
      title: "Immigration Consultant, RCIC",
      image: "https://randomuser.me/api/portraits/women/1.jpg",
    },
    {
      quote:
        "The analysis provided gives me confidence when advising clients on their chances for judicial review.",
      author: "Michael Rodriguez",
      title: "Immigration Attorney",
      image: "https://randomuser.me/api/portraits/men/2.jpg",
    },
    {
      quote:
        "Case patterns I wouldn't have noticed are now instantly highlighted. It's like having an expert assistant.",
      author: "Priya Sharma",
      title: "RCIC, Former Visa Officer",
      image: "https://randomuser.me/api/portraits/women/3.jpg",
    },
  ];

  // Steps for the How It Works section
  const steps = [
    {
      number: "01",
      title: "Upload Documents",
      description:
        "Securely upload GCMS notes and case files through our encrypted platform.",
    },
    {
      number: "02",
      title: "AI Processing",
      description:
        "Our algorithms analyze the content, identifying patterns and key decision factors.",
    },
    {
      number: "03",
      title: "Review Insights",
      description:
        "Receive comprehensive analysis with viability scores and recommendations.",
    },
    {
      number: "04",
      title: "Take Action",
      description:
        "Make informed decisions on judicial review based on objective analysis.",
    },
  ];

  // Initialize on mount
  onMount(() => {
    // Check system preference for dark mode
    if (
      window.matchMedia &&
      window.matchMedia("(prefers-color-scheme: dark)").matches
    ) {
      darkMode = true;
      document.documentElement.classList.add("dark");
    }

    // Initialize animations or other DOM-dependent code here
  });
</script>

<div class="min-h-screen bg-teal-50 transition-colors duration-200">
  <!-- Navigation -->
  <!-- <nav
    class="sticky top-0 z-50 backdrop-blur-sm bg-white/80 dark:bg-gray-950/80 border-b border-gray-200 dark:border-gray-800"
  >
    <div class="container mx-auto flex justify-between items-center p-4">
      <div
        class="text-2xl font-bold text-teal-600 dark:text-teal-500 transition-colors font-ibm-plex-serif"
      >
        LegalAI
      </div>

      <div class="hidden md:flex space-x-8">
        <a
          href="#features"
          class="text-gray-700 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500 transition-colors"
          >Features</a
        >
        <a
          href="#how-it-works"
          class="text-gray-700 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500 transition-colors"
          >How it Works</a
        >
        <a
          href="#testimonials"
          class="text-gray-700 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500 transition-colors"
          >Testimonials</a
        >
        <a
          href="#pricing"
          class="text-gray-700 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500 transition-colors"
          >Pricing</a
        >
      </div>

      <div class="flex items-center space-x-4">
        <button
          on:click={toggleDarkMode}
          class="p-2 rounded-full text-gray-700 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-800 transition-colors"
          aria-label="Toggle dark mode"
        >
          {#if darkMode}
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-5 h-5"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M12 3v2.25m6.364.386l-1.591 1.591M21 12h-2.25m-.386 6.364l-1.591-1.591M12 18.75V21m-4.773-4.227l-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z"
              />
            </svg>
          {:else}
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-5 h-5"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M21.752 15.002A9.718 9.718 0 0118 15.75c-5.385 0-9.75-4.365-9.75-9.75 0-1.33.266-2.597.748-3.752A9.753 9.753 0 003 11.25C3 16.635 7.365 21 12.75 21a9.753 9.753 0 009.002-5.998z"
              />
            </svg>
          {/if}
        </button>

        <button
          class="bg-teal-600 dark:bg-teal-500 text-white px-6 py-2 rounded-lg hover:bg-teal-700 dark:hover:bg-teal-600 transition-colors shadow-sm font-medium"
        >
          Get Started
        </button>
      </div>
    </div>
  </nav> -->

  <!-- Hero Section -->
  <section class="relative overflow-hidden pt-16 md:pt-20 lg:pt-28">
    <div class="absolute inset-0 -z-10 overflow-hidden" aria-hidden="true">
      <svg
        class="absolute left-0 top-0 h-full w-full stroke-gray-200 dark:stroke-gray-800 [mask-image:radial-gradient(100%_100%_at_top_right,white,transparent)]"
      >
        <defs>
          <pattern
            id="precise-grid"
            width="40"
            height="40"
            patternUnits="userSpaceOnUse"
            ><path d="M.5 40V.5H40" fill="none" /></pattern
          >
        </defs>
        <rect
          width="100%"
          height="100%"
          stroke-width="0"
          fill="url(#precise-grid)"
        />
      </svg>
    </div>

    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <div class="grid lg:grid-cols-2 gap-12 lg:gap-16 items-center">
        <div class="text-center lg:text-left">
          <h1
            class="text-4xl font-extrabold tracking-tight text-gray-800 sm:text-5xl lg:text-6xl font-ibm-plex-serif"
          >
            <span class="block">Unlock Judicial Review Insights.</span>
            <span class=" text-teal-800 mt-1 sm:mt-2">
              Instantly Analyze TRV Refusals.
            </span>
          </h1>

          <p
            class="mt-6 text-lg text-gray-600 sm:text-xl max-w-2xl mx-auto lg:mx-0"
          >
            Our AI-powered platform processes GCMS notes and case documents to
            deliver rapid, objective analysis for Canadian immigration cases.
            Focus your expertise where it matters most.
          </p>

          <div
            class="mt-8 sm:mt-10 space-x-4 text-sm lg:text-base flex flex-col sm:flex-row items-center justify-center lg:justify-start space-y-4 sm:space-y-0"
          >
            <span class="inline-flex items-center text-teal-700">
              <svg
                class="w-5 h-5 mr-1.5 text-teal-600"
                fill="currentColor"
                viewBox="0 0 20 20"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  fill-rule="evenodd"
                  d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z"
                  clip-rule="evenodd"
                ></path>
              </svg>
              AI-Powered Precision
            </span>
            <span class="inline-flex items-center text-teal-700">
              <svg
                class="w-5 h-5 mr-1.5 text-teal-600"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M12 6v6h4.5m4.5 0a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z"
                />
              </svg>
              Results in Seconds
            </span>
          </div>

          <!-- Email Signup Form -->
          <div class="mt-10 sm:mt-12 max-w-lg mx-auto lg:mx-0">
            <!-- <form class="sm:flex"> -->
            <!-- <div class="flex-1">
                <label for="email-address" class="sr-only">Email address</label>
                <input
                  id="email-address"
                  name="email"
                  type="email"
                  autocomplete="email"
                  required
                  class="block w-full px-4 py-3 text-base text-gray-900 placeholder-gray-500 dark:placeholder-gray-400 bg-white border-2 border-teal-300 rounded-l-md shadow-sm focus:outline-none focus:ring-2 focus:ring-teal-500 focus:border-teal-500"
                  placeholder="Enter your professional email"
                />
              </div> -->
            <div class="flex space-x-4 mt-3 sm:mt-0">
              <Button
                onclick={() => {
                  goto("/app");
                }}
                size="lg"
                class="btn">Get a sample analysis</Button
              >
              <Button
                onclick={() => {
                  goto("/app");
                }}
                size="lg"
                class="btn-tertiary">Add your own documents</Button
              >
            </div>
            <!-- </form> -->
            <p class="mt-3 text-sm text-gray-500">
              Free 14-day trial. No credit card required.
            </p>
          </div>
        </div>

        <!-- Dashboard Preview -->
        <div class="relative lg:mt-0 mt-10">
          <div
            class="relative bg-white rounded-xl shadow-xl overflow-hidden border border-gray-200"
          >
            <div
              class="p-4 border-b border-gray-200 flex justify-between items-center bg-teal-800"
            >
              <div class="flex space-x-2">
                <div class="w-3 h-3 rounded-full bg-red-500"></div>
                <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
                <div class="w-3 h-3 rounded-full bg-green-500"></div>
              </div>
              <div class="text-sm font-medium text-gray-100">
                Case Analysis Dashboard
              </div>
              <div></div>
            </div>

            <div class="p-6 bg-white">
              <div class="flex justify-between items-center mb-6">
                <h3 class="text-lg font-bold text-gray-900">
                  Case Viability Analysis
                </h3>
                <div
                  class="bg-teal-100 text-teal-800 text-xs font-medium px-2.5 py-0.5 rounded-full"
                >
                  High Confidence
                </div>
              </div>

              <div class="grid grid-cols-2 gap-4 mb-6">
                <div class="bg-gray-50 p-4 rounded-lg">
                  <div class="text-sm text-gray-500">Viability Score</div>
                  <div class="text-2xl font-bold text-gray-900 mt-1">
                    82<span class="text-sm text-gray-500">/100</span>
                  </div>
                  <div class="w-full bg-gray-200 rounded-full h-2 mt-2">
                    <div
                      class="bg-teal-600 h-2 rounded-full"
                      style="width: 82%"
                    ></div>
                  </div>
                </div>
                <div class="bg-gray-50 p-4 rounded-lg">
                  <div class="text-sm text-gray-500">Issues Detected</div>
                  <div class="text-2xl font-bold text-gray-900 mt-1">3</div>
                  <div class="flex space-x-1 mt-2">
                    <span
                      class="px-2 py-1 text-xs rounded bg-red-100 text-red-800"
                      >Critical</span
                    >
                    <span
                      class="px-2 py-1 text-xs rounded bg-red-100 text-red-800"
                      >Critical</span
                    >
                    <span
                      class="px-2 py-1 text-xs rounded bg-yellow-100 text-yellow-800"
                      >Minor</span
                    >
                  </div>
                </div>
              </div>

              <div class="mb-6">
                <div class="flex justify-between items-center mb-2">
                  <h4 class="font-medium text-gray-900">Key Factors</h4>
                  <span class="text-xs text-gray-500">Impact</span>
                </div>

                <div class="space-y-2">
                  <div class="flex items-center justify-between">
                    <div class="flex items-center">
                      <svg
                        class="w-4 h-4 text-red-500 mr-2"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 20 20"
                        fill="currentColor"
                      >
                        <path
                          fill-rule="evenodd"
                          d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z"
                          clip-rule="evenodd"
                        />
                      </svg>
                      <span class="text-s"
                        >Inconsistent financial documentation</span
                      >
                    </div>
                    <div class="flex space-x-1">
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-600 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-600 rounded-sm"></div>
                    </div>
                  </div>

                  <div class="flex items-center justify-between">
                    <div class="flex items-center">
                      <svg
                        class="w-4 h-4 text-red-500 mr-2"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 20 20"
                        fill="currentColor"
                      >
                        <path
                          fill-rule="evenodd"
                          d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z"
                          clip-rule="evenodd"
                        />
                      </svg>
                      <span class="text-sm text-gray-700"
                        >Procedural fairness concerns</span
                      >
                    </div>
                    <div class="flex space-x-1">
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-600 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-600 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-600 rounded-sm"></div>
                    </div>
                  </div>

                  <div class="flex items-center justify-between">
                    <div class="flex items-center">
                      <svg
                        class="w-4 h-4 text-yellow-500 mr-2"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 20 20"
                        fill="currentColor"
                      >
                        <path
                          fill-rule="evenodd"
                          d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z"
                          clip-rule="evenodd"
                        />
                      </svg>
                      <span class="text-sm text-gray-700"
                        >Unclear travel history</span
                      >
                    </div>
                    <div class="flex space-x-1">
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-600 rounded-sm"></div>
                      <div class="w-2 h-4 bg-teal-200 rounded-sm"></div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="text-right">
                <button
                  class="text-sm font-medium text-teal-600 hover:text-teal-700 transition-colors"
                >
                  View Full Analysis →
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- Features Section -->
  <!-- <section id="features" class="py-16 md:py-24 bg-gray-50 dark:bg-gray-900">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <div class="max-w-3xl mx-auto text-center mb-16">
        <h2
          class="text-3xl font-bold tracking-tight text-gray-900 dark:text-white sm:text-4xl font-ibm-plex-serif"
        >
          Comprehensive Tools for Immigration Professionals
        </h2>
        <p class="mt-4 text-lg text-gray-600 dark:text-gray-300">
          Our platform provides the insights you need to make informed decisions
          on judicial review cases.
        </p>
      </div>

      <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
        {#each features as feature}
          <div
            class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700 hover:shadow-md transition-shadow"
          >
            <div
              class="w-12 h-12 {feature.color} rounded-lg flex items-center justify-center mb-4"
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
                  d={feature.icon}
                />
              </svg>
            </div>
            <h3
              class="text-xl font-bold text-gray-900 dark:text-white mb-2 font-ibm-plex"
            >
              {feature.title}
            </h3>
            <p class="text-gray-600 dark:text-gray-300">
              {feature.description}
            </p>
          </div>
        {/each}
      </div>
    </div>
  </section> -->

  <!-- How It Works Section -->
  <!-- <section id="how-it-works" class="py-16 md:py-24">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <div class="max-w-3xl mx-auto text-center mb-16">
        <h2
          class="text-3xl font-bold tracking-tight text-gray-900 dark:text-white sm:text-4xl font-ibm-plex-serif"
        >
          How It Works
        </h2>
        <p class="mt-4 text-lg text-gray-600 dark:text-gray-300">
          A simple process designed to fit seamlessly into your existing
          workflow.
        </p>
      </div>

      <div class="grid md:grid-cols-2 lg:grid-cols-4 gap-8">
        {#each steps as step}
          <div class="relative">
            <div
              class="flex items-center justify-center w-12 h-12 rounded-full bg-teal-100 dark:bg-teal-900 text-teal-600 dark:text-teal-300 font-bold text-lg mb-4 z-10"
            >
              {step.number}
            </div>

            <div
              class="hidden md:block absolute top-6 left-12 h-px bg-teal-200 dark:bg-teal-800 w-full"
              style="display: {step.number === '04' ? 'none' : 'block'}"
            ></div>

            <h3
              class="text-xl font-bold text-gray-900 dark:text-white mb-2 font-ibm-plex"
            >
              {step.title}
            </h3>
            <p class="text-gray-600 dark:text-gray-300">{step.description}</p>
          </div>
        {/each}
      </div>

      <div class="mt-16 text-center">
        <a
          href="#"
          class="inline-flex items-center justify-center px-8 py-3 text-base font-medium rounded-md text-white bg-teal-600 dark:bg-teal-500 hover:bg-teal-700 dark:hover:bg-teal-600 transition-colors"
        >
          Start Your Free Trial
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-5 h-5 ml-2"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M13.5 4.5L21 12m0 0l-7.5 7.5M21 12H3"
            />
          </svg>
        </a>
      </div>
    </div>
  </section> -->

  <!-- Testimonials -->
  <!-- <section id="testimonials" class="py-16 md:py-24 bg-gray-50 dark:bg-gray-900">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <div class="max-w-3xl mx-auto text-center mb-16">
        <h2
          class="text-3xl font-bold tracking-tight text-gray-900 dark:text-white sm:text-4xl font-ibm-plex-serif"
        >
          Trusted by Immigration Professionals
        </h2>
        <p class="mt-4 text-lg text-gray-600 dark:text-gray-300">
          See what RCICs and immigration attorneys are saying about our
          platform.
        </p>
      </div>

      <div class="grid md:grid-cols-3 gap-8">
        {#each testimonials as testimonial}
          <div
            class="bg-white dark:bg-gray-800 p-6 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700"
          >
            <div class="flex items-center mb-4">
              <div class="text-teal-600 dark:text-teal-500">
                <svg
                  class="h-5 w-5"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    d="M9.983 3v7.391c0 5.704-3.731 9.57-8.983 10.609l-.995-2.151c2.432-.917 3.995-3.638 3.995-5.849h-4v-10h9.983zm14.017 0v7.391c0 5.704-3.748 9.571-9 10.609l-.996-2.151c2.433-.917 3.996-3.638 3.996-5.849h-3.983v-10h9.983z"
                  />
                </svg>
              </div>
            </div>
            <p class="text-gray-600 dark:text-gray-300 mb-6 italic">
              "{testimonial.quote}"
            </p>
            <div class="flex items-center">
              <img
                class="h-10 w-10 rounded-full mr-3"
                src={testimonial.image}
                alt={testimonial.author}
              />
              <div>
                <h4 class="font-bold text-gray-900 dark:text-white">
                  {testimonial.author}
                </h4>
                <p class="text-sm text-gray-500 dark:text-gray-400">
                  {testimonial.title}
                </p>
              </div>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section> -->

  <!-- Pricing Section -->
  <!-- <section id="pricing" class="py-16 md:py-24">
    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <div class="max-w-3xl mx-auto text-center mb-16">
        <h2
          class="text-3xl font-bold tracking-tight text-gray-900 dark:text-white sm:text-4xl font-ibm-plex-serif"
        >
          Simple, Transparent Pricing
        </h2>
        <p class="mt-4 text-lg text-gray-600 dark:text-gray-300">
          Choose the plan that works best for your practice.
        </p>
      </div>

      <div class="grid lg:grid-cols-3 gap-8 max-w-5xl mx-auto">
       
        <div
          class="bg-white dark:bg-gray-800 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700 overflow-hidden"
        >
          <div class="p-6">
            <h3
              class="text-2xl font-bold text-gray-900 dark:text-white font-ibm-plex"
            >
              Basic
            </h3>
            <div class="mt-4 flex items-baseline">
              <span
                class="text-5xl font-extrabold tracking-tight text-gray-900 dark:text-white"
                >$99</span
              >
              <span
                class="ml-1 text-xl font-semibold text-gray-500 dark:text-gray-400"
                >/month</span
              >
            </div>
            <p class="mt-2 text-gray-500 dark:text-gray-400">
              For immigration consultants starting out.
            </p>
          </div>
          <div class="px-6 pt-6 pb-8">
            <ul class="space-y-4">
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Up to 10 case analyses per month</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Basic case analysis reports</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Email support</span
                >
              </li>
            </ul>
            <div class="mt-8">
              <a
                href="#"
                class="block w-full bg-teal-50 dark:bg-gray-700 text-teal-600 dark:text-teal-400 font-medium rounded-md px-4 py-2 text-center hover:bg-teal-100 dark:hover:bg-gray-600 transition-colors"
              >
                Start Basic Trial
              </a>
            </div>
          </div>
        </div>

        <div
          class="bg-white dark:bg-gray-800 rounded-lg shadow-lg border-2 border-teal-500 dark:border-teal-400 overflow-hidden lg:-mt-4 lg:-mb-4 relative z-10 transform lg:scale-105"
        >
          <div class="absolute top-0 inset-x-0">
            <div
              class="bg-teal-500 dark:bg-teal-400 text-white text-xs text-center font-bold uppercase tracking-wide py-1"
            >
              Most Popular
            </div>
          </div>
          <div class="p-6 mt-2">
            <h3
              class="text-2xl font-bold text-gray-900 dark:text-white font-ibm-plex"
            >
              Professional
            </h3>
            <div class="mt-4 flex items-baseline">
              <span
                class="text-5xl font-extrabold tracking-tight text-gray-900 dark:text-white"
                >$199</span
              >
              <span
                class="ml-1 text-xl font-semibold text-gray-500 dark:text-gray-400"
                >/month</span
              >
            </div>
            <p class="mt-2 text-gray-500 dark:text-gray-400">
              For established immigration practices.
            </p>
          </div>
          <div class="px-6 pt-6 pb-8">
            <ul class="space-y-4">
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Up to 30 case analyses per month</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Advanced case analysis reports</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Priority email & phone support</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Case similarity analysis</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >AI argument suggestions</span
                >
              </li>
            </ul>
            <div class="mt-8">
              <a
                href="#"
                class="block w-full bg-teal-600 dark:bg-teal-500 text-white font-medium rounded-md px-4 py-2 text-center hover:bg-teal-700 dark:hover:bg-teal-600 transition-colors"
              >
                Start Pro Trial
              </a>
            </div>
          </div>
        </div>

        <div
          class="bg-white dark:bg-gray-800 rounded-lg shadow-sm border border-gray-200 dark:border-gray-700 overflow-hidden"
        >
          <div class="p-6">
            <h3
              class="text-2xl font-bold text-gray-900 dark:text-white font-ibm-plex"
            >
              Enterprise
            </h3>
            <div class="mt-4 flex items-baseline">
              <span
                class="text-5xl font-extrabold tracking-tight text-gray-900 dark:text-white"
                >$399</span
              >
              <span
                class="ml-1 text-xl font-semibold text-gray-500 dark:text-gray-400"
                >/month</span
              >
            </div>
            <p class="mt-2 text-gray-500 dark:text-gray-400">
              For large firms with complex needs.
            </p>
          </div>
          <div class="px-6 pt-6 pb-8">
            <ul class="space-y-4">
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Unlimited case analyses</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Custom integration options</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Dedicated account manager</span
                >
              </li>
              <li class="flex items-start">
                <svg
                  class="flex-shrink-0 h-6 w-6 text-teal-500"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                  aria-hidden="true"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 13l4 4L19 7"
                  />
                </svg>
                <span class="ml-3 text-gray-600 dark:text-gray-300"
                  >Advanced analytics dashboard</span
                >
              </li>
            </ul>
            <div class="mt-8">
              <a
                href="#"
                class="block w-full bg-teal-50 dark:bg-gray-700 text-teal-600 dark:text-teal-400 font-medium rounded-md px-4 py-2 text-center hover:bg-teal-100 dark:hover:bg-gray-600 transition-colors"
              >
                Contact Sales
              </a>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section> -->

  <!-- CTA Section -->
  <!-- <section class="relative py-16 md:py-24 bg-teal-700 dark:bg-teal-800">
    <div class="absolute inset-0 -z-10 overflow-hidden" aria-hidden="true">
      <svg
        class="absolute right-0 top-0 h-full w-full stroke-teal-600 dark:stroke-teal-700 opacity-20 [mask-image:radial-gradient(100%_100%_at_top_left,white,transparent)]"
      >
        <defs>
          <pattern
            id="grid-pattern"
            width="40"
            height="40"
            patternUnits="userSpaceOnUse"
            ><path d="M.5 40V.5H40" fill="none" /></pattern
          >
        </defs>
        <rect
          width="100%"
          height="100%"
          stroke-width="0"
          fill="url(#grid-pattern)"
        />
      </svg>
    </div>

    <div class="container mx-auto px-4 sm:px-6 lg:px-8">
      <div class="max-w-4xl mx-auto text-center">
        <h2
          class="text-3xl font-bold tracking-tight text-white sm:text-4xl font-ibm-plex-serif"
        >
          Ready to transform your immigration practice?
        </h2>
        <p class="mt-4 text-lg text-teal-100">
          Join hundreds of immigration professionals who are saving time and
          improving outcomes with our AI platform.
        </p>
        <div class="mt-8 flex justify-center">
          <a
            href="#"
            class="inline-flex items-center justify-center px-8 py-3 border border-transparent text-base font-medium rounded-md text-teal-700 dark:text-teal-800 bg-white hover:bg-gray-100 transition-colors"
          >
            Start Your Free Trial
          </a>
          <a
            href="#"
            class="ml-4 inline-flex items-center justify-center px-8 py-3 border border-white text-base font-medium rounded-md text-white hover:bg-teal-600 dark:hover:bg-teal-700 transition-colors"
          >
            Schedule a Demo
          </a>
        </div>
      </div>
    </div>
  </section> -->

  <!-- Footer -->
  <!-- <footer
    class="bg-gray-50 dark:bg-gray-900 border-t border-gray-200 dark:border-gray-800"
  >
    <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-12">
      <div class="xl:grid xl:grid-cols-3 xl:gap-8">
        <div class="space-y-8 xl:col-span-1">
          <div
            class="text-2xl font-bold text-teal-600 dark:text-teal-500 font-ibm-plex-serif"
          >
            LegalAI
          </div>
          <p class="text-gray-600 dark:text-gray-400 max-w-xs">
            AI-powered tools designed specifically for Canadian immigration
            professionals.
          </p>
          <div class="flex space-x-6">
            <a
              href="#"
              class="text-gray-500 hover:text-gray-400"
              aria-label="Twitter"
            >
              <svg
                class="h-6 w-6"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  d="M8.29 20.251c7.547 0 11.675-6.253 11.675-11.675 0-.178 0-.355-.012-.53A8.348 8.348 0 0022 5.92a8.19 8.19 0 01-2.357.646 4.118 4.118 0 001.804-2.27 8.224 8.224 0 01-2.605.996 4.107 4.107 0 00-6.993 3.743 11.65 11.65 0 01-8.457-4.287 4.106 4.106 0 001.27 5.477A4.072 4.072 0 012.8 9.713v.052a4.105 4.105 0 003.292 4.022 4.095 4.095 0 01-1.853.07 4.108 4.108 0 003.834 2.85A8.233 8.233 0 012 18.407a11.616 11.616 0 006.29 1.84"
                />
              </svg>
            </a>
            <a
              href="#"
              class="text-gray-500 hover:text-gray-400"
              aria-label="LinkedIn"
            >
              <svg
                class="h-6 w-6"
                fill="currentColor"
                viewBox="0 0 24 24"
                aria-hidden="true"
              >
                <path
                  d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"
                />
              </svg>
            </a>
          </div>
        </div>
        <div class="mt-12 grid grid-cols-2 gap-8 xl:mt-0 xl:col-span-2">
          <div class="md:grid md:grid-cols-2 md:gap-8">
            <div>
              <h3
                class="text-sm font-semibold text-gray-500 dark:text-gray-400 tracking-wider uppercase"
              >
                Product
              </h3>
              <ul class="mt-4 space-y-4">
                <li>
                  <a
                    href="#features"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Features</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Case Studies</a
                  >
                </li>
                <li>
                  <a
                    href="#testimonials"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Testimonials</a
                  >
                </li>
                <li>
                  <a
                    href="#pricing"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Pricing</a
                  >
                </li>
              </ul>
            </div>
            <div class="mt-12 md:mt-0">
              <h3
                class="text-sm font-semibold text-gray-500 dark:text-gray-400 tracking-wider uppercase"
              >
                Support
              </h3>
              <ul class="mt-4 space-y-4">
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Documentation</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Guides</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >API Status</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Contact Us</a
                  >
                </li>
              </ul>
            </div>
          </div>
          <div class="md:grid md:grid-cols-2 md:gap-8">
            <div>
              <h3
                class="text-sm font-semibold text-gray-500 dark:text-gray-400 tracking-wider uppercase"
              >
                Company
              </h3>
              <ul class="mt-4 space-y-4">
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >About</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Blog</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Partners</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Careers</a
                  >
                </li>
              </ul>
            </div>
            <div class="mt-12 md:mt-0">
              <h3
                class="text-sm font-semibold text-gray-500 dark:text-gray-400 tracking-wider uppercase"
              >
                Legal
              </h3>
              <ul class="mt-4 space-y-4">
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Privacy</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Terms</a
                  >
                </li>
                <li>
                  <a
                    href="#"
                    class="text-base text-gray-600 dark:text-gray-300 hover:text-teal-600 dark:hover:text-teal-500"
                    >Cookie Policy</a
                  >
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
      <div class="mt-12 border-t border-gray-200 dark:border-gray-800 pt-8">
        <p class="text-base text-gray-500 dark:text-gray-400 text-center">
          &copy; 2023 LegalAI. All rights reserved.
        </p>
      </div>
    </div>
  </footer> -->
</div>
