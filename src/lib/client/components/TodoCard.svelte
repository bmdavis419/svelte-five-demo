<script lang="ts">
  import { slide, fade } from "svelte/transition";
  import { quintOut } from "svelte/easing";

  const { title, description, remove } = $props<{
    title: string;
    description: string;
    remove: () => void;
  }>();
</script>

<div
  class="p-4 w-full border-b border-gray-100 flex flex-row items-center justify-between"
  in:slide={{
    delay: 250,
    duration: 300,
    easing: quintOut,
  }}
  out:fade
>
  <div class="flex flex-col gap-y-2">
    <h3 class="text-2xl font-bold">{title}</h3>
    <p class="text-lg font-light">{description}</p>
  </div>

  <button
    on:click={remove}
    class="bg-gradient-to-tr from-orange-400 to-orange-600 font-bold text-lg px-4 py-2 rounded-full hover:bg-gradient-to-br"
    >Remove</button
  >
</div>
