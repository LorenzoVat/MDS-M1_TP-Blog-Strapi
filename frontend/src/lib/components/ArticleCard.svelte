<script>
  export let article;
</script>

<article class="strapi-card">
  {#if article.coverImage}
    <img 
      src={`http://localhost:1338${article.coverImage.url}`}
      alt={article.title}
      class="card-image"
    />
  {:else}
    <div class="card-image-placeholder">
      <div class="placeholder-icon">📄</div>
      <div class="placeholder-gradient"></div>
    </div>
  {/if}
  
  <div class="card-content">
    <h3 class="card-title">{article.title}</h3>
    
    <div class="card-meta">
      <span class="meta-item">
        <svg class="meta-icon" viewBox="0 0 24 24" width="16" height="16">
          <path fill="currentColor" d="M12 2C6.5 2 2 6.5 2 12s4.5 10 10 10 10-4.5 10-10S17.5 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm.5-13H11v6l5.2 3.2.8-1.3-4.5-2.7V7z"/>
        </svg>
        {new Date(article.publishedAt).toLocaleDateString()}
      </span>
    </div>
    
    <p class="card-excerpt">
      {#each article.content.slice(0, 1) as block}
        {#each block.children.slice(0, 1) as child}
          {#if child.text.length > 100}
            {child.text.slice(0, 100)}...
          {:else}
            {child.text}
          {/if}
        {/each}
      {/each}
    </p>
    
    <a href={`/article/${article.slug}`} class="card-link">
      Lire l'article
      <svg class="link-arrow" viewBox="0 0 24 24" width="16" height="16">
        <path fill="currentColor" d="M5 13h11.17l-4.88 4.88c-.39.39-.39 1.03 0 1.42.39.39 1.02.39 1.41 0l6.59-6.59c.39-.39.39-1.02 0-1.41l-6.58-6.6c-.39-.39-1.02-.39-1.41 0-.39.39-.39 1.02 0 1.41L16.17 11H5c-.55 0-1 .45-1 1s.45 1 1 1z"/>
      </svg>
    </a>
  </div>
</article>

<style>
  .strapi-card {
    background: white;
    border-radius: 12px;
    box-shadow: 0 2px 16px rgba(73, 69, 255, 0.08);
    border: 1px solid #f6f5ff;
    overflow: hidden;
    transition: all 0.3s ease;
    height: 100%;
    display: flex;
    flex-direction: column;
  }

  .strapi-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 32px rgba(73, 69, 255, 0.12);
    border-color: #eae8ff;
  }

  .card-image {
    width: 100%;
    height: 200px;
    object-fit: cover;
  }

  .card-image-placeholder {
    width: 100%;
    height: 200px;
    background: linear-gradient(135deg, #8E75B8 0%, #4945ff 100%);
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
  }

  .placeholder-icon {
    font-size: 3rem;
    color: white;
    z-index: 2;
  }

  .placeholder-gradient::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: linear-gradient(45deg, transparent 30%, rgba(255,255,255,0.1) 50%, transparent 70%);
    animation: shimmer 2s infinite;
  }

  @keyframes shimmer {
    0% { transform: translateX(-100%); }
    100% { transform: translateX(100%); }
  }

  .card-content {
    padding: 1.5rem;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
  }

  .card-title {
    margin: 0 0 0.75rem 0;
    font-size: 1.25rem;
    color: #32324d;
    line-height: 1.4;
    font-weight: 600;
  }

  .card-meta {
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  .meta-item {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    color: #666687;
    font-size: 0.875rem;
  }

  .meta-icon {
    color: #8e8ea9;
  }

  .card-excerpt {
    color: #666687;
    line-height: 1.6;
    margin-bottom: 1.5rem;
    flex-grow: 1;
    font-size: 0.95rem;
  }

  .card-link {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    color: #4945ff;
    text-decoration: none;
    font-weight: 600;
    font-size: 0.9rem;
    transition: all 0.3s ease;
    align-self: flex-start;
    padding: 0.5rem 0;
  }

  .card-link:hover {
    color: #271fe0;
    gap: 0.75rem;
  }

  .link-arrow {
    transition: transform 0.3s ease;
  }

  .card-link:hover .link-arrow {
    transform: translateX(2px);
  }
</style>