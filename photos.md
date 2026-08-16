---
layout: page
title: Foto
permalink: /photos/
---


<!-- GLightbox CSS & JS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/glightbox/dist/css/glightbox.min.css" />
<script src="https://cdn.jsdelivr.net/gh/mcstudios/glightbox/dist/js/glightbox.min.js"></script>

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

  /* Card Styling aligned with Minima */
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

  /* Robust GLightbox Controls Reset for Minima v2 */
  .glightbox-container .gbtn {
    background: rgba(0, 0, 0, 0.6) !important;
    border-radius: 50% !important;
    width: 46px !important;
    height: 46px !important;
    padding: 0 !important;
    display: flex !important;
    align-items: center !important;
    justify-content: center !important;
  }

  .glightbox-container .gbtn svg {
    width: 22px !important;
    height: 22px !important;
    display: block !important;
  }

  .glightbox-container .gbtn svg path {
    fill: #ffffff !important;
  }
</style>

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
             class="photo-card glightbox" 
             data-gallery="gallery-{{ year }}"
             data-title="{{ caption }}">
            
            <img src="{{ file.path | relative_url }}" alt="{{ caption }}" loading="lazy">
            <div class="photo-caption">{{ caption }}</div>
          </a>
        {% endif %}
      {% endfor %}
    </div>
  </section>
{% endfor %}

<script>
  document.addEventListener('DOMContentLoaded', function() {
    GLightbox({
      selector: '.glightbox',
      loop: true
    });
  });
</script>