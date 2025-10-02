<script>
  import { onMount } from "svelte";
  import { page } from "$app/stores";
  import Header from "$lib/components/Header.svelte";
  import CommentList from "$lib/components/CommentList.svelte";
  import CommentForm from "$lib/components/CommentForm.svelte";
  import Footer from "$lib/components/Footer.svelte";

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
  <title>{article?.title || "Article"} - Strapi Blog</title>
</svelte:head>

<Header />

<main class="strapi-article-main">
  {#if loading}
    <div class="loading-container">
      <div class="loading-spinner"></div>
      <p>Chargement de l'article...</p>
    </div>
  {:else if error}
    <div class="error-container">
      <div class="error-icon">⚠️</div>
      <h2>Article non trouvé</h2>
      <p>{error}</p>
      <a href="/" class="back-home">← Retour à l'accueil</a>
    </div>
  {:else if article}
    <article class="article-detail">
      <header class="article-header">
        <div class="article-meta">
          <a href="/" class="back-link">
            <svg viewBox="0 0 24 24" width="16" height="16">
              <path
                fill="currentColor"
                d="M20 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H20v-2z"
              />
            </svg>
            Retour aux articles
          </a>
          <span class="publish-date">
            Publié le {new Date(article.publishedAt).toLocaleDateString(
              "fr-FR",
              {
                day: "numeric",
                month: "long",
                year: "numeric",
              }
            )}
          </span>
        </div>

        <h1 class="article-title">{article.title}</h1>

        {#if article.coverImage}
          <div class="cover-image-container">
            <img
              src={`http://localhost:1338${article.coverImage.url}`}
              alt={article.title}
              class="cover-image"
            />
          </div>
        {/if}
      </header>

      <div class="article-content">
        {#each article.content as block}
          {#each block.children as child}
            <p>{child.text}</p>
          {/each}
        {/each}
      </div>

      <footer class="article-footer">
        <div class="article-actions">
          <a href="/" class="action-button">
            <svg viewBox="0 0 24 24" width="16" height="16">
              <path
                fill="currentColor"
                d="M20 11H7.83l5.59-5.59L12 4l-8 8 8 8 1.41-1.41L7.83 13H20v-2z"
              />
            </svg>
            Retour aux articles
          </a>
          <button
            class="action-button secondary"
            on:click={() => {
              const commentsSection = document.querySelector(".strapi-form");
              if (commentsSection) {
                commentsSection.scrollIntoView({ behavior: "smooth" });
              }
            }}
          >
            <svg viewBox="0 0 24 24" width="16" height="16">
              <path
                fill="currentColor"
                d="M20 2H4c-1.1 0-1.99.9-1.99 2L2 22l4-4h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm-2 12H6v-2h12v2zm0-3H6V9h12v2zm0-3H6V6h12v2z"
              />
            </svg>
            Laisser un commentaire
          </button>
        </div>
      </footer>
    </article>

    <section class="comments-section">
      <CommentList {comments} />
      <CommentForm onCommentSubmit={addComment} />
    </section>
  {:else}
    <div class="not-found-container">
      <div class="not-found-icon">📭</div>
      <h2>Article non trouvé</h2>
      <p>L'article que vous recherchez n'existe pas ou a été supprimé.</p>
      <a href="/" class="back-home">← Retour à l'accueil</a>
    </div>
  {/if}
  <Footer />
</main>

<style>
  .strapi-article-main {
    min-height: 100vh;
    background: linear-gradient(180deg, #ffffff 0%, #fafaff 100%);
  }

  .loading-container,
  .error-container,
  .not-found-container {
    max-width: 800px;
    margin: 0 auto;
    padding: 4rem 2rem;
    text-align: center;
  }

  .loading-spinner {
    width: 60px;
    height: 60px;
    border: 4px solid #f6f5ff;
    border-top: 4px solid #4945ff;
    border-radius: 50%;
    margin: 0 auto 2rem;
    animation: spin 1s linear infinite;
  }

  .error-icon,
  .not-found-icon {
    font-size: 4rem;
    margin-bottom: 1rem;
    opacity: 0.7;
  }

  .error-container h2,
  .not-found-container h2 {
    color: #32324d;
    margin-bottom: 1rem;
  }

  .back-home {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    color: #4945ff;
    text-decoration: none;
    font-weight: 600;
    margin-top: 1.5rem;
    padding: 0.75rem 1.5rem;
    border: 2px solid #eae8ff;
    border-radius: 8px;
    transition: all 0.3s ease;
  }

  .back-home:hover {
    background: #fafaff;
    border-color: #4945ff;
  }

  .article-detail {
    max-width: 800px;
    margin: 0 auto;
    background: white;
    border-radius: 16px;
    box-shadow: 0 4px 24px rgba(73, 69, 255, 0.08);
    border: 1px solid #f0efff;
    overflow: hidden;
    margin-top: 2rem;
  }

  .article-header {
    padding: 3rem 3rem 2rem;
  }

  .article-meta {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 2rem;
    flex-wrap: wrap;
    gap: 1rem;
  }

  .back-link {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    color: #4945ff;
    text-decoration: none;
    font-weight: 500;
    padding: 0.5rem 1rem;
    border-radius: 6px;
    transition: all 0.3s ease;
  }

  .back-link:hover {
    background: #fafaff;
  }

  .publish-date {
    color: #666687;
    font-size: 0.95rem;
  }

  .article-title {
    font-size: 2.5rem;
    color: #32324d;
    margin: 0 0 2rem 0;
    line-height: 1.2;
    font-weight: 700;
  }

  .cover-image-container {
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 8px 32px rgba(73, 69, 255, 0.1);
  }

  .cover-image {
    width: 100%;
    height: 400px;
    object-fit: cover;
    display: block;
  }

  .article-content {
    padding: 0 3rem 2rem;
    line-height: 1.8;
    color: #4a4a6a;
    font-size: 1.125rem;
  }

  .article-content p {
    margin-bottom: 1.5rem;
  }

  .article-footer {
    padding: 2rem 3rem;
    border-top: 1px solid #f6f5ff;
    background: #fafaff;
  }

  .article-actions {
    display: flex;
    gap: 1rem;
    flex-wrap: wrap;
  }

  .action-button {
    display: inline-flex;
    align-items: center;
    gap: 0.75rem;
    padding: 0.875rem 1.5rem;
    border-radius: 8px;
    text-decoration: none;
    font-weight: 600;
    transition: all 0.3s ease;
    border: 2px solid #4945ff;
    background: #4945ff;
    color: white;
    cursor: pointer;
    font-size: 0.95rem;
  }

  .action-button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(73, 69, 255, 0.3);
  }

  .action-button.secondary {
    background: transparent;
    color: #4945ff;
  }

  .action-button.secondary:hover {
    background: #fafaff;
  }

  .comments-section {
    max-width: 800px;
    margin: 3rem auto 4rem;
    padding: 0 2rem;
  }

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }

  @media (max-width: 768px) {
    .article-header {
      padding: 2rem 1.5rem 1.5rem;
    }

    .article-title {
      font-size: 2rem;
    }

    .article-content {
      padding: 0 1.5rem 1.5rem;
      font-size: 1rem;
    }

    .article-footer {
      padding: 1.5rem;
    }

    .article-actions {
      flex-direction: column;
    }

    .action-button {
      justify-content: center;
    }

    .cover-image {
      height: 250px;
    }

    .comments-section {
      padding: 0 1rem;
      margin: 2rem auto 3rem;
    }

    .loading-container,
    .error-container,
    .not-found-container {
      padding: 2rem 1rem;
    }
  }
</style>
