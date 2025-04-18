<script>
  let { text = "Thinking" } = $props();
</script>

<div
  class="flex w-fit items-center justify-center space-x-3 rounded-full p-4 bg-teal-700/50"
>
  <div class="flex space-x-2 gap-1 items-center">
    <div
      class="w-3 h-3 rounded-full bg-teal-500 animate-ping"
      style="animation-duration: 1.2s; animation-delay: 0s;"
    ></div>
    <div
      class="w-3 h-3 rounded-full bg-teal-400 animate-ping"
      style="animation-duration: 1.2s; animation-delay: 0.2s;"
    ></div>
    <div
      class="w-3 h-3 rounded-full bg-teal-300 animate-ping"
      style="animation-duration: 1.2s; animation-delay: 0.4s;"
    ></div>
    <div
      class="w-3 h-3 rounded-full bg-teal-200 animate-ping"
      style="animation-duration: 1.2s; animation-delay: 0.6s;"
    ></div>
    <div
      class="w-3 h-3 rounded-full bg-teal-100 animate-ping"
      style="animation-duration: 1.2s; animation-delay: 0.8s;"
    ></div>
    <span class="text-white font-medium animate-pulse">{text}</span>
  </div>
</div>
