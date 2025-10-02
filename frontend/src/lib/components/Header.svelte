<header class="strapi-header">
  <nav class="nav">
    <div class="nav-container">
      <a href="/" class="logo">
        <span class="logo-icon">⚡</span>
        <span class="logo-text">Strapi Blog</span>
      </a>
      <div class="nav-links">
        <!-- BOUTON "ARTICLES" SUPPRIMÉ -->
        <a href="https://strapi.io" target="_blank" class="nav-link external">
          Strapi.io ↗
        </a>
      </div>
    </div>
  </nav>
</header>

<style>
  .strapi-header {
    background: linear-gradient(135deg, #8e75b8 0%, #4945ff 100%);
    color: white;
    padding: 0;
    box-shadow: 0 4px 20px rgba(73, 69, 255, 0.15);
  }

  .nav {
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  }

  .nav-container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1.5rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 70px;
  }

  .logo {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    color: white;
    text-decoration: none;
    font-weight: 700;
    font-size: 1.25rem;
  }

  .logo-icon {
    font-size: 1.5rem;
    background: rgba(255, 255, 255, 0.1);
    padding: 0.5rem;
    border-radius: 8px;
  }

  .nav-links {
    display: flex;
    gap: 2rem;
    align-items: center;
  }

  .nav-link {
    color: rgba(255, 255, 255, 0.9);
    text-decoration: none;
    font-weight: 500;
    padding: 0.5rem 1rem;
    border-radius: 6px;
    transition: all 0.3s ease;
  }

  .nav-link:hover {
    color: white;
    background: rgba(255, 255, 255, 0.1);
  }

  .external {
    background: rgba(255, 255, 255, 0.1);
    border: 1px solid rgba(255, 255, 255, 0.2);
  }

  .external:hover {
    background: rgba(255, 255, 255, 0.2);
  }

  @media (max-width: 768px) {
    .nav-container {
      padding: 0 1rem;
    }

    .logo-text {
      display: none;
    }

    .nav-links {
      gap: 1rem;
    }

    .nav-link {
      padding: 0.5rem;
      font-size: 0.9rem;
    }
  }
</style>
