<script>
  // urql initialization
  import { getContextClient, gql, queryStore } from '@urql/svelte'

  const posts = queryStore({
    client: getContextClient(),
    query: gql`
      query Posts {
        posts {
          title
          slug
          date
          excerpt
          tags
          coverImage {
            url(
              transformation: {
                image: { resize: { fit: clip, width: 600 } }
              }
            )
          }
          content {
            html
          }
        }
      }
    `,
  })
</script>

{#if $posts.fetching}
  <p>Loading...</p>
{:else if $posts.error}
  <p>Oopsie! {$posts.error.message}</p>
{:else}
  <div class="prose mb-12">
    <h1>URQL with GraphCMS</h1>
    <p>
      An example project using the GraphCMS blog template and URQL for
      the GraphQL client
    </p>
  </div>
  {#each $posts.data?.posts as { title, slug, excerpt, coverImage, tags }}
    <div class="card text-center shadow-2xl mb-20">
      <figure class="">
        <img
          class=""
          src={coverImage.url}
          alt={`Cover image for ${title}`}
        />
      </figure>
      <div class="card-body prose">
        <h2 class="title">{title}</h2>
        <p>{excerpt}</p>
        <div class="flex justify-center mb-5 space-x-2">
          {#each tags as tag}
            <span class="badge badge-primary">{tag}</span>
          {/each}
        </div>
        <div class="justify-center card-actions">
          <a
            sveltekit:prefetch
            href={`/posts/${slug}`}
            class="btn btn-outline btn-primary">Read &rArr;</a
          >
        </div>
      </div>
    </div>
  {/each}
{/if}
