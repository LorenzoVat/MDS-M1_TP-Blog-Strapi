<script>
  import { onMount } from "svelte";
  import { page } from "$app/stores";

  let article = null;
  let comments = [];
  let loading = true;
  let error = null;

  onMount(async () => {
    try {
      const slug = $page.params.slug;
      const response = await fetch(
        `http://localhost:1338/api/articles?filters[slug][$eq]=${slug}&populate=coverImage`
      );

      if (!response.ok) throw new Error("Article non trouvé");

      const data = await response.json();
      article = data.data[0];

      const commentsResponse = await fetch(
        `http://localhost:1338/api/comments?filters[article][id][$eq]=${article.id}`
      );
      const commentsData = await commentsResponse.json();
      comments = commentsData.data || [];
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  });

  async function addComment(authorName, content) {
    try {
      const response = await fetch("http://localhost:1338/api/comments", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          data: {
            content: content,
            authorName: authorName,
            article: article.id,
          },
        }),
      });

      if (response.ok) {
        const commentsResponse = await fetch(
          `http://localhost:1338/api/comments?filters[article][id][$eq]=${article.id}`
        );
        const commentsData = await commentsResponse.json();
        comments = commentsData.data || [];
        return true;
      }
    } catch (err) {
      console.error("Erreur:", err);
    }
    return false;
  }
</script>

<svelte:head>
  <title>{article?.title || "Article"}</title>
</svelte:head>

<div>
  {#if loading}
    <p>Chargement...</p>
  {:else if error}
    <p style="color: red;">Erreur: {error}</p>
  {:else if article}
    <h1>{article.title}</h1>
    {#if article.coverImage}
      <img 
        src={`http://localhost:1338${article.coverImage.url}`}
        alt={article.title}
        style="max-width: 100%; height: auto; margin: 10px 0;"
      />
    {/if}
    <p>
      <strong>Publié le:</strong>
      {new Date(article.publishedAt).toLocaleDateString()}
    </p>

    <div>
      <strong>Contenu:</strong>
      {#each article.content as block}
        {#each block.children as child}
          <p>{child.text}</p>
        {/each}
      {/each}
    </div>

    <div style="margin-top: 2rem;">
      <h2>Commentaires ({comments.length})</h2>

      {#if comments.length === 0}
        <p>Aucun commentaire. Soyez le premier à commenter !</p>
      {:else}
        {#each comments as comment}
          <div
            style="border-left: 3px solid #ccc; padding-left: 1rem; margin: 1rem 0;"
          >
            <p>
              <strong>{comment.authorName}</strong> - {new Date(
                comment.publishedAt
              ).toLocaleDateString()}
            </p>
            <p>{comment.content}</p>
          </div>
        {/each}
      {/if}
    </div>

    <div style="margin-top: 2rem;">
      <h3>Ajouter un commentaire</h3>
      <form
        on:submit|preventDefault={async (e) => {
          const formData = new FormData(e.target);
          const authorName = formData.get("authorName");
          const content = formData.get("content");

          if (await addComment(authorName, content)) {
            e.target.reset();
          }
        }}
      >
        <div>
          <input
            type="text"
            name="authorName"
            placeholder="Votre nom"
            required
            style="display: block; margin: 0.5rem 0; padding: 0.5rem; width: 100%; max-width: 300px;"
          />
        </div>

        <div>
          <textarea
            name="content"
            placeholder="Votre commentaire"
            required
            rows="4"
            style="display: block; margin: 0.5rem 0; padding: 0.5rem; width: 100%; max-width: 500px;"
          ></textarea>
        </div>

        <button
          type="submit"
          style="padding: 0.5rem 1rem; background: #007acc; color: white; border: none; border-radius: 4px;"
        >
          Publier le commentaire
        </button>
      </form>
    </div>

    <a href="/">← Retour à l'accueil</a>
  {:else}
    <p>Article non trouvé</p>
  {/if}
</div>