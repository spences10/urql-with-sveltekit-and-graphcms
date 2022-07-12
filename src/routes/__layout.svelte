<script lang="ts">
  import Footer from '$lib/components/footer.svelte'
  import Navbar from '$lib/components/navbar.svelte'
  import { createClient, setContextClient } from '@urql/svelte'
  import { onMount } from 'svelte'
  import { themeChange } from 'theme-change'
  import '../app.css'

  const client = createClient({
    url: import.meta.env.VITE_GRAPHQL_API.toString(),
    fetchOptions: () => {
      return {
        headers: {
          authorization: `bearer ${
            import.meta.env.VITE_SECRET_TOKEN
          }`,
        },
      }
    },
  })
  setContextClient(client)

  onMount(() => {
    themeChange(false)
  })
</script>

<Navbar />

<main class="container max-w-3xl mx-auto px-4">
  <slot />
</main>

<Footer />
