---
layout: default
title: Главная
permalink: /
---

<style>
  .snowflake {
    position: absolute;
    background: white;
    border-radius: 50%;
    pointer-events: none;
    opacity: 0.8;
    z-index: 10;
  }

  @keyframes snowfall {
    0% {
      transform: translate(0, 0) rotate(0deg);
      opacity: 0;
    }
    10% {
      opacity: 1;
    }
    90% {
      opacity: 1;
    }
    100% {
      transform: translate(var(--end-x), 100vh) rotate(360deg);
      opacity: 0;
    }
  }

  @keyframes fadeInUp {
    from {
      opacity: 0;
      transform: translateY(30px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes fadeInLeft {
    from {
      opacity: 0;
      transform: translateX(-30px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }

  @keyframes fadeInRight {
    from {
      opacity: 0;
      transform: translateX(30px);
    }
    to {
      opacity: 1;
      transform: translateX(0);
    }
  }

  @keyframes fadeInScale {
    from {
      opacity: 0;
      transform: scale(0.95);
    }
    to {
      opacity: 1;
      transform: scale(1);
    }
  }

  @keyframes floatY {
    0%, 100% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(-10px);
    }
  }

  @keyframes floatDude {
    0%, 100% {
      transform: translateY(0);
    }
    50% {
      transform: translateY(-15px);
    }
  }

  .animate-fade-in-up {
    animation: fadeInUp 0.8s ease-out forwards;
    opacity: 0;
  }

  .animate-fade-in-left {
    animation: fadeInLeft 0.8s ease-out forwards;
    opacity: 0;
  }

  .animate-fade-in-right {
    animation: fadeInRight 0.8s ease-out forwards;
    opacity: 0;
  }

  .animate-fade-in-scale {
    animation: fadeInScale 0.6s ease-out forwards;
    opacity: 0;
  }

  .animate-float-y {
    animation: floatY 3s ease-in-out infinite;
  }

  .animate-float-dude {
    animation: floatDude 4s ease-in-out infinite;
  }

  .animation-delay-100 {
    animation-delay: 0.1s;
  }

  .animation-delay-200 {
    animation-delay: 0.2s;
  }

  .animation-delay-300 {
    animation-delay: 0.3s;
  }

  .animation-delay-400 {
    animation-delay: 0.4s;
  }

  .animation-delay-500 {
    animation-delay: 0.5s;
  }

  .animation-delay-600 {
    animation-delay: 0.6s;
  }

  .hero-section {
    height: 100vh;
    position: relative;
    overflow: hidden;
  }

  .video-background {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
  }

  .video-background video {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .video-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(to bottom, rgba(0, 0, 0, 0.8), rgba(0, 0, 0, 0.9));
    z-index: 2;
  }

  .hero-content {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 3;
  }

  .gradient-text {
    background: linear-gradient(90deg, #ec4b4b, #ff416c);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  .glow-on-hover {
    position: relative;
    overflow: hidden;
  }

  .glow-on-hover::before {
    content: '';
    position: absolute;
    top: 0;
    left: -100%;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.1), transparent);
    transition: left 0.7s;
  }

  .glow-on-hover:hover::before {
    left: 100%;
  }

  .reveal-on-scroll {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity 0.8s ease, transform 0.8s ease;
  }

  .reveal-on-scroll.visible {
    opacity: 1;
    transform: translateY(0);
  }

  .card-hover-effect {
    transition: all 0.3s ease;
  }

  .card-hover-effect:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 25px rgba(236, 75, 75, 0.2);
  }

  .list-item-animate {
    opacity: 0;
    transform: translateX(-20px);
    animation: fadeInLeft 0.6s ease-out forwards;
  }

  .gallery-section {
    margin: 4rem 0;
    width: 100%;
  }
  
  .gallery-title {
    text-align: center;
    color: white;
    font-size: 2.5rem;
    font-weight: bold;
    margin-bottom: 2rem;
  }
  
  .gallery-container {
    position: relative;
    width: 100%;
    max-width: 1200px;
    margin: 0 auto;
    overflow: hidden;
    border-radius: 12px;
  }
  
  .gallery-slides {
    display: flex;
    transition: transform 0.3s ease;
    width: 100%;
  }
  
  .gallery-slide {
    flex: 0 0 100%;
    width: 100%;
    position: relative;
    border-radius: 16px;
    overflow: hidden;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 1rem;
  }
  
  .gallery-image-container {
    height: 450px;
    border-radius: 16px;
    overflow: hidden;
    position: relative;
    background: rgba(15, 15, 15, 0.5);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  }
  
  .gallery-image-container img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    display: block;
  }
  
  .gallery-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    width: 50px;
    height: 50px;
    background: rgba(236, 75, 75, 0.8);
    border: none;
    border-radius: 50%;
    color: white;
    font-size: 24px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease;
    z-index: 10;
  }
  
  .gallery-btn:hover {
    background: rgba(236, 75, 75, 1);
    transform: translateY(-50%) scale(1.1);
  }
  
  .gallery-btn-prev {
    left: 20px;
  }
  
  .gallery-btn-next {
    right: 20px;
  }
  
  .gallery-indicators {
    display: flex;
    justify-content: center;
    gap: 0.5rem;
    margin-top: 1.5rem;
  }
  
  .gallery-indicator {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: rgba(255, 255, 255, 0.2);
    cursor: pointer;
    transition: all 0.3s ease;
  }
  
  .gallery-indicator:hover {
    background: rgba(236, 75, 75, 0.6);
    transform: scale(1.2);
  }
  
  .gallery-indicator.active {
    background: #ec4b4b;
    transform: scale(1.2);
    box-shadow: 0 0 10px rgba(236, 75, 75, 0.5);
  }

  .cta-section {
    position: relative;
    border-radius: 1.5rem;
    margin-top: 5rem;
    margin-bottom: 4rem;
    min-height: 300px;
  }

  .cta-bg {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: linear-gradient(to right, rgba(236, 75, 75, 0.2), rgba(255, 65, 108, 0.1));
    border-radius: 1.5rem;
    z-index: 1;
  }

  .cta-content {
    position: relative;
    z-index: 20;
    padding: 2rem;
    min-height: 300px;
  }

  .cta-image-container {
    position: absolute;
    z-index: 30;
    pointer-events: none;
    display: none;
  }

  .cta-image-container img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    object-position: bottom right;
  }

  .hero-mascot-container {
    position: absolute;
    z-index: 40;
    pointer-events: none;
    display: none;
    filter: drop-shadow(0 10px 25px rgba(0, 0, 0, 0.3));
  }

  .hero-mascot-container img {
    object-fit: contain;
    display: block;
    width: 100%;
    height: 100%;
  }

  @media (max-width: 767px) {
    .hero-mascot-container,
    .cta-image-container {
      display: none !important;
    }
    
    .gallery-image-container {
      height: 350px;
    }
    
    .gallery-btn {
      width: 40px;
      height: 40px;
      font-size: 20px;
    }
    
    .gallery-btn-prev {
      left: 10px;
    }
    
    .gallery-btn-next {
      right: 10px;
    }
  }

  @media (min-width: 768px) and (max-width: 1023px) {
    .hero-mascot-container {
      display: none;
    }
    
    .cta-image-container {
      display: none;
    }
    
    .cta-content {
      padding-right: 320px;
      padding-left: 2rem;
      padding-top: 2rem;
      padding-bottom: 2rem;
    }
  }

  @media (min-width: 1024px) and (max-width: 1279px) {
    .hero-mascot-container {
      display: none;
    }
    
    .cta-image-container {
      display: none;
    }
    
    .cta-content {
      padding-right: 400px;
      padding-left: 4rem;
      padding-top: 3rem;
      padding-bottom: 3rem;
    }
  }

  @media (min-width: 1280px) and (max-width: 1535px) {
    .hero-mascot-container {
      display: block;
      width: 1100px;
      height: 1100px;
      right: -450px;
      bottom: -285px;
      transform: rotate(4.5deg);
      transform-origin: bottom right;
    }
    
    .cta-image-container {
      display: block;
      width: 600px;
      height: 450px;
      right: 80px;
      bottom: -50px;
    }
    
    .cta-content {
      padding-right: 450px;
      padding-left: 5rem;
    }
  }

  @media (min-width: 1536px) {
    .hero-mascot-container {
      display: block;
      width: 1100px;
      height: 1100px;
      right: -430px;
      bottom: -280px;
      transform: rotate(5deg);
      transform-origin: bottom right;
    }
    
    .cta-image-container {
      display: block;
      width: 560px;
      height: 470px;
      right: 70px;
      bottom: -75px;
    }
    
    .cta-content {
      padding-right: 450px;
      padding-left: 5rem;
    }
  }

  @media (max-width: 480px) {
    .gallery-image-container {
      height: 250px;
    }
    
    .gallery-title {
      font-size: 2rem;
    }
  }
