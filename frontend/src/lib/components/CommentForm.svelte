<script>
  export let onCommentSubmit;
  
  let submitting = false;
  
  async function handleSubmit(e) {
    e.preventDefault();
    submitting = true;
    
    const formData = new FormData(e.target);
    const authorName = formData.get('authorName');
    const content = formData.get('content');
    
    const success = await onCommentSubmit(authorName, content);
    
    if (success) {
      e.target.reset();
    }
    
    submitting = false;
  }
</script>

<div class="strapi-form">
  <h4 class="form-title">
    <svg class="form-icon" viewBox="0 0 24 24" width="20" height="20">
      <path fill="currentColor" d="M18 14v-2h-2v2h2zm0 4v-2h-2v2h2zm2-10h-2V4h2v4zm0 4h-2V8h2v4zm-6 8h-2v-4h2v4zm0 4h-2v-4h2v4zm-4 0H8v-4h2v4zm0 4H8v-4h2v4zM6 6H4v12h2V6z"/>
    </svg>
    Laisser un commentaire
  </h4>
  
  <form on:submit|preventDefault={handleSubmit} class="comment-form">
    <div class="form-group">
      <label for="authorName" class="form-label">Votre nom</label>
      <input
        type="text"
        id="authorName"
        name="authorName"
        placeholder="Ex: Jean Dupont"
        required
        class="form-input"
      />
    </div>
    
    <div class="form-group">
      <label for="content" class="form-label">Votre commentaire</label>
      <textarea
        id="content"
        name="content"
        placeholder="Partagez votre avis sur cet article..."
        required
        rows="5"
        class="form-textarea"
        disabled={submitting}
      ></textarea>
    </div>
    
    <button type="submit" disabled={submitting} class="submit-button">
      {#if submitting}
        <svg class="button-spinner" viewBox="0 0 24 24" width="16" height="16">
          <path fill="currentColor" d="M12,4V2A10,10 0 0,0 2,12H4A8,8 0 0,1 12,4Z"/>
        </svg>
        Publication en cours...
      {:else}
        <svg class="button-icon" viewBox="0 0 24 24" width="16" height="16">
          <path fill="currentColor" d="M2,21L23,12L2,3V10L17,12L2,14V21Z"/>
        </svg>
        Publier le commentaire
      {/if}
    </button>
  </form>
</div>

<style>
  .strapi-form {
    background: white;
    border-radius: 12px;
    padding: 2rem;
    border: 1px solid #f6f5ff;
    margin-top: 2rem;
  }

  .form-title {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    color: #32324d;
    margin: 0 0 1.5rem 0;
    font-size: 1.25rem;
    font-weight: 600;
  }

  .form-icon {
    color: #4945ff;
  }

  .comment-form {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  .form-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  .form-label {
    color: #32324d;
    font-weight: 500;
    font-size: 0.95rem;
  }

  .form-input,
  .form-textarea {
    padding: 0.875rem 1rem;
    border: 2px solid #e0deff;
    border-radius: 8px;
    font-size: 1rem;
    font-family: inherit;
    transition: all 0.3s ease;
    background: #fafaff;
  }

  .form-input:focus,
  .form-textarea:focus {
    outline: none;
    border-color: #4945ff;
    background: white;
    box-shadow: 0 0 0 3px rgba(73, 69, 255, 0.1);
  }

  .form-input:disabled,
  .form-textarea:disabled {
    opacity: 0.6;
    cursor: not-allowed;
  }

  .form-textarea {
    resize: vertical;
    min-height: 120px;
    line-height: 1.5;
  }

  .submit-button {
    display: inline-flex;
    align-items: center;
    gap: 0.75rem;
    background: linear-gradient(135deg, #8E75B8 0%, #4945ff 100%);
    color: white;
    border: none;
    padding: 1rem 2rem;
    border-radius: 8px;
    font-size: 1rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    align-self: flex-start;
  }

  .submit-button:hover:not(:disabled) {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(73, 69, 255, 0.3);
  }

  .submit-button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
    transform: none;
  }

  .button-spinner {
    animation: spin 1s linear infinite;
  }

  .button-icon {
    transition: transform 0.3s ease;
  }

  .submit-button:hover:not(:disabled) .button-icon {
    transform: translateX(2px);
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  @media (max-width: 768px) {
    .strapi-form {
      padding: 1.5rem;
    }

    .submit-button {
      align-self: stretch;
      justify-content: center;
    }
  }
</style>