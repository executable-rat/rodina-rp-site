---
layout: default
title: Главная
permalink: /
---

<div class="hero-wrapper">
  <video 
    autoplay 
    muted 
    loop 
    playsinline 
    class="hero-video"
    aria-hidden="true"
  >
    <source src="https://assets.mixkit.co/videos/41576/41576-720.mp4" type="video/mp4">
  </video>
  
  <div class="hero-overlay" aria-hidden="true"></div>
  
  <div class="hero-content">
    <h1 class="hero-title">
      RODINA <span class="text-red">RP</span>
    </h1>
    
    <p class="hero-subtitle">
      Экскурсия в российскую душу через ролевую игру<br class="hidden sm:block">
      Станьте кем угодно в уникальной социальной структуре
    </p>

    
    <div class="hero-buttons">
      <a 
        href="https://gamemonitoring.ru/unturned/servers/10706611/connect" 
        target="_blank" 
        rel="noopener noreferrer"
        class="hero-button hero-button-play"
      >
        <i class="bi bi-play-fill"></i>
        НАЧАТЬ ИГРУ
        <i class="bi bi-arrow-right"></i>
      </a>
      
      <a 
        href="https://discord.gg/4ZmEmNzk" 
        target="_blank" 
        rel="noopener noreferrer"
        class="hero-button hero-button-discord"
      >
        <i class="bi bi-discord"></i>
        DISCORD
      </a>
    </div>
  </div>
</div>

<div class="content-background">
  <div class="main-content-container">
    <section class="russian-realities-section">
      <div class="russian-realities-grid">
        <div class="russian-realities-content">
          <h2 class="russian-realities-title">Ролевая игра в российских реалиях</h2>
          <p class="russian-realities-text">
            Основная цель проекта — создать правдоподобную социальную среду для отыгрыша. 
            Создание персонажа начинается с выбора имени, фамилии и пола. Дальнейшая же 
            глубина и уникальность вашего героя полностью определяются вашими действиями, 
            речами и социальными связями в игре.
          </p>
          <ul class="russian-realities-list">
            <li class="russian-realities-list-item">
              <i class="bi bi-check-circle russian-realities-list-icon"></i>
              <span>Отыгрыш и взаимодействие в рамках российского социума</span>
            </li>
            <li class="russian-realities-list-item">
              <i class="bi bi-check-circle russian-realities-list-icon"></i>
              <span>Экономика, основанная на взаимодействии игроков</span>
            </li>
            <li class="russian-realities-list-item">
              <i class="bi bi-check-circle russian-realities-list-icon"></i>
              <span>Живые события и конфликты, формируемые сообществом</span>
            </li>
          </ul>
        </div>
        <div class="russian-realities-image">
          <div class="russian-realities-image-container">
            <img src="{{ site.baseurl }}/assets/img/mpo.png" alt="Магазин премиальной одежды">
          </div>
        </div>
      </div>
    </section>

  <section class="screenshots-section">
    <h2 class="screenshots-title">Скриншоты с сервера</h2>
    <div class="screenshots-gallery">
      <div class="screenshots-slides" id="screenshotsSlides">
        <div class="screenshot-slide">
          <img src="{{ site.baseurl }}/assets/img/carshop.png" alt="Магазин авто">
          <div class="screenshot-info">
            <h3>Автосалон</h3>
            <p>Широкий выбор транспортных средств для игроков</p>
          </div>
        </div>
        <div class="screenshot-slide">
          <img src="{{ site.baseurl }}/assets/img/fishing.png" alt="Рыбалка">
          <div class="screenshot-info">
            <h3>Рыбалка</h3>
            <p>Спокойный отдых на природе с возможностью заработка</p>
          </div>
        </div>
        <div class="screenshot-slide">
          <img src="{{ site.baseurl }}/assets/img/kpp.png" alt="Локация">
          <div class="screenshot-info">
            <h3>Контрольный пункт</h3>
            <p>Стратегические точки на карте для контроля территории</p>
          </div>
        </div>
        <div class="screenshot-slide">
          <img src="{{ site.baseurl }}/assets/img/kpz.png" alt="Кафе">
          <div class="screenshot-info">
            <h3>Кафе "КПЗ"</h3>
            <p>Место для новых встреч и ролевых взаимодействий</p>
          </div>
        </div>
        <div class="screenshot-slide">
          <img src="{{ site.baseurl }}/assets/img/mpo.png" alt="Магазин премиальной одежды">
          <div class="screenshot-info">
            <h3>Премиальная одежда</h3>
            <p>Эксклюзивные вещи для кастомизации персонажа</p>
          </div>
        </div>
        <div class="screenshot-slide">
          <img src="{{ site.baseurl }}/assets/img/rosgvardia.png" alt="Росгвардия">
          <div class="screenshot-info">
            <h3>Росгвардия</h3>
            <p>Здание правоохранительных органов</p>
          </div>
        </div>
        <div class="screenshot-slide">
          <img src="{{ site.baseurl }}/assets/img/sberbank.png" alt="Сбербанк">
          <div class="screenshot-info">
            <h3>Сбербанк</h3>
            <p>Финансовый центр для хранения и "приумножения" средств</p>
          </div>
        </div>
        <div class="screenshot-slide">
          <img src="{{ site.baseurl }}/assets/img/sturm.png" alt="Штурм здания">
          <div class="screenshot-info">
            <h3>Тактические операции</h3>
            <p>Интенсивные моменты игрового процесса</p>
          </div>
        </div>
      </div>
      
      <button class="screenshot-nav screenshot-nav-prev" id="screenshotPrevBtn">
        <i class="bi bi-chevron-left"></i>
      </button>
      <button class="screenshot-nav screenshot-nav-next" id="screenshotNextBtn">
        <i class="bi bi-chevron-right"></i>
      </button>
      
      <div class="screenshot-counter" id="screenshotCounter">1 / 8</div>
    </div>
    
    <div class="screenshot-indicators" id="screenshotIndicators">
      <div class="screenshot-indicator active" data-index="0"></div>
      <div class="screenshot-indicator" data-index="1"></div>
      <div class="screenshot-indicator" data-index="2"></div>
      <div class="screenshot-indicator" data-index="3"></div>
      <div class="screenshot-indicator" data-index="4"></div>
      <div class="screenshot-indicator" data-index="5"></div>
      <div class="screenshot-indicator" data-index="6"></div>
      <div class="screenshot-indicator" data-index="7"></div>
    </div>
  </section>
    <section class="customization-section">
      <div class="customization-grid">
        <div class="customization-image-container">
          <img src="{{ site.baseurl }}/assets/img/sturm.png" alt="Штурм здания">
        </div>
        <div class="customization-content">
          <h2 class="customization-title">Кастомизация и фракции</h2>
          <p class="customization-text">
            Внешний вид — отличный инструмент отыгрыша. На сервере доступна 
            <strong>огромная библиотека одежды и экипировки</strong>, которую можно приобрести 
            за внутриигровую валюту, заработанную в процессе RP. Выбирайте путь: вступайте 
            в официальные государственные структуры или идите в криминальные группировки — 
            каждая фракция предлагает свой уникальный стиль и возможности.
          </p>
          <div class="customization-features">
            <div class="customization-feature">
              <i class="bi bi-shield-check customization-feature-icon"></i>
              <h3 class="customization-feature-title">Государственные структуры</h3>
              <p class="customization-feature-description">
                Правоохранительные органы, медики, адвокаты
              </p>
            </div>
            <div class="customization-feature">
              <i class="bi bi-shield-exclamation customization-feature-icon"></i>
              <h3 class="customization-feature-title">Криминальные группировки</h3>
              <p class="customization-feature-description">
                Банды, наркотики, черный рынок
              </p>
            </div>
          </div>
        </div>
      </div>
    </section>
  </div>
