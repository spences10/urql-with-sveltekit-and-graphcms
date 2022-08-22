<script lang="ts">
  export let data
  let { slug } = data

  import { getContextClient, gql, queryStore } from '@urql/svelte'

  const page = queryStore({
    client: getContextClient(),
    variables: { slug },
    query: gql`
      query GetPage($slug: String!) {
        page(where: { slug: $slug }) {
          title
          content {
            html
          }
        }
      }
    `,
  })
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