</style>

<section class="hero-section">
  <div class="video-background">
    <video autoplay muted loop playsinline>
      <source src="https://assets.mixkit.co/videos/41576/41576-720.mp4" type="video/mp4">
    </video>
  </div>
  
  <div class="video-overlay"></div>
  
  <div id="snow-container"></div>
  
  <div class="hero-content">
    <div class="w-full max-w-4xl px-4 text-center">
      <h1 class="text-5xl md:text-7xl lg:text-8xl font-bold text-white mb-4 animate-fade-in-up">
        RODINA <span class="text-primary-red">ROLEPLAY</span>
      </h1>
      
      <p class="text-xl md:text-2xl lg:text-3xl text-gray-300 mb-8 animate-fade-in-up animation-delay-100">
        Многопользовательская онлайн игра с огромным Открытым миром<br>в котором ты можешь стать кем захочешь
      </p>
      
      <div class="flex flex-col sm:flex-row gap-6 justify-center mt-12 animate-fade-in-up animation-delay-200">
        <a href="https://gamemonitoring.ru/unturned/servers/10706611/connect" 
           target="_blank"
           class="px-10 py-5 bg-gradient-to-r from-primary-red to-accent-red text-white font-bold text-xl rounded-xl hover:shadow-2xl hover:shadow-primary-red/40 transition-all duration-300 hover:scale-105 flex items-center justify-center gap-3 group relative overflow-hidden glow-on-hover">
          <i class="bi bi-play-fill text-2xl"></i>
          НАЧАТЬ ИГРУ
          <i class="bi bi-arrow-right text-2xl group-hover:translate-x-2 transition-transform"></i>
        </a>
        
        <a href="https://discord.gg/4ZmEmNzk" 
           target="_blank"
           class="px-10 py-5 bg-[#5865F2] text-white font-bold text-xl rounded-xl hover:shadow-2xl hover:shadow-[#5865F2]/40 transition-all duration-300 hover:scale-105 flex items-center justify-center gap-3 relative overflow-hidden glow-on-hover">
          <i class="bi bi-discord text-2xl"></i>
          DISCORD
        </a>
      </div>
    </div>
  </div>
