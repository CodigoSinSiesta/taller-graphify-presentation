<script lang="ts">
  import { onMount } from 'svelte';
  import gsap from 'gsap';
  import { animateSlideEntrance, addButtonHoverAnimation } from '../utils/animations';

  let currentSlide = $state(0);
  let slides = $state<HTMLElement[]>([]);
  let menuOpen = $state(false);

  let touchStartX = 0;
  let touchStartY = 0;
  let touchEndX = 0;
  let touchEndY = 0;
  let isSwiping = false;
  let lastSwipeTime = 0;

  const slideNames = [
    'hero',
    'problema',
    'idea-karpathy',
    'anatomia',
    'plantillas',
    'flujo-ingesta',
    'hands-on-setup',
    'hands-on-ingest',
    'mas-alla',
    'cierre'
  ] as const;

  const slideTitles: Record<(typeof slideNames)[number], string> = {
    hero: 'Inicio',
    problema: 'El problema',
    'idea-karpathy': 'La idea (Karpathy)',
    anatomia: 'Anatomía del wiki',
    plantillas: 'Plantillas y vocabulario',
    'flujo-ingesta': 'Flujo de ingesta',
    'hands-on-setup': 'Hands-on 1 · Setup',
    'hands-on-ingest': 'Hands-on 2 · Ingest',
    'mas-alla': 'Más allá',
    cierre: 'Cierre'
  };

  const totalSlides = slideNames.length;

  onMount(() => {
    slides = Array.from(document.querySelectorAll('.swiper-slide')) as HTMLElement[];

    if (slides.length > 0) {
      slides[0].classList.add('swiper-slide-active');
      animateSlideEntrance(slides[0]);
    }

    const buttons = document.querySelectorAll('.btn');
    buttons.forEach((btn) => {
      addButtonHoverAnimation(btn as HTMLElement);
    });

    const hash = window.location.hash.replace('#/', '');
    const slideIndex = slideNames.indexOf(hash as (typeof slideNames)[number]);
    if (slideIndex !== -1 && slideIndex !== currentSlide) {
      goToSlide(slideIndex);
    }

    const handleHashChange = () => {
      const nextHash = window.location.hash.replace('#/', '');
      const nextSlide = slideNames.indexOf(nextHash as (typeof slideNames)[number]);
      if (nextSlide !== -1 && nextSlide !== currentSlide) {
        goToSlide(nextSlide);
      }
    };

    window.addEventListener('hashchange', handleHashChange);
    return () => window.removeEventListener('hashchange', handleHashChange);
  });

  $effect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key === 'ArrowRight') {
        nextSlide();
      } else if (e.key === 'ArrowLeft') {
        prevSlide();
      } else if (e.key === 'Escape' && menuOpen) {
        menuOpen = false;
      }
    };

    window.addEventListener('keydown', handleKeyDown);
    return () => window.removeEventListener('keydown', handleKeyDown);
  });

  function updateHash(slideName: string) {
    window.location.hash = `#/${slideName}`;
  }

  function goToSlide(index: number) {
    if (index < 0 || index >= slides.length || index === currentSlide) return;

    if (menuOpen) {
      menuOpen = false;
    }

    const previousSlide = slides[currentSlide];
    if (previousSlide) {
      gsap.to(previousSlide, {
        opacity: 0,
        duration: 0.3,
        ease: 'power2.out',
        onComplete: () => previousSlide.classList.remove('swiper-slide-active')
      });
    }

    currentSlide = index;
    slides[currentSlide]?.classList.add('swiper-slide-active');
    animateSlideEntrance(slides[currentSlide]);
    updateHash(slideNames[currentSlide]);
  }

  function goToSlideFromMenu(index: number) {
    menuOpen = false;
    goToSlide(index);
  }

  function nextSlide() {
    goToSlide(currentSlide + 1);
  }

  function prevSlide() {
    goToSlide(currentSlide - 1);
  }

  const SWIPE_THRESHOLD = 50;
  const SWIPE_DEBOUNCE = 300;

  function handleTouchStart(event: TouchEvent) {
    const touch = event.touches[0];
    touchStartX = touch.clientX;
    touchStartY = touch.clientY;
    touchEndX = touch.clientX;
    touchEndY = touch.clientY;
    isSwiping = true;
  }

  function handleTouchMove(event: TouchEvent) {
    if (!isSwiping) return;
    const touch = event.touches[0];
    touchEndX = touch.clientX;
    touchEndY = touch.clientY;
  }

  function handleTouchEnd() {
    if (!isSwiping) return;
    isSwiping = false;

    const deltaX = touchEndX - touchStartX;
    const deltaY = touchEndY - touchStartY;
    const absDeltaX = Math.abs(deltaX);
    const absDeltaY = Math.abs(deltaY);

    const now = Date.now();
    if (now - lastSwipeTime < SWIPE_DEBOUNCE) return;

    if (absDeltaX < SWIPE_THRESHOLD) return;
    if (absDeltaY > absDeltaX) return;

    const element = document.elementFromPoint(touchStartX, touchStartY);
    if (element) {
      const isScrollable =
        element.closest('.slide-drawer') ||
        element.closest('[style*="overflow"]') ||
        element.scrollHeight > element.clientHeight;
      if (isScrollable) return;
    }

    lastSwipeTime = now;

    if (menuOpen) {
      menuOpen = false;
    }

    if (deltaX > 0) {
      prevSlide();
    } else {
      nextSlide();
    }
  }
