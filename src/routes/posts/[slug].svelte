<script context="module">
  export const load = async ({ params }) => {
    const { slug } = params
    return { props: { slug } }
  }
</script>

<script>
  export let slug
  import { gql, operationStore, query } from '@urql/svelte'
  const postQuery = gql`
    query GetPost($slug: String!) {
      post(where: { slug: $slug }) {
        title
        excerpt
        date
        tags
        author {
          name
          authorTitle: title
          picture {
            url(
              transformation: {
                image: {
                  resize: { fit: clip, height: 50, width: 50 }
                }
              }
            )
          }
        }
        content {
          html
        }
        coverImage {
          url
        }
      }
    }
  `
  const post = operationStore(postQuery, { slug })
  query(post)
</script>

<svelte:head>
  <title>URQL with GraphCMS | {$post.data?.post.title}</title>
</svelte:head>

{#if $post.fetching}
  <p>Loading...</p>
{:else if $post.error}
  <p>Oopsie! {$post.error.message}</p>
{:else}
  <div class="sm:-mx-5 md:-mx-10 lg:-mx-20 xl:-mx-38 mb-5">
    <img
      src={$post.data?.post.coverImage.url}
      alt={`Cover image for ${$post.data?.post.title}`}
      class=""
    />
  </div>

  <h1 class="text-4xl font-semibold mb-5">
    {$post.data?.post.title}
  </h1>

  <a href="/" class="inline-flex items-center mb-3">
    <img
      src={$post.data?.post.author.picture.url}
      alt={$post.data?.post.name}
      class="w-12 h-12 rounded-full flex-shrink-0 object-cover object-center"
    />
    <span class="flex-grow flex flex-col pl-4">
      <span class="title-font font-medium">
        {$post.data?.post.author.name}
      </span>
      <span class="text-secondary text-xs tracking-widest mt-0.5">
        {$post.data?.post.author.authorTitle}
      </span>
    </span>
  </a>

  <p class="text-secondary text-xs tracking-widest font-semibold">
    {new Date($post.data?.post.date).toDateString()}
  </p>

  <div class="mb-5 flex justify-between">
    <div>
      {#if $post.data?.post.tags}
        <div class="mt-5 space-x-2">
          {#each $post.data?.post.tags as tag}
            <span class="badge badge-primary">{tag}</span>
          {/each}
        </div>
      {/if}
    </div>
  </div>

  <article class="prose mb-28">
    {@html $post.data?.post.content.html}
  </article>
{/if}