</section>

<div class="space-y-20 mb-20 container mx-auto px-4">
    <div class="relative reveal-on-scroll">
        <div class="grid md:grid-cols-2 gap-8 items-center">
            <div class="relative z-10 order-2 md:order-1 animate-fade-in-left">
                <h2 class="text-3xl font-bold text-white mb-4">Ролевая игра в российских реалиях</h2>
                <p class="text-gray-300 mb-6 text-lg">
                    Основная цель проекта — создать правдоподобную социальную среду для отыгрыша. Создание персонажа начинается с выбора имени, фамилии и пола. Дальнейшая же глубина и уникальность вашего героя полностью определяются вашими действиями, речами и социальными связями в игре.
                </p>
                <ul class="space-y-3 text-gray-400">
                    <li class="flex items-center gap-3 list-item-animate" style="animation-delay: 0.1s;">
                        <i class="bi bi-check-circle text-primary-red text-lg"></i>
                        <span>Отыгрыш и взаимодействие в рамках российского социума</span>
                    </li>
                    <li class="flex items-center gap-3 list-item-animate" style="animation-delay: 0.2s;">
                        <i class="bi bi-check-circle text-primary-red text-lg"></i>
                        <span>Экономика, основанная на взаимодействии игроков</span>
                    </li>
                    <li class="flex items-center gap-3 list-item-animate" style="animation-delay: 0.3s;">
                        <i class="bi bi-check-circle text-primary-red text-lg"></i>
                        <span>Живые события и конфликты, формируемые сообществом</span>
                    </li>
                </ul>
            </div>
            <div class="relative order-1 md:order-2 animate-fade-in-right">
                <div class="absolute -top-6 -right-6 w-48 h-48 bg-primary-red/20 rounded-full blur-3xl floating-element" style="animation-delay: 0.5s;"></div>
                <div class="relative rounded-2xl overflow-hidden border border-primary-red/30 shadow-2xl card-hover-effect">
                    <img src="{{ site.baseurl }}/assets/img/mpo.png" 
                         alt="Магазин премиальной одежды" 
                         class="w-full h-64 md:h-80 object-cover hover:scale-110 transition-transform duration-700">
                </div>
            </div>
        </div>
    </div>