</script>

<div
  class="presentation-container"
  ontouchstart={handleTouchStart}
  ontouchmove={handleTouchMove}
  ontouchend={handleTouchEnd}
>
  <div class="swiper">
    <div class="swiper-wrapper">
      <slot />
    </div>
    <div class="swiper-pagination"></div>

    <button class="swiper-button-prev nav-btn nav-btn-prev" title="Diapositiva anterior" onclick={prevSlide}>
      <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="15 18 9 12 15 6"></polyline></svg>
    </button>

    <button class="swiper-button-next nav-btn nav-btn-next" title="Siguiente diapositiva" onclick={nextSlide}>
      <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="9 18 15 12 9 6"></polyline></svg>
    </button>
  </div>

  <div class="slide-counter">
    <span class="counter-number">{currentSlide + 1}</span>
    <span class="counter-separator">/</span>
    <span class="counter-total">{totalSlides}</span>
  </div>

  <button class="hamburger-btn" class:open={menuOpen} title="Menú de navegación" onclick={() => menuOpen = !menuOpen}>
    <span class="hamburger-line"></span>
    <span class="hamburger-line"></span>
    <span class="hamburger-line"></span>
  </button>

  {#if menuOpen}
    <div class="drawer-overlay" onclick={() => menuOpen = false}></div>
  {/if}

  <div class="slide-drawer" class:open={menuOpen}>
    <div class="drawer-header">
      <span class="drawer-title">Navegación</span>
      <button class="drawer-close" title="Cerrar menú" onclick={() => menuOpen = false}>
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
          <line x1="18" y1="6" x2="6" y2="18"></line>
          <line x1="6" y1="6" x2="18" y2="18"></line>
        </svg>
      </button>
    </div>
    <nav class="drawer-nav">
      <ul class="drawer-list">
        {#each slideNames as name, index}
          <li>
            <button class="drawer-item" class:active={index === currentSlide} onclick={() => goToSlideFromMenu(index)}>
              <span class="item-number">{index + 1}</span>
              <span class="item-title">{slideTitles[name]}</span>
            </button>
          </li>
        {/each}
      </ul>
    </nav>
  </div>
</div>

<style>
  .presentation-container {
    position: relative;
    width: 100%;
    height: 100vh;
    background-color: var(--color-base-dark);
    overflow: hidden;
  }

  .swiper {
    width: 100%;
    height: 100%;
    --swiper-theme-color: var(--color-electric);
  }

  :global(.swiper-wrapper) {
    width: 100%;
    height: 100%;
    overflow: hidden;
  }

  :global(.swiper-slide) {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: flex-start !important;
    justify-content: center;
    scroll-behavior: smooth;
    position: absolute !important;
    top: 0;
    left: 0;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.6s ease-out;
    visibility: hidden;
    overflow: auto;
  }

  :global(.swiper-slide-active) {
    opacity: 1;
    pointer-events: auto;
    visibility: visible;
    overflow-y: auto !important;
  }

  .nav-btn {
    position: fixed;
    top: 50%;
    transform: translateY(-50%);
    z-index: 100;
    width: 50px;
    height: 50px;
    background: rgba(30, 58, 138, 0.6);
    border: 2px solid var(--color-electric);
    border-radius: var(--radius-md);
    color: var(--color-neutral-light);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all var(--transition-base);
    backdrop-filter: blur(8px);
  }

  .nav-btn:hover {
    background: rgba(30, 58, 138, 0.9);
    border-color: var(--color-accent-bright);
    transform: translateY(-50%) scale(1.1);
  }

  .nav-btn-prev { left: var(--spacing-2xl); }
  .nav-btn-next { right: var(--spacing-2xl); }

  .slide-counter {
    position: fixed;
    bottom: var(--spacing-xl);
    right: var(--spacing-2xl);
    z-index: 100;
    font-family: var(--font-mono);
    font-size: 1.25rem;
    color: var(--color-electric);
    display: flex;
    gap: var(--spacing-grid);
    align-items: center;
    background: rgba(30, 58, 138, 0.4);
    padding: var(--spacing-md) var(--spacing-lg);
    border-radius: var(--radius-md);
    border: 1px solid rgba(96, 165, 250, 0.2);
    backdrop-filter: blur(8px);
  }

  .counter-separator,
  .counter-total {
    color: var(--color-neutral-light);
    opacity: 0.7;
  }

  .hamburger-btn {
    display: none;
    position: fixed;
    top: var(--spacing-lg);
    right: var(--spacing-lg);
    z-index: var(--z-modal);
    width: 48px;
    height: 48px;
    background: rgba(30, 58, 138, 0.6);
    border: 2px solid var(--color-electric);
    border-radius: var(--radius-md);
    cursor: pointer;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap: 5px;
    transition: all var(--transition-base);
    backdrop-filter: blur(8px);
  }

  .hamburger-line {
    display: block;
    width: 22px;
    height: 2px;
    background: var(--color-neutral-light);
    border-radius: 2px;
    transition: all var(--transition-fast);
    transform-origin: center;
  }

  .hamburger-btn.open .hamburger-line:nth-child(1) { transform: translateY(7px) rotate(45deg); }
  .hamburger-btn.open .hamburger-line:nth-child(2) { opacity: 0; transform: scaleX(0); }
  .hamburger-btn.open .hamburger-line:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

  .drawer-overlay {
    position: fixed;
    inset: 0;
    background: rgba(10, 22, 40, 0.7);
    z-index: var(--z-modal);
  }

  .slide-drawer {
    display: none;
    position: fixed;
    top: 0;
    right: 0;
    width: min(300px, 85vw);
    height: 100vh;
    background: linear-gradient(180deg, rgba(30, 58, 138, 0.95) 0%, rgba(10, 22, 40, 0.98) 100%);
    border-left: 2px solid var(--color-electric);
    z-index: var(--z-popover);
    transform: translateX(100%);
    transition: transform var(--transition-base) cubic-bezier(0.4, 0, 0.2, 1);
    overflow-y: auto;
    backdrop-filter: blur(12px);
  }

  .slide-drawer.open { transform: translateX(0); }

  .drawer-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: var(--spacing-lg);
    border-bottom: 1px solid rgba(96, 165, 250, 0.2);
  }

  .drawer-title {
    font-family: var(--font-display);
    font-size: 1.25rem;
    font-weight: 700;
    color: var(--color-neutral-light);
    letter-spacing: 0.05em;
    text-transform: uppercase;
  }

  .drawer-close {
    width: 48px;
    height: 48px;
    background: transparent;
    border: 2px solid var(--color-electric);
    border-radius: var(--radius-md);
    color: var(--color-neutral-light);
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .drawer-nav { padding: var(--spacing-md); }
  .drawer-list { list-style: none; padding: 0; margin: 0; }

  .drawer-item {
    width: 100%;
    min-height: 56px;
    padding: var(--spacing-md) var(--spacing-lg);
    background: transparent;
    border: none;
    border-radius: var(--radius-md);
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: var(--spacing-md);
    text-align: left;
    margin-bottom: var(--spacing-xs);
    color: inherit;
  }

  .drawer-item:hover { background: rgba(96, 165, 250, 0.15); }

  .drawer-item.active {
    background: rgba(59, 130, 246, 0.25);
    border-left: 4px solid var(--color-accent-bright);
  }

  .item-number {
    font-family: var(--font-mono);
    font-size: 0.875rem;
    font-weight: 600;
    color: var(--color-electric);
    min-width: 28px;
  }

  .item-title {
    font-family: var(--font-body);
    font-size: 1rem;
    font-weight: 500;
    color: var(--color-neutral-light);
  }

  @media (max-width: 768px) {
    .nav-btn {
      width: 44px;
      height: 44px;
    }

    .nav-btn-prev { left: var(--spacing-lg); }
    .nav-btn-next { right: var(--spacing-lg); }

    .slide-counter {
      bottom: var(--spacing-lg);
      right: var(--spacing-lg);
      font-size: 1rem;
      padding: var(--spacing-sm) var(--spacing-md);
    }

    .hamburger-btn { display: flex; }
    .slide-drawer { display: block; }

    :global(.swiper-slide) {
      overflow-y: auto;
      padding-top: 80px;
      padding-bottom: 72px;
    }
  }
</style>
