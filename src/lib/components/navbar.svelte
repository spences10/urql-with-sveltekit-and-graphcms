<script lang="ts">
  import { gql, operationStore, query } from '@urql/svelte'
  import ThemeSelect from './theme-select.svelte'

  const pagesQuery = gql`
    query AllPages {
      pages {
        title
        slug
        content {
          html
        }
      }
    }
  `
  const pages = operationStore(pagesQuery)
  query(pages)
</script>

<div class="navbar mb-10 shadow-lg bg-neutral text-neutral-content">
  <div class="px-2 mx-2 navbar-start">
    <a sveltekit:prefetch href="/">
      <span class="text-lg font-bold"> URQL with GraphCMS </span>
    </a>
  </div>
  <div class="px-2 mx-2 navbar-center">
    <div class="flex items-stretch">
      <a
        sveltekit:prefetch
        class="btn btn-ghost btn-sm rounded-btn"
        href={`/`}
      >
        Home
      </a>
      {#if $pages.fetching}
        <p class="btn btn-ghost btn-sm rounded-btn">Loading...</p>
      {:else if $pages.error}
        <p class="btn btn-ghost btn-sm rounded-btn">
          Oopsie! {$pages.error.message}
        </p>
      {:else}
        {#each $pages.data?.pages as { title, slug }}
          <a
            sveltekit:prefetch
            class="btn btn-ghost btn-sm rounded-btn"
            href={`/${slug}`}
          >
            {title}
          </a>
        {/each}
      {/if}
    </div>
  </div>
  <div class="navbar-end">
    <ThemeSelect />
  </div>
</div>