</div>

<div class="gallery-section reveal-on-scroll">
    <h2 class="gallery-title animate-fade-in-up">Скриншоты с сервера</h2>
    
    <div class="gallery-container">
        <div class="gallery-slides" id="gallerySlides">
            <div class="gallery-slide">
                <div class="gallery-image-container">
                    <img src="{{ site.baseurl }}/assets/img/carshop.png" alt="Магазин авто">
                </div>
            </div>
            <div class="gallery-slide">
                <div class="gallery-image-container">
                    <img src="{{ site.baseurl }}/assets/img/fishing.png" alt="Рыбалка">
                </div>
            </div>
            <div class="gallery-slide">
                <div class="gallery-image-container">
                    <img src="{{ site.baseurl }}/assets/img/kpp.png" alt="Локация">
                </div>
            </div>
            <div class="gallery-slide">
                <div class="gallery-image-container">
                    <img src="{{ site.baseurl }}/assets/img/kpz.png" alt="Кафе">
                </div>
            </div>
            <div class="gallery-slide">
                <div class="gallery-image-container">
                    <img src="{{ site.baseurl }}/assets/img/mpo.png" alt="Магазин премиальной одежды">
                </div>
            </div>
            <div class="gallery-slide">
                <div class="gallery-image-container">
                    <img src="{{ site.baseurl }}/assets/img/rosgvardia.png" alt="Росгвардия">
                </div>
            </div>
            <div class="gallery-slide">
                <div class="gallery-image-container">
                    <img src="{{ site.baseurl }}/assets/img/sberbank.png" alt="Сбербанк">
                </div>
            </div>
            <div class="gallery-slide">
                <div class="gallery-image-container">
                    <img src="{{ site.baseurl }}/assets/img/sturm.png" alt="Штурм здания">
                </div>
            </div>
        </div>
        
        <button class="gallery-btn gallery-btn-prev animate-fade-in-left" id="prevBtn">
            <i class="bi bi-chevron-left"></i>
        </button>
        <button class="gallery-btn gallery-btn-next animate-fade-in-right" id="nextBtn">
            <i class="bi bi-chevron-right"></i>
        </button>
    </div>
    

    <div class="gallery-indicators" id="galleryIndicators">
        <div class="gallery-indicator active" data-index="0"></div>
        <div class="gallery-indicator" data-index="1"></div>
        <div class="gallery-indicator" data-index="2"></div>
        <div class="gallery-indicator" data-index="3"></div>
        <div class="gallery-indicator" data-index="4"></div>
        <div class="gallery-indicator" data-index="5"></div>
        <div class="gallery-indicator" data-index="6"></div>
        <div class="gallery-indicator" data-index="7"></div>
    </div>
