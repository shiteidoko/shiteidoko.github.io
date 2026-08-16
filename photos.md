---
layout: page
title: Photos
permalink: /photos/
---

<style>
  /* Photo Grid Layout */
  .photo-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
    gap: 16px;
    margin-top: 1.5rem;
  }

  .year-section {
    margin-bottom: 2.5rem;
  }

  .year-title {
    font-size: 1.5rem;
    font-weight: 400;
    letter-spacing: -1px;
    color: var(--text-color, #111111);
    border-bottom: 1px solid var(--border-color, #e8e8e8);
    padding-bottom: 6px;
    margin-bottom: 1.25rem;
  }

  /* Card Styling Aligned with Minima */
  .photo-card {
    display: block;
    background: #ffffff;
    border: 1px solid var(--border-color, #e8e8e8);
    border-radius: 4px;
    overflow: hidden;
    text-decoration: none !important;
    transition: border-color 0.2s ease, box-shadow 0.2s ease;
  }

  .photo-card:hover {
    border-color: var(--brand-color, #2a7ae2);
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
  }

  .photo-card img {
    width: 100%;
    height: 180px;
    object-fit: cover;
    display: block;
  }

  .photo-caption {
    padding: 10px 12px;
    font-size: 0.875rem;
    color: var(--text-color, #111111);
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    text-transform: capitalize;
  }

  /* Custom Native Modal Styles */
  .custom-modal {
    border: none;
    padding: 0;
    background: transparent;
    max-width: 100vw;
    max-height: 100vh;
    width: 100vw;
    height: 100vh;
    overflow: hidden;
  }

  .custom-modal::backdrop {
    background: rgba(0, 0, 0, 0.9);
  }

  .modal-wrapper {
    position: relative;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  .modal-img-container {
    max-width: 90vw;
    max-height: 80vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .modal-img-container img {
    max-width: 100%;
    max-height: 80vh;
    object-fit: contain;
    border-radius: 4px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.5);
  }

  .modal-caption {
    color: #ffffff;
    margin-top: 1rem;
    font-size: 1rem;
    text-align: center;
    text-transform: capitalize;
  }

  /* Control Buttons */
  .modal-btn {
    position: absolute;
    background: rgba(255, 255, 255, 0.15);
    color: #ffffff;
    border: none;
    border-radius: 50%;
    width: 48px;
    height: 48px;
    font-size: 24px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    user-select: none;
    transition: background 0.2s ease;
    z-index: 10;
  }

  .modal-btn:hover {
    background: rgba(255, 255, 255, 0.35);
  }

  .modal-close { top: 20px; right: 20px; }
  .modal-prev { left: 20px; top: 50%; transform: translateY(-50%); }
  .modal-next { right: 20px; top: 50%; transform: translateY(-50%); }
</style>

<!-- Photos Markup -->
{% assign all_photos = site.static_files | where_exp: "item", "item.path contains '/assets/photos/'" %}

{% assign years = "" | split: "" %}
{% for file in all_photos %}
  {% assign parts = file.path | split: "/" %}
  {% assign year = parts[3] %}
  {% unless years contains year %}
    {% assign years = years | push: year %}
  {% endunless %}
{% endfor %}

{% assign sorted_years = years | sort | reverse %}

{% for year in sorted_years %}
  <section class="year-section">
    <h2 class="year-title">{{ year }}</h2>
    
    <div class="photo-grid">
      {% for file in all_photos %}
        {% assign parts = file.path | split: "/" %}
        {% assign file_year = parts[3] %}
        
        {% if file_year == year %}
          {% assign filename = file.name | split: "." | first %}
          {% assign caption = filename | replace: "-", " " | replace: "_", " " %}
          
          <a href="{{ file.path | relative_url }}" 
             class="photo-card gallery-item" 
             data-group="gallery-{{ year }}"
             data-caption="{{ caption }}">
            
            <img src="{{ file.path | relative_url }}" alt="{{ caption }}" loading="lazy">
            <div class="photo-caption">{{ caption }}</div>
          </a>
        {% endif %}
      {% endfor %}
    </div>
  </section>
{% endfor %}

<!-- HTML5 Native Modal -->
<dialog id="photoModal" class="custom-modal">
  <div class="modal-wrapper">
    <button class="modal-btn modal-close" id="modalClose" aria-label="Close">&times;</button>
    <button class="modal-btn modal-prev" id="modalPrev" aria-label="Previous">&#10094;</button>
    <button class="modal-btn modal-next" id="modalNext" aria-label="Next">&#10095;</button>
    
    <div class="modal-img-container">
      <img id="modalImage" src="" alt="">
    </div>
    <div id="modalCaption" class="modal-caption"></div>
  </div>
</dialog>

<!-- Vanilla JS Gallery Logic -->
<script>
  document.addEventListener('DOMContentLoaded', () => {
    const modal = document.getElementById('photoModal');
    const modalImg = document.getElementById('modalImage');
    const modalCaption = document.getElementById('modalCaption');
    const closeBtn = document.getElementById('modalClose');
    const prevBtn = document.getElementById('modalPrev');
    const nextBtn = document.getElementById('modalNext');

    let currentGroupItems = [];
    let currentIndex = 0;

    // Attach click event to all photo links
    document.querySelectorAll('.gallery-item').forEach(item => {
      item.addEventListener('click', (e) => {
        e.preventDefault();
        
        const group = item.getAttribute('data-group');
        currentGroupItems = Array.from(document.querySelectorAll(`.gallery-item[data-group="${group}"]`));
        currentIndex = currentGroupItems.indexOf(item);
        
        updateModal();
        modal.showModal();
      });
    });

    function updateModal() {
      const activeItem = currentGroupItems[currentIndex];
      modalImg.src = activeItem.getAttribute('href');
      modalCaption.textContent = activeItem.getAttribute('data-caption');

      // Toggle navigation buttons if group has only 1 image
      const hasMultiple = currentGroupItems.length > 1;
      prevBtn.style.display = hasMultiple ? 'flex' : 'none';
      nextBtn.style.display = hasMultiple ? 'flex' : 'none';
    }

    function showNext() {
      if (currentGroupItems.length <= 1) return;
      currentIndex = (currentIndex + 1) % currentGroupItems.length;
      updateModal();
    }

    function showPrev() {
      if (currentGroupItems.length <= 1) return;
      currentIndex = (currentIndex - 1 + currentGroupItems.length) % currentGroupItems.length;
      updateModal();
    }

    // Event Listeners
    nextBtn.addEventListener('click', showNext);
    prevBtn.addEventListener('click', showPrev);
    closeBtn.addEventListener('click', () => modal.close());

    // Close on backdrop click
    modal.addEventListener('click', (e) => {
      if (e.target === modal || e.target.classList.contains('modal-wrapper')) {
        modal.close();
      }
    });

    // Keyboard Navigation
    document.addEventListener('keydown', (e) => {
      if (!modal.open) return;
      if (e.key === 'ArrowRight') showNext();
      if (e.key === 'ArrowLeft') showPrev();
    });

    // Touch Swipe Support for Mobile
    let touchStartX = 0;
    modal.addEventListener('touchstart', e => { touchStartX = e.changedTouches[0].screenX; }, { passive: true });
    modal.addEventListener('touchend', e => {
      const touchEndX = e.changedTouches[0].screenX;
      if (touchStartX - touchEndX > 50) showNext();
      if (touchEndX - touchStartX > 50) showPrev();
    }, { passive: true });
  });
</script>