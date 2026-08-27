---
layout: page
title: Comunitate
description: ""
---

<!-- Global CSS for Modern Card Design & Responsiveness -->
<style>
  .hero-card {
    display: flex;
    flex-direction: row;
    background-color: #ffffff;
    border: 1px solid #e0e0e0;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
    margin-bottom: 2rem;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
  }
  
  .hero-card:hover {
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.12);
  }

  .hero-card.reverse {
    flex-direction: row-reverse;
  }

  .hero-image {
    flex: 1;
    min-height: 280px;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
  }

  .hero-content {
    flex: 1;
    padding: 2.5rem 2rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    color: #1a1a1a;
  }

  .hero-content h1, .hero-content h3 {
    color: #111111 !important;
    margin-top: 0;
    margin-bottom: 1rem;
    font-weight: 700;
  }

  .hero-content p {
    color: #333333;
    font-size: 1.05rem;
    line-height: 1.6;
    margin-bottom: 0.8rem;
  }

  .btn-container {
    display: flex;
    gap: 0.75rem;
    margin-top: 1.5rem;
    flex-wrap: wrap;
  }

  .btn-primary {
    background-color: #2e7d32;
    color: #ffffff !important;
    padding: 12px 20px;
    text-decoration: none;
    border-radius: 6px;
    font-weight: 600;
    display: inline-flex;
    align-items: center;
    gap: 10px;
    transition: background-color 0.2s ease;
  }

  .btn-primary:hover {
    background-color: #1b5e20;
  }

  .btn-secondary {
    background-color: #f5f5f5;
    color: #2e7d32 !important;
    border: 1px solid #2e7d32;
    padding: 12px 20px;
    text-decoration: none;
    border-radius: 6px;
    font-weight: 600;
    display: inline-block;
    transition: background-color 0.2s ease;
  }

  .btn-secondary:hover {
    background-color: #e8f5e9;
  }

  /* Responsive layout for mobile devices */
  @media (max-width: 768px) {
    .hero-card, .hero-card.reverse {
      flex-direction: column;
    }
    .hero-image {
      height: 220px;
      width: 100%;
    }
    .hero-content {
      padding: 1.5rem;
    }
  }
</style>

<br>

<div class="hero-card">
  <div class="hero-image" style="background-image: url('assets/images/hero_copii.jpg');"></div>
  <div class="hero-content">
    <h3 style="font-size: 1.6rem;">Susține-ți copiii să practice Ju-Jitsu!</h3>
    <p>Redirecționează 3,5% din impozit către Clubul Sportiv ShiTeiDoKo, fără niciun cost suplimentar pentru tine.</p>
    <div class="btn-container">
      <a href="https://shiteidoko.github.io/comunitate" class="btn-primary">Susține ShiTeiDoKo</a>
    </div>
  </div>
</div>

<br>

### Rămâi conectat cu noi

Suntem prezenți pe rețelele de socializare:

* 📱 **[Social Media](https://shiteidoko.github.io/media)** – Urmărește activitatea din club.
* ⭐ **[Recenzii Facebook](https://www.facebook.com/shiteidoko/reviews/)** – Vezi ce spun membrii și părinții despre noi.

<br>

### Descarcă materiale

Vrei să ne susții sau sa printezi stickerele ? Descarcă pachetul nostru:

* 📥 **[ShiTeiDoKo Sticker Pack (.zip)](https://shiteidoko.github.io/assets/sticker_pack.zip)**