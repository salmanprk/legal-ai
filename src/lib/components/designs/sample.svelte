<script>
  const features = [
    {
      icon: "🧠",
      title: "AI-Powered Legal Insight",
      description:
        "Instantly classify refusal reasoning as generic or substantive",
    },
    {
      icon: "📝",
      title: "Structured Recommendations",
      description:
        "Know when to file for Judicial Review, Reapply, or take a different route",
    },
    {
      icon: "📊",
      title: "Confidence Scoring",
      description:
        "Every suggestion is backed by rationale and confidence scores",
    },
    {
      icon: "📄",
      title: "Client-Ready Reports",
      description:
        "Download professional summaries ready to share with clients",
    },
    {
      icon: "⏱️",
      title: "Save Hours Per Case",
      description:
        "Skip manual analysis. Upload and review in under 60 seconds",
    },
    {
      icon: "💡",
      title: "Smart Analysis",
      description:
        "Get detailed insights into procedural fairness concerns and JR potential",
    },
  ];

  const steps = [
    {
      number: "1",
      title: "Upload Documents",
      description:
        "Simply upload your GCMS notes or Rule 9 refusal explanation",
    },
    {
      number: "2",
      title: "AI Analysis",
      description:
        "Our AI analyzes the content for generic vs substantive reasoning",
    },
    {
      number: "3",
      title: "Get Insights",
      description: "Receive detailed analysis and actionable recommendations",
    },
  ];
</script>

<!-- Hero Section -->
<div class="min-h-screen bg-gray-950">
  <!-- Navigation -->
  <nav class="p-6">
    <div class="container mx-auto flex justify-between items-center">
      <div class="text-2xl font-bold text-teal-500">LegalAI</div>
      <div class="hidden md:flex space-x-8">
        <a
          href="#features"
          class="text-gray-300 hover:text-teal-500 transition-colors"
          >Features</a
        >
        <a
          href="#how-it-works"
          class="text-gray-300 hover:text-teal-500 transition-colors"
          >How it Works</a
        >
        <a
          href="#testimonials"
          class="text-gray-300 hover:text-teal-500 transition-colors"
          >Testimonials</a
        >
      </div>
      <button
        class="bg-teal-600 text-white px-6 py-2 rounded-lg hover:bg-teal-700 transition-colors"
      >
        Get Started
      </button>
    </div>
  </nav>

  <!-- Hero Content -->
  <div class="container mx-auto px-6 py-24">
    <div class="grid md:grid-cols-2 gap-12 items-center">
      <div class="space-y-8">
        <h1 class="text-5xl font-bold text-white leading-tight">
          Your AI Legal Assistant for TRV Refusals
        </h1>
        <p class="text-xl text-gray-400">
          Upload your GCMS or Rule 9 notes. Let AI surface the key refusal
          reasons, flag generic decisions, and recommend your next legal move.
        </p>
        <div class="flex space-x-4">
          <button
            class="bg-teal-600 text-white px-8 py-4 rounded-lg text-lg font-semibold hover:bg-teal-700 transition-colors"
          >
            Try Sample Analysis
          </button>
          <button
            class="border border-teal-600 text-teal-500 px-8 py-4 rounded-lg text-lg font-semibold hover:bg-teal-950 transition-colors"
          >
            Learn More
          </button>
        </div>
      </div>
      <div class="relative">
        <!-- Placeholder for hero image or animation -->
        <div
          class="aspect-video bg-teal-900/20 rounded-2xl overflow-hidden border border-teal-800/30"
        >
          <!-- Add demo screenshot or animation here -->
        </div>
        <!-- Decorative elements -->
        <div
          class="absolute -z-10 top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[120%] h-[120%] bg-teal-500/10 blur-3xl rounded-full"
        ></div>
      </div>
    </div>
  </div>

  <!-- Features Section -->
  <section id="features" class="py-24 bg-gray-900/50">
    <div class="container mx-auto px-6">
      <div class="text-center mb-16">
        <h2 class="text-4xl font-bold text-white mb-4">Key Benefits</h2>
        <p class="text-xl text-gray-400">
          Turn opaque officer reasoning into actionable legal insight in seconds
        </p>
      </div>

      <div class="grid md:grid-cols-3 gap-8">
        {#each features as feature}
          <div
            class="p-8 rounded-xl bg-gray-900/50 border border-gray-800 hover:border-teal-800 transition-colors"
          >
            <div class="text-3xl text-teal-500 mb-4">{feature.icon}</div>
            <h3 class="text-xl font-bold text-white mb-2">{feature.title}</h3>
            <p class="text-gray-400">{feature.description}</p>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- How It Works Section -->
  <section id="how-it-works" class="py-24">
    <div class="container mx-auto px-6">
      <div class="text-center mb-16">
        <h2 class="text-4xl font-bold text-white mb-4">How It Works</h2>
        <p class="text-xl text-gray-400">
          Three simple steps to get actionable insights
        </p>
      </div>

      <div class="grid md:grid-cols-3 gap-8">
        {#each steps as step}
          <div class="relative">
            <div class="p-8 rounded-xl bg-gray-900/50 border border-gray-800">
              <div
                class="text-6xl font-bold text-teal-500/20 absolute top-4 right-4"
              >
                {step.number}
              </div>
              <h3 class="text-xl font-bold text-white mb-4">{step.title}</h3>
              <p class="text-gray-400">{step.description}</p>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </section>

  <!-- Social Proof Section -->
  <section id="testimonials" class="py-24 bg-gray-900/50">
    <div class="container mx-auto px-6">
      <div class="text-center mb-16">
        <h2 class="text-4xl font-bold text-white mb-4">What RCICs Say</h2>
        <p class="text-xl text-gray-400">
          Join hundreds of immigration consultants who trust our platform
        </p>
      </div>

      <div class="grid md:grid-cols-2 gap-8">
        <div class="p-8 rounded-xl bg-gray-900/50 border border-gray-800">
          <div class="flex items-start gap-4">
            <div class="w-12 h-12 rounded-full bg-teal-900/50"></div>
            <div>
              <p class="text-gray-300 mb-4">
                "Before this tool, I'd spend 30+ minutes reading through GCMS
                notes. Now I know in 60 seconds if it's worth going to court —
                and my clients love the clarity."
              </p>
              <p class="text-teal-500 font-semibold">A. Karim, RCIC</p>
            </div>
          </div>
        </div>
        <!-- Add more testimonials here -->
      </div>
    </div>
  </section>

  <!-- Final CTA Section -->
  <section class="py-24">
    <div class="container mx-auto px-6 text-center">
      <h2 class="text-4xl font-bold text-white mb-6">Ready to Get Started?</h2>
      <p class="text-xl text-gray-400 mb-8">
        Try our sample analysis or upload your first case today.
      </p>
      <div class="flex justify-center space-x-4">
        <button
          class="bg-teal-600 text-white px-8 py-4 rounded-lg text-lg font-semibold hover:bg-teal-700 transition-colors"
        >
          Try Sample Analysis
        </button>
        <button
          class="border border-teal-600 text-teal-500 px-8 py-4 rounded-lg text-lg font-semibold hover:bg-teal-950 transition-colors"
        >
          Contact Sales
        </button>
      </div>
    </div>
  </section>
</div>

<style>
  /* Add any custom styles here */
</style>
