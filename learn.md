rembg pillow for remove background
Navbar each section
 The implementation uses JavaScript + IntersectionObserver API:

  Core Technique

  // 1. Observe when element enters viewport
  const counterObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting && !entry.target.classList.contains('counted')) {
        entry.target.classList.add('counted'); // Prevent re-triggering
        animate(entry.target);
      }
    });
  }, { threshold: 0.5 });

  // 2. Animation loop using setInterval
  function animate(element) {
    const target = parseInt(element.dataset.target);
    const suffix = element.dataset.suffix || '';
    let current = 0;
    const increment = target / 60; // Steps to reach target
    const timer = setInterval(() => {
      current += increment;
      if (current >= target) {
        current = target;
        clearInterval(timer); // Stop when done
      }
      element.textContent = Math.floor(current).toLocaleString() + suffix;
    }, 25); // ~1.5s total (60 steps × 25ms)
  }

  Key Components
  
  Component: IntersectionObserver
  Purpose: Detects when counter enters viewport (triggers once)
  ────────────────────────────────────────
  Component: data-target
  Purpose: Stores final number value in HTML
  ────────────────────────────────────────
  Component: data-suffix
  Purpose: Stores "+" or "K" suffix
  ────────────────────────────────────────
  Component: .counted class
  Purpose: Prevents re-animation on scroll back
  ────────────────────────────────────────
  Component: setInterval
  Purpose: Smooth incremental updates (~60fps)
  ────────────────────────────────────────
  Component: toLocaleString()
  Purpose: Formats numbers with thousand separators

  Alternatives
  
  - CSS: Can only animate opacity/transform, not numeric values
  - requestAnimationFrame: More performant for complex animations
  - Libraries: GSAP or CountUp.js for features like easing curves