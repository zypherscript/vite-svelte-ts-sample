<script lang="ts">
  import Counter from './lib/Counter.svelte'
  import Canvas, { type customRgb } from './lib/Canvas.svelte'
  import TailwindCss from './TailwindCSS.svelte'

  const rgb: customRgb = {r: 128, g: 128, b: 128};
</script>
<TailwindCss />

<main>
  <div class="container mx-auto">
    <Canvas mask="/src/assets/svelte-logo-mask.svg" rgb={rgb}/>
    <!-- <Canvas mask="/src/assets/svelte-logo-mask.svg"/> -->
  </div>
</main>
