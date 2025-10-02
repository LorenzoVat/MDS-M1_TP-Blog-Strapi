<script>
  export let comments = [];
</script>

<div class="strapi-comments">
  <h3 class="comments-title">
    <svg class="title-icon" viewBox="0 0 24 24" width="20" height="20">
      <path
        fill="currentColor"
        d="M20 2H4c-1.1 0-1.99.9-1.99 2L2 22l4-4h14c1.1 0 2-.9 2-2V4c0-1.1-.9-2-2-2zm-2 12H6v-2h12v2zm0-3H6V9h12v2zm0-3H6V6h12v2z"
      />
    </svg>
    Commentaires ({comments.length})
  </h3>

  {#if comments.length === 0}
    <div class="no-comments">
      <div class="no-comments-icon">💬</div>
      <p>Soyez le premier à commenter cet article !</p>
    </div>
  {:else}
    <div class="comments-list">
      {#each comments as comment}
        <article class="comment">
          <div class="comment-header">
            <div class="comment-author">
              <div class="author-avatar">
                {comment.authorName.charAt(0).toUpperCase()}
              </div>
              <div class="author-info">
                <strong class="author-name">{comment.authorName}</strong>
                <span class="comment-date">
                  {new Date(comment.publishedAt).toLocaleDateString("fr-FR", {
                    day: "numeric",
                    month: "long",
                    year: "numeric",
                    hour: "2-digit",
                    minute: "2-digit",
                  })}
                </span>
              </div>
            </div>
          </div>
          <div class="comment-content">
            {comment.content}
          </div>
        </article>
      {/each}
    </div>
  {/if}
</div>

<style>
  .strapi-comments {
    background: white;
    border-radius: 12px;
    padding: 2rem;
    border: 1px solid #f6f5ff;
  }

  .comments-title {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    color: #32324d;
    margin: 0 0 2rem 0;
    font-size: 1.5rem;
    font-weight: 600;
  }

  .title-icon {
    color: #4945ff;
  }

  .no-comments {
    text-align: center;
    padding: 3rem 2rem;
    background: #fafaff;
    border-radius: 8px;
    border: 2px dashed #eae8ff;
  }

  .no-comments-icon {
    font-size: 3rem;
    margin-bottom: 1rem;
    opacity: 0.5;
  }

  .no-comments p {
    color: #666687;
    margin: 0;
    font-size: 1.1rem;
  }

  .comments-list {
    display: flex;
    flex-direction: column;
    gap: 1.5rem;
  }

  .comment {
    background: #fafaff;
    border-radius: 12px;
    padding: 1.5rem;
    border: 1px solid #f0efff;
    transition: all 0.3s ease;
  }

  .comment:hover {
    border-color: #e0deff;
    transform: translateY(-1px);
  }

  .comment-header {
    margin-bottom: 1rem;
  }

  .comment-author {
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  .author-avatar {
    width: 40px;
    height: 40px;
    background: linear-gradient(135deg, #8e75b8 0%, #4945ff 100%);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    color: white;
    font-weight: 600;
    font-size: 0.9rem;
  }

  .author-info {
    display: flex;
    flex-direction: column;
    gap: 0.25rem;
  }

  .author-name {
    color: #32324d;
    font-size: 1rem;
  }

  .comment-date {
    color: #8e8ea9;
    font-size: 0.875rem;
  }

  .comment-content {
    color: #4a4a6a;
    line-height: 1.6;
    margin: 0;
    font-size: 1rem;
  }

  @media (max-width: 768px) {
    .strapi-comments {
      padding: 1.5rem;
    }

    .comment {
      padding: 1.25rem;
    }

    .comment-author {
      gap: 0.75rem;
    }

    .author-avatar {
      width: 36px;
      height: 36px;
      font-size: 0.8rem;
    }
  }
</style>