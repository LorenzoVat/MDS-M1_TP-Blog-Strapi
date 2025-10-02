<footer class="strapi-footer">
  <div class="footer-content">
    <div class="footer-section">
      <h4>Strapi Blog</h4>
      <p>Un blog moderne propulsé par Strapi Headless CMS</p>
    </div>
    <div class="footer-section">
      <h4>Liens</h4>
      <a href="/">Accueil</a>
      <a href="https://strapi.io" target="_blank">Strapi.io</a>
      <a href="http://localhost:1338/admin" target="_blank">Admin</a>
    </div>
    <div class="footer-section">
      <h4>Technos</h4>
      <span>Strapi v5</span>
      <span>SvelteKit</span>
      <span>REST API</span>
    </div>
  </div>
  <div class="footer-bottom">
    <p>&copy; 2025 Strapi Blog - TP CMS Headless</p>
  </div>
</footer>

<style>
  .strapi-footer {
    background: linear-gradient(135deg, #32324d 0%, #1c1c2e 100%);
    color: white;
    padding: 3rem 0 1rem;
    margin-top: 4rem;
  }

  .footer-content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 2rem;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 2rem;
  }

  .footer-section h4 {
    color: #8e75b8;
    margin-bottom: 1rem;
    font-size: 1.1rem;
  }

  .footer-section a {
    display: block;
    color: rgba(255, 255, 255, 0.8);
    text-decoration: none;
    margin-bottom: 0.5rem;
    transition: color 0.3s ease;
  }

  .footer-section a:hover {
    color: #4945ff;
  }

  .footer-section span {
    display: block;
    color: rgba(255, 255, 255, 0.6);
    margin-bottom: 0.5rem;
    font-size: 0.9rem;
  }

  .footer-bottom {
    max-width: 1200px;
    margin: 2rem auto 0;
    padding: 1rem 2rem 0;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    text-align: center;
    color: rgba(255, 255, 255, 0.5);
    font-size: 0.9rem;
  }

  @media (max-width: 768px) {
    .footer-content {
      grid-template-columns: 1fr;
      text-align: center;
      padding: 0 1rem;
    }
  }
</style>
