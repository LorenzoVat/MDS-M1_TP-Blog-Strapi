<script>
  import { onMount } from "svelte";
  import Header from "$lib/components/Header.svelte";
  import ArticleCard from "$lib/components/ArticleCard.svelte";
  import Footer from "$lib/components/Footer.svelte";

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
  <title>Strapi Blog - Powered by Strapi CMS</title>
</svelte:head>

<Header />

<main class="strapi-main">
  <section class="hero-section">
    <div class="hero-content">
      <h1 class="hero-title">
        <span class="hero-gradient">Blog Strapi</span>
      </h1>
      <p class="hero-subtitle">
        Un blog moderne propulsé par Strapi Headless CMS
      </p>
      <div class="hero-stats">
        <div class="stat">
          <span class="stat-number">{articles.length}</span>
          <span class="stat-label">Articles</span>
        </div>
        <div class="stat">
          <span class="stat-number">100%</span>
          <span class="stat-label">Strapi</span>
        </div>
      </div>
    </div>
    <div class="hero-graphic">
      <div class="graphic-orb"></div>
    </div>
  </section>

  {#if loading}
    <section class="loading-section">
      <div class="loading-spinner"></div>
      <p>Chargement des articles...</p>
    </section>
  {:else if error}
    <section class="error-section">
      <div class="error-icon">⚠️</div>
      <h2>Erreur de connexion</h2>
      <p>{error}</p>
      <p class="error-help">Vérifiez que Strapi est démarré sur localhost:1338</p>
    </section>
  {:else if articles.length === 0}
    <section class="empty-section">
      <div class="empty-icon">📝</div>
      <h2>Aucun article publié</h2>
      <p>Créez votre premier article dans l'admin Strapi</p>
      <a href="http://localhost:1338/admin" target="_blank" class="strapi-button">
        Ouvrir Strapi Admin ↗
      </a>
    </section>
  {:else}
    <section class="articles-section">
      <div class="section-header">
        <h2>Derniers articles</h2>
        <p>Découvrez notre contenu récent</p>
      </div>
      <div class="articles-grid">
        {#each articles as article}
          <ArticleCard {article} />
        {/each}
      </div>
    </section>
    <Footer />
  {/if}
</main>

<style>
  .strapi-main {
    min-height: 100vh;
    background: linear-gradient(180deg, #f6f5ff 0%, #ffffff 100%);
  }

  .hero-section {
    max-width: 1200px;
    margin: 0 auto;
    padding: 4rem 2rem;
    display: grid;
    grid-template-columns: 1fr auto;
    gap: 4rem;
    align-items: center;
  }

  .hero-title {
    font-size: 3.5rem;
    font-weight: 800;
    margin-bottom: 1rem;
    line-height: 1.1;
  }

  .hero-gradient {
    background: linear-gradient(135deg, #8E75B8 0%, #4945ff 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .hero-subtitle {
    font-size: 1.25rem;
    color: #666687;
    margin-bottom: 2rem;
    max-width: 500px;
  }

  .hero-stats {
    display: flex;
    gap: 3rem;
  }

  .stat {
    text-align: center;
  }

  .stat-number {
    display: block;
    font-size: 2rem;
    font-weight: 700;
    color: #4945ff;
  }

  .stat-label {
    font-size: 0.875rem;
    color: #666687;
    text-transform: uppercase;
    letter-spacing: 0.5px;
  }

  .hero-graphic {
    position: relative;
  }

  .graphic-orb {
    width: 200px;
    height: 200px;
    background: linear-gradient(135deg, #8E75B8 0%, #4945ff 100%);
    border-radius: 50%;
    opacity: 0.1;
    animation: float 6s ease-in-out infinite;
  }

  @keyframes float {
    0%, 100% { transform: translateY(0px); }
    50% { transform: translateY(-20px); }
  }

  .loading-section, .error-section, .empty-section {
    max-width: 1200px;
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

  .error-icon, .empty-icon {
    font-size: 4rem;
    margin-bottom: 1rem;
  }

  .error-section h2, .empty-section h2 {
    color: #32324d;
    margin-bottom: 1rem;
  }

  .error-help {
    color: #666687;
    font-size: 0.9rem;
    margin-top: 1rem;
  }

  .strapi-button {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    background: #4945ff;
    color: white;
    padding: 0.75rem 1.5rem;
    border-radius: 6px;
    text-decoration: none;
    font-weight: 600;
    transition: all 0.3s ease;
    margin-top: 1rem;
  }

  .strapi-button:hover {
    background: #271fe0;
    transform: translateY(-1px);
  }

  .articles-section {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem 2rem 4rem;
  }

  .section-header {
    text-align: center;
    margin-bottom: 3rem;
  }

  .section-header h2 {
    font-size: 2.5rem;
    color: #32324d;
    margin-bottom: 0.5rem;
  }

  .section-header p {
    color: #666687;
    font-size: 1.125rem;
  }

  .articles-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
    gap: 2rem;
  }

  @media (max-width: 768px) {
    .hero-section {
      grid-template-columns: 1fr;
      text-align: center;
      padding: 2rem 1rem;
      gap: 2rem;
    }

    .hero-title {
      font-size: 2.5rem;
    }

    .hero-stats {
      justify-content: center;
    }

    .graphic-orb {
      width: 150px;
      height: 150px;
    }

    .articles-section {
      padding: 1rem;
    }

    .articles-grid {
      grid-template-columns: 1fr;
    }
  }
</style>