</div>

<div class="relative reveal-on-scroll">
    <div class="grid md:grid-cols-2 gap-8 items-center">
        <div class="relative animate-fade-in-left">
            <div class="absolute -top-6 -left-6 w-48 h-48 bg-accent-red/20 rounded-full blur-3xl floating-element" style="animation-delay: 0.3s;"></div>
            <div class="relative rounded-2xl overflow-hidden border border-primary-red/30 shadow-2xl card-hover-effect">
                <img src="{{ site.baseurl }}/assets/img/sturm.png" 
                     alt="Штурм здания" 
                     class="w-full h-64 md:h-80 object-cover hover:scale-110 transition-transform duration-700">
            </div>
        </div>
        <div class="relative z-10 animate-fade-in-right">
            <h2 class="text-3xl font-bold text-white mb-4">Кастомизация и фракции</h2>
            <p class="text-gray-300 mb-6 text-lg">
                Внешний вид — отличный инструмент отыгрыша. На сервере доступна <strong class="gradient-text">огромная библиотека одежды и экипировки</strong>, которую можно приобрести за внутриигровую валюту, заработанную в процессе RP. Выбирайте путь: вступайте в официальные государственные структуры или идите в криминальные группировки — каждая фракция предлагает свой уникальный стиль и возможности.
            </p>
            <div class="grid grid-cols-2 gap-4">
                <div class="bg-dark-bg/50 rounded-xl p-4 border border-gray-800 card-hover-effect reveal-on-scroll">
                    <i class="bi bi-shield-check text-2xl text-primary-red mb-2"></i>
                    <h4 class="font-bold text-white mb-1">Государственные структуры</h4>
                    <p class="text-sm text-gray-400">Правоохранительные органы, медики, адвокаты</p>
                </div>
                <div class="bg-dark-bg/50 rounded-xl p-4 border border-gray-800 card-hover-effect reveal-on-scroll" style="animation-delay: 0.2s;">
                    <i class="bi bi-shield-exclamation text-2xl text-accent-red mb-2"></i>
                    <h4 class="font-bold text-white mb-1">Криминальные группировки</h4>
                    <p class="text-sm text-gray-400">Банды, наркотики, черный рынок</p>
                </div>
            </div>
        </div>
    </div>
</div>

<div class="cta-section reveal-on-scroll">
    <div class="cta-bg"></div>
    
    <div class="cta-content">
        <h2 class="text-3xl md:text-4xl font-bold text-white mb-4 md:mb-6 animate-fade-in-up">Готовы начать игру?</h2>
        <p class="text-lg md:text-xl text-gray-300 mb-6 md:mb-8 max-w-full md:max-w-2xl animate-fade-in-up animation-delay-100">
            Присоединяйтесь к проекту, где ролевая игра строится вокруг России и её социальной структуры. Создайте персонажа, найдите свою фракцию, зарабатывайте на кастомизацию и станьте частью истории.
        </p>
        <div class="flex flex-col sm:flex-row gap-4 md:gap-6">
            <a href="https://gamemonitoring.ru/unturned/servers/10706611/connect" 
               target="_blank"
               class="px-6 py-3 md:px-10 md:py-4 bg-gradient-to-r from-primary-red to-accent-red text-white font-bold rounded-xl hover:shadow-2xl hover:shadow-primary-red/40 transition-all duration-300 hover:scale-105 flex items-center justify-center gap-3 text-sm md:text-base glow-on-hover animate-fade-in-up animation-delay-200">
                <i class="bi bi-controller text-lg md:text-xl"></i>
                Начать игру
            </a>
            <a href="{{ site.baseurl }}/rules/general/" 
               class="px-6 py-3 md:px-10 md:py-4 bg-dark-bg border-2 border-primary-red text-white font-bold rounded-xl hover:bg-primary-red/10 transition-all duration-300 flex items-center justify-center gap-3 text-sm md:text-base glow-on-hover animate-fade-in-up animation-delay-300">
                <i class="bi bi-book text-lg md:text-xl"></i>
                Правила
            </a>
        </div>
    </div>
    
    <div class="cta-image-container animate-float-dude">
        <img src="{{ site.baseurl }}/assets/img/dude.png" alt="Персонаж">
    </div>
