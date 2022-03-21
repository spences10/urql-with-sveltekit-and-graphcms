<script lang="ts">
  import Footer from '$lib/components/footer.svelte'
  import Navbar from '$lib/components/navbar.svelte'
  import { initClient } from '@urql/svelte'
  import { onMount } from 'svelte'
  import { themeChange } from 'theme-change'
  import '../app.css'

  initClient({
    url: import.meta.env.VITE_GRAPHQL_API as string,
  })

  onMount(() => {
    themeChange(false)
  })
</script>

<Navbar />

<main class="container max-w-3xl mx-auto px-4">
  <slot />
</main>

<Footer />