</div>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    const slidesContainer = document.getElementById('screenshotsSlides');
    const prevBtn = document.getElementById('screenshotPrevBtn');
    const nextBtn = document.getElementById('screenshotNextBtn');
    const indicators = document.querySelectorAll('.screenshot-indicator');
    const counter = document.getElementById('screenshotCounter');
    const totalSlides = document.querySelectorAll('.screenshot-slide').length;
    let currentSlide = 0;
    
    function updateSlide() {
      slidesContainer.style.transform = `translateX(-${currentSlide * 100}%)`;
      
      indicators.forEach((indicator, index) => {
        if (index === currentSlide) {
          indicator.classList.add('active');
        } else {
          indicator.classList.remove('active');
        }
      });
      
      counter.textContent = `${currentSlide + 1} / ${totalSlides}`;
    }
    
    prevBtn.addEventListener('click', () => {
      currentSlide = (currentSlide > 0) ? currentSlide - 1 : totalSlides - 1;
      updateSlide();
    });
    
    nextBtn.addEventListener('click', () => {
      currentSlide = (currentSlide < totalSlides - 1) ? currentSlide + 1 : 0;
      updateSlide();
    });
    
    indicators.forEach((indicator, index) => {
      indicator.addEventListener('click', () => {
        currentSlide = index;
        updateSlide();
      });
    });
    
    let autoplayInterval = setInterval(() => {
      currentSlide = (currentSlide < totalSlides - 1) ? currentSlide + 1 : 0;
      updateSlide();
    }, 5000);
    
    const gallery = document.querySelector('.screenshots-gallery');
    gallery.addEventListener('mouseenter', () => {
      clearInterval(autoplayInterval);
    });
    
    gallery.addEventListener('mouseleave', () => {
      autoplayInterval = setInterval(() => {
        currentSlide = (currentSlide < totalSlides - 1) ? currentSlide + 1 : 0;
        updateSlide();
      }, 5000);
    });
    
    updateSlide();
  });
</script>