<script>
  import { Tooltip } from "bits-ui";

  let { children, text } = $props();
</script>

<Tooltip.Provider>
  <Tooltip.Root>
    <Tooltip.Trigger>
      {@render children()}
    </Tooltip.Trigger>
    <Tooltip.Portal>
      <Tooltip.Content
        class="bg-teal-900 px-4 py-2 text-white rounded-lg border border-teal-600 max-w-72"
      >
        <Tooltip.Arrow />
        {text}
      </Tooltip.Content>
    </Tooltip.Portal>
  </Tooltip.Root>
</Tooltip.Provider>
