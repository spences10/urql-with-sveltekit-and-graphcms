<script lang="ts" context="module">
  export const load = async ({ params }) => {
    const { slug } = params
    return { props: { slug } }
  }
</script>

<script lang="ts">
  export let slug
  import { gql, operationStore, query } from '@urql/svelte'
  const pageQuery = gql`
    query GetPage($slug: String!) {
      page(where: { slug: $slug }) {
        title
        content {
          html
        }
      }
    }
  `
  const page = operationStore(pageQuery, { slug })
  query(page)
</script>

<svelte:head>
  <title>URQL with GraphCMS | {$page.data?.page.title}</title>
</svelte:head>

{#if $page.fetching}
  <p>Loading...</p>
{:else if $page.error}
  <p>Oopsie! {$page.error.message}</p>
{:else}
  <h1 class="text-4xl font-semibold mb-5">
    {$page.data?.page.title}
  </h1>

  <article class="prose">
    {@html $page.data?.page.content.html}
  </article>
{/if}
