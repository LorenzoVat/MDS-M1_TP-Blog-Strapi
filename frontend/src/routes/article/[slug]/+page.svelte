<script>
  import { onMount } from "svelte";
  import { page } from "$app/stores";

  let article = null;
  let loading = true;
  let error = null;

  onMount(async () => {
    try {
      const slug = $page.params.slug;
      const response = await fetch(
        `http://localhost:1338/api/articles?filters[slug][$eq]=${slug}`
      );

      if (!response.ok) throw new Error("Article non trouvé");

      const data = await response.json();
      article = data.data[0];
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  });
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

    <a href="/">← Retour à l'accueil</a>
  {:else}
    <p>Article non trouvé</p>
  {/if}
</div>