</div>

<script>
  function createSnowflake() {
    const snowflake = document.createElement('div');
    snowflake.classList.add('snowflake');
    
    const size = Math.random() * 5 + 2;
    snowflake.style.width = `${size}px`;
    snowflake.style.height = `${size}px`;
    
    snowflake.style.left = `${Math.random() * 100}%`;
    snowflake.style.top = '-10px';
    
    const duration = Math.random() * 10 + 10;
    const delay = Math.random() * 5;
    const endX = Math.random() * 100 - 50;
    
    snowflake.style.animation = `snowfall ${duration}s linear ${delay}s infinite`;
    snowflake.style.setProperty('--end-x', `${endX}px`);
    
    document.getElementById('snow-container').appendChild(snowflake);
    
    setTimeout(() => {
      snowflake.remove();
    }, (duration + delay) * 1000);
  }

  function createSnowfall() {
    setInterval(createSnowflake, 50);
  }

  document.addEventListener('DOMContentLoaded', function() {
    const gallerySlides = document.getElementById('gallerySlides');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    const indicators = document.querySelectorAll('.gallery-indicator');
    
    let currentSlide = 0;
    const totalSlides = 8;
    
    function updateCarousel() {
      gallerySlides.style.transform = `translateX(-${currentSlide * 100}%)`;
      
      indicators.forEach((indicator, index) => {
        indicator.classList.toggle('active', index === currentSlide);
      });
    }
    
    function nextSlide() {
      currentSlide = (currentSlide + 1) % totalSlides;
      updateCarousel();
    }
    
    function prevSlide() {
      currentSlide = (currentSlide - 1 + totalSlides) % totalSlides;
      updateCarousel();
    }
    
    function goToSlide(index) {
      currentSlide = index;
      updateCarousel();
    }
    
    prevBtn.addEventListener('click', prevSlide);
    nextBtn.addEventListener('click', nextSlide);
    
    indicators.forEach((indicator, index) => {
      indicator.addEventListener('click', () => goToSlide(index));
    });
    
    let autoSlideInterval = setInterval(nextSlide, 5000);
    
    const galleryContainer = document.querySelector('.gallery-container');
    galleryContainer.addEventListener('mouseenter', () => {
      clearInterval(autoSlideInterval);
    });
    
    galleryContainer.addEventListener('mouseleave', () => {
      autoSlideInterval = setInterval(nextSlide, 5000);
    });
    
    updateCarousel();

    const revealElements = document.querySelectorAll('.reveal-on-scroll');
    
    const revealOnScroll = function() {
      revealElements.forEach(element => {
        const elementTop = element.getBoundingClientRect().top;
        const elementVisible = 150;
        
        if (elementTop < window.innerHeight - elementVisible) {
          element.classList.add('visible');
        }
      });
    };
    
    window.addEventListener('scroll', revealOnScroll);
    revealOnScroll();

    createSnowfall();

    const listItems = document.querySelectorAll('.list-item-animate');
    listItems.forEach((item, index) => {
      item.style.animationDelay = `${0.1 * (index + 1)}s`;
    });

    const floatingElements = document.querySelectorAll('.floating-element');
    floatingElements.forEach((element, index) => {
      if (!element.style.animationDelay) {
        element.style.animationDelay = `${0.2 * index}s`;
      }
    });

    setTimeout(() => {
      document.body.classList.add('loaded');
    }, 100);
  });
</script>