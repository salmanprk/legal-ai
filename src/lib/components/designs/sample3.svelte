<!-- 
  Hero Section Variant 2 for CaseDecide AI (Placeholder Name)
  - Layout inspired by Cal.com hero section
  - Uses TailwindCSS classes
  - Responsive (Mobile-first, stacks vertically)
  - Modern Tech / Business Professional Style
  - Teal color theme
  - Recommended Font: Inter (Add via CDN or Tailwind config)
-->
<section class="bg-gray-50 dark:bg-gray-900">
  <div
    class="mx-auto max-w-screen-xl px-4 py-16 sm:py-24 lg:py-32 grid lg:grid-cols-2 gap-8 lg:gap-16 items-center"
  >
    <!-- Text Content -->
    <div class="text-center lg:text-left">
      <!-- Optional Pill Badge -->
      <span
        class="inline-flex items-center px-3 py-0.5 rounded-full text-xs sm:text-sm font-medium bg-teal-100 text-teal-800 dark:bg-teal-900 dark:text-teal-300 mb-3"
      >
        AI Analysis Engine <span class="ml-1 font-semibold">v1.0 Soon</span>
      </span>

      <h1
        class="text-4xl font-extrabold tracking-tight text-gray-900 dark:text-white sm:text-5xl lg:text-6xl"
      >
        <span class="block">Judicial Review Viability,</span>
        <span class="block text-teal-700 dark:text-teal-500"
          >Analyzed with AI</span
        >
      </h1>
      <p
        class="mt-4 text-lg text-gray-600 dark:text-gray-400 sm:mt-6 sm:text-xl"
      >
        Empower your RCIC practice. Analyze GCMS notes and documents with AI to
        confidently advise clients on TRV refusal appeals.
      </p>

      <!-- Email Signup Form (Styled similar to Cal.com buttons) -->
      <div class="mt-8 sm:mt-10">
        <form action="#" method="POST" class="w-full max-w-md mx-auto lg:mx-0">
          <div class="sm:flex sm:gap-3">
            <label for="email-cal" class="sr-only">Email address</label>
            <input
              id="email-cal"
              name="email"
              type="email"
              autocomplete="email"
              required
              class="block w-full px-4 py-3 text-base text-gray-900 placeholder-gray-500 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-teal-500 focus:border-teal-500 dark:bg-gray-800 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-teal-500 dark:focus:border-teal-500 sm:flex-1"
              placeholder="Enter your email"
            />
            <button
              type="submit"
              class="mt-3 w-full sm:w-auto sm:mt-0 px-6 py-3 border border-transparent text-base font-medium rounded-md shadow-sm text-white bg-teal-700 hover:bg-teal-800 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-teal-600 dark:bg-teal-600 dark:hover:bg-teal-700 dark:focus:ring-teal-800 flex-shrink-0"
            >
              Notify Me
            </button>
          </div>
          <p class="mt-3 text-sm text-gray-500 dark:text-gray-400">
            Be the first to know. No spam, ever.
          </p>
        </form>
      </div>
    </div>

    <!-- Visual Placeholder (Styled like the Cal.com card) -->
    <div class="mt-10 lg:mt-0">
      <div
        class="bg-white dark:bg-gray-800 rounded-lg shadow-xl p-6 border border-gray-100 dark:border-gray-700"
      >
        <!-- Placeholder Content: Representing the app's analysis result -->
        <div class="flex items-center mb-4">
          <span
            class="inline-block p-2 bg-teal-100 dark:bg-teal-900 rounded-md mr-3"
          >
            <svg
              class="w-6 h-6 text-teal-700 dark:text-teal-400"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
              xmlns="http://www.w3.org/2000/svg"
              ><path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
              ></path></svg
            >
          </span>
          <div>
            <p class="text-sm font-medium text-gray-500 dark:text-gray-400">
              Case Analysis Result
            </p>
            <p class="text-lg font-semibold text-gray-900 dark:text-white">
              JR Recommended
            </p>
          </div>
        </div>
        <div class="space-y-3 text-sm text-gray-700 dark:text-gray-300">
          <p>
            <strong class="font-medium text-gray-900 dark:text-white"
              >Confidence Score:</strong
            > 85%
          </p>
          <p>
            <strong class="font-medium text-gray-900 dark:text-white"
              >Key Factors:</strong
            > Procedural Fairness Issue Detected, Rule 9 Compliance Issue.
          </p>
          <div
            class="h-20 bg-gray-100 dark:bg-gray-700 rounded-md mt-4 flex items-center justify-center text-gray-400 dark:text-gray-500 text-xs italic"
          >
            Simplified Visual Placeholder
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
