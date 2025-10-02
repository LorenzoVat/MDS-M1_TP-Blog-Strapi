<script>
  import { onMount } from "svelte";

  let articles = [];
  let loading = true;
  let error = null;

  onMount(async () => {
    try {
      const API_URL = "http://localhost:1338/api/articles?populate=coverImage";

      const response = await fetch(API_URL);
      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }

      const data = await response.json();
      articles = data.data;
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  });
</script>

<svelte:head>
  <title>Mon Blog</title>
</svelte:head>

<div>
  <h1>Mon Blog - Liste des articles</h1>

  {#if loading}
    <p>Chargement en cours...</p>
  {:else if error}
    <p style="color: red;">Erreur: {error}</p>
  {:else if articles.length === 0}
    <p>Aucun article trouvé</p>
  {:else}
    <p><strong>{articles.length}</strong> article(s) trouvé(s)</p>

    {#each articles as article}
      <div style="border: 1px solid #ccc; margin: 10px; padding: 10px;">
        {#if article.coverImage}
          <img
            src={`http://localhost:1338${article.coverImage.url}`}
            alt={article.title}
            style="max-width: 300px; height: auto; margin-bottom: 10px;"
          />
        {/if}

        <h2>{article.title}</h2>
        <p><strong>ID:</strong> {article.id}</p>
        <p><strong>Slug:</strong> {article.slug}</p>
        <p>
          <strong>Publié le:</strong>
          {article.publishedAt
            ? new Date(article.publishedAt).toLocaleDateString()
            : "Brouillon"}
        </p>

        <div>
          <strong>Contenu:</strong>
          {#each article.content as block}
            {#each block.children as child}
              <p>{child.text}</p>
            {/each}
          {/each}
        </div>

        <a href={`/article/${article.slug}`}>Voir l'article</a>
      </div>
    {/each}
  {/if}

  <!-- Debug -->
  <details style="margin-top: 20px;">
    <summary>Debug complet</summary>
    <pre>{JSON.stringify(articles, null, 2)}</pre>
  </details>
</div>
