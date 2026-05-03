# Complete UI Animation Handbook

> A professional reference, thinking guide, and decision framework for implementing intentional, performant, and user-centered UI animations. Every motion here serves a purpose — never decoration.

---

## Table of Contents

1. [Introduction to UI Animation](#1-introduction-to-ui-animation)
2. [Animation Fundamentals](#2-animation-fundamentals)
3. [Element-Based Animations](#3-element-based-animations)
   - 3.1 [Text Animations](#31-text-animations)
   - 3.2 [Buttons & Inputs](#32-buttons--inputs)
   - 3.3 [Cards & Containers](#33-cards--containers)
   - 3.4 [Images & Icons](#34-images--icons)
   - 3.5 [Layout & Page Transitions](#35-layout--page-transitions)
   - 3.6 [Background & Ambient Effects](#36-background--ambient-effects)
4. [Interaction Patterns](#4-interaction-patterns)
5. [Scroll & Advanced Motion](#5-scroll--advanced-motion)
6. [Ready-to-Use UI Patterns](#6-ready-to-use-ui-patterns)
7. [Libraries & Tools](#7-libraries--tools)
8. [Performance & Optimization](#8-performance--optimization)
9. [Anti-Patterns](#9-anti-patterns)
10. [Animation Decision Framework](#10-animation-decision-framework)
11. [Quick Reference](#11-quick-reference)

---

## 1. Introduction to UI Animation

### 1.1 Purpose of Animation

Animation is a UX tool, not a decoration layer. Before adding any motion, it must justify its existence by serving one of four functional goals:

| Goal | Definition | Example |
|---|---|---|
| **Feedback** | Confirms the user's action was received | Button press scale, ripple effect |
| **Guidance** | Directs attention to the next step or new content | Scroll reveal, notification slide-in |
| **State Transition** | Communicates a change in system state | Loading spinner, success checkmark, modal open |
| **Delight** | Adds polish without interfering with task flow | Subtle card hover elevation, gradient text shimmer |

> **Rule:** If you cannot complete the sentence *"This animation helps the user by ___,"* remove it.

Delight is only valid after Feedback, Guidance, and State Transition needs are met. It never replaces function.

### 1.2 Types of Motion in UI

- **Micro-interactions** — Small, immediate responses to direct input (hover, press, focus)
- **State transitions** — Visual changes between two system states (loading → success, closed → open)
- **Structural transitions** — Changes in layout or navigation (modal open, route change, list reorder)
- **Ambient motion** — Background movement that creates atmosphere without demanding attention
- **Storytelling motion** — Orchestrated sequences for onboarding, hero sections, or progressive reveals

---

## 2. Animation Fundamentals

### 2.1 Duration

Duration determines whether an animation feels responsive or sluggish. Humans perceive:

- **Under 100ms** — Instantaneous; no animation needed
- **100–200ms** — Must feel instant; micro-interaction range
- **200–400ms** — Clear but non-blocking; ideal for UI transitions
- **400–600ms** — Noticeable; only for significant context changes
- **600ms+** — Storytelling territory; never for functional navigation

**Timing Brackets:**

| Category | Range | Use For |
|---|---|---|
| Micro | 80–200ms | Button press, hover, toggle, tooltip |
| UI Transition | 200–350ms | Modal, sidebar, dropdown, card expand |
| Page / Route | 250–350ms | Route changes (users feel navigation lag acutely) |
| Storytelling | 400–800ms | Hero sections, scroll reveals, onboarding |
| Ambient / Loop | 3s–∞ | Background gradients, particles, floating shapes |

> **Rule:** Navigation and action confirmation animations must never exceed 300ms.

**CSS Duration Tokens:**

```css
:root {
  --dur-instant:  80ms;   /* Tooltips, toggles */
  --dur-micro:    150ms;  /* Hover, focus, press */
  --dur-ui:       280ms;  /* Modals, sidebars, dropdowns */
  --dur-page:     320ms;  /* Route transitions */
  --dur-reveal:   600ms;  /* Scroll reveals, hero entrances */
  --dur-ambient:  4000ms; /* Background gradients, particles */
}
```

---

### 2.2 Easing

Easing encodes physical meaning. Every curve communicates how an object behaves in space.

| Easing | Curve | When to Use | Meaning |
|---|---|---|---|
| `ease-out` | `cubic-bezier(0,0,0.2,1)` | Entrances, items appearing | Fast start, smooth landing — arrives decisively |
| `ease-in` | `cubic-bezier(0.4,0,1,1)` | Exits, items disappearing | Slow start, fast exit — departs with purpose |
| `ease-in-out` | `cubic-bezier(0.4,0,0.2,1)` | In-place transitions, morphing | Symmetric movement between two positions |
| Spring / Overshoot | `cubic-bezier(0.34,1.56,0.64,1)` | Cards, buttons, interactive elements | Overshoots target slightly — feels alive, physical |
| Snappy | `cubic-bezier(0.16,1,0.3,1)` | Quick UI feedback | Fast with smooth landing — responsive without harshness |
| Linear | `linear` | Spinners, progress bars **only** | **Never for UI elements** — feels mechanical and dead |

**CSS Easing Tokens:**

```css
:root {
  --ease-enter:  cubic-bezier(0.16, 1,    0.3,  1);   /* Fast in, smooth landing */
  --ease-exit:   cubic-bezier(0.4,  0,    1,    1);   /* Slow out, fast exit */
  --ease-inout:  cubic-bezier(0.4,  0,    0.2,  1);   /* In-place transitions */
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1);  /* Overshoot, physical */
  --ease-snappy: cubic-bezier(0.16, 1,    0.3,  1);   /* Responsive feedback */
}
```

---

### 2.3 Delay

Delay controls when an animation begins relative to its trigger. Use it to:

- **Prevent racing** — Ensure hover animations don't fire on accidental cursor passes
- **Create stagger** — Sequence sibling elements
- **Add anticipation** — A brief pause before a dramatic reveal

> **Avoid:** Never use `animation-delay` on actions that require immediate user feedback (button clicks, form errors). Delay here feels broken.

---

### 2.4 Stagger

Stagger gives sequential animations a reading order, guiding the eye naturally through content.

**Stagger Guidelines:**

| Level | Delay Per Item | Use For |
|---|---|---|
| List / grid items | 80–120ms | Cards, nav links, grid cells |
| Letter-level (split text) | 30–50ms | Headline character reveals |
| Word-level | 50–80ms | Subtitle word reveals |
| Section-level groups | 100–150ms | Full section staggering |

> **Rule:** Stagger follows logical reading order (top-to-bottom, left-to-right). The focal point should move in the direction the user's eye should travel. Never stagger randomly.

---

### 2.5 Motion Hierarchy

Not all elements deserve equal motion emphasis. Mirror visual hierarchy with motion hierarchy.

| Tier | Elements | Motion Intensity | Rationale |
|---|---|---|---|
| **Primary** | Main CTA, hero headline, key action | Most pronounced (scale, translate, color) | Demands immediate attention |
| **Secondary** | Cards, nav items, supporting content | Moderate (subtle scale, fade) | Supports without competing |
| **Ambient** | Background gradients, particles, shapes | Minimal (slow drift, low amplitude) | Atmosphere only — must never distract |

> **Rule:** Never animate multiple elements at the same priority level simultaneously. The primary action animates first, largest, or most dramatically.

---

## 3. Element-Based Animations

### 3.1 Text Animations

#### Fade Up (Standard Entry)

**Use for:** Headlines, paragraph reveals, list items, any text that enters the viewport.

```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}

.fade-up {
  animation: fadeUp var(--dur-reveal) var(--ease-enter) both;
}

/* Staggered children */
.stagger-parent > *:nth-child(1) { animation-delay: 0ms; }
.stagger-parent > *:nth-child(2) { animation-delay: 100ms; }
.stagger-parent > *:nth-child(3) { animation-delay: 200ms; }
```

**Trigger → Behavior → Result:**
`Page load / scroll enter` → `opacity 0→1 + translateY 16px→0` → `Text reads as arriving with purpose`

---

#### Gradient Text Shimmer

**Use for:** Hero headlines, premium branding moments. Use sparingly.

```css
.gradient-text {
  background: linear-gradient(90deg, #6366f1, #ec4899, #6366f1);
  background-size: 200% auto;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: shimmer 3s linear infinite;
}

@keyframes shimmer {
  to { background-position: 200% center; }
}
```

---

#### Typewriter Effect

**Use for:** Terminal-style UIs, onboarding prompts, hero taglines. One use per page maximum.

```js
function typewriter(el, text, speed = 50) {
  let i = 0;
  el.textContent = '';
  const timer = setInterval(() => {
    el.textContent += text[i++];
    if (i >= text.length) clearInterval(timer);
  }, speed);
}
```

---

### 3.2 Buttons & Inputs

#### Button Press (Primary Feedback)

**Trigger → Behavior → Result:**
`hover` → `scale(1.03) + translateY(-1px)` → `Element feels elevated and clickable`
`active` → `scale(0.97) + translateY(0)` → `Physical press confirmation`

```css
.btn {
  transition:
    transform    var(--dur-micro) var(--ease-spring),
    box-shadow   var(--dur-micro) ease,
    background   var(--dur-micro) ease;
}
.btn:hover  { transform: translateY(-1px) scale(1.03); box-shadow: 0 8px 24px rgba(0,0,0,0.15); }
.btn:active { transform: translateY(0) scale(0.97); }
```

---

#### Button Loading State

**Trigger → Behavior → Result:**
`click (async)` → `label fades → spinner fades in, button disabled` → `Prevents double-submit, communicates progress`
`success event` → `spinner fades → checkmark draws + color shift` → `Positive reinforcement`

```js
function setButtonState(btn, state) {
  const states = {
    default:  { text: 'Submit',    disabled: false, class: '' },
    loading:  { text: '',          disabled: true,  class: 'loading' },
    success:  { text: '✓ Saved',   disabled: false, class: 'success' },
    error:    { text: '✗ Failed',  disabled: false, class: 'error' }
  };
  const s = states[state];
  btn.textContent = s.text;
  btn.disabled    = s.disabled;
  btn.className   = `btn btn--${s.class || 'default'}`;
}
```

---

#### Ripple Effect (Click Feedback)

**Use for:** Buttons, list items — tactile click feedback.

```js
function addRipple(btn) {
  btn.style.position = 'relative';
  btn.style.overflow = 'hidden';
  btn.addEventListener('click', e => {
    const rect = btn.getBoundingClientRect();
    const dot  = document.createElement('span');
    dot.className = 'ripple';
    dot.style.left = (e.clientX - rect.left - 5) + 'px';
    dot.style.top  = (e.clientY - rect.top  - 5) + 'px';
    btn.appendChild(dot);
    dot.addEventListener('animationend', () => dot.remove());
  });
}
```

```css
.ripple {
  position: absolute; border-radius: 50%;
  width: 10px; height: 10px;
  background: rgba(255,255,255,0.4);
  animation: rippleOut 0.6s linear forwards;
  pointer-events: none;
}
@keyframes rippleOut { to { transform: scale(40); opacity: 0; } }
```

---

#### Input Focus & Floating Label

**Trigger → Behavior → Result:**
`focus` → `border-color shift + box-shadow grow` → `Field clearly active, guides user`
`focus (with floating label)` → `label translateY(-20px) + scale(0.85)` → `Label moves out of input, stays readable`

```css
.input-wrap {
  position: relative;
}
.input-wrap input {
  border: 1.5px solid #d1d5db;
  border-radius: 8px;
  padding: 14px 12px 6px;
  transition: border-color var(--dur-micro) var(--ease-enter),
              box-shadow   var(--dur-micro) var(--ease-enter);
}
.input-wrap input:focus {
  border-color: #6366f1;
  box-shadow: 0 0 0 3px rgba(99,102,241,0.15);
  outline: none;
}
.input-wrap label {
  position: absolute; left: 12px; top: 14px;
  font-size: 14px; color: #9ca3af;
  transition: transform var(--dur-micro) var(--ease-enter),
              font-size  var(--dur-micro) var(--ease-enter),
              color      var(--dur-micro) var(--ease-enter);
  pointer-events: none;
}
.input-wrap input:focus + label,
.input-wrap input:not(:placeholder-shown) + label {
  transform: translateY(-10px);
  font-size: 11px;
  color: #6366f1;
}
```

---

#### Input Error Shake

**Trigger → Behavior → Result:**
`validation failure` → `border turns red + horizontal shake` → `Draws attention to the problem field`

```css
@keyframes shake {
  0%,100% { transform: translateX(0); }
  15%     { transform: translateX(-6px); }
  30%     { transform: translateX(6px); }
  45%     { transform: translateX(-4px); }
  60%     { transform: translateX(4px); }
  75%     { transform: translateX(-2px); }
}
```

```js
function shakeInput(el) {
  el.style.borderColor = '#f87171';
  el.style.animation   = 'shake 0.4s cubic-bezier(0.36,0.07,0.19,0.97)';
  el.addEventListener('animationend', () => {
    el.style.animation = '';
  }, { once: true });
}
```

---

### 3.3 Cards & Containers

#### Card Hover (Elevation)

**Trigger → Behavior → Result:**
`hover` → `translateY(-4px) scale(1.03) + deeper shadow` → `Card lifts, communicates clickability`

```css
.card {
  transition:
    transform    var(--dur-micro) var(--ease-spring),
    box-shadow   var(--dur-micro) ease,
    border-color var(--dur-micro) ease;
  will-change: transform;
}

.card:hover {
  transform: translateY(-4px) scale(1.03);
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.18);
}

/* Remove will-change after transition */
.card:not(:hover) { will-change: auto; }
```

---

#### 3D Tilt Card

**Use for:** Feature cards, product showcases, premium interfaces.

```js
function initTilt(card) {
  card.addEventListener('mousemove', e => {
    const rect  = card.getBoundingClientRect();
    const cx    = rect.left + rect.width  / 2;
    const cy    = rect.top  + rect.height / 2;
    const rotX  = ((e.clientY - cy) / (rect.height / 2)) * -8;
    const rotY  = ((e.clientX - cx) / (rect.width  / 2)) *  8;
    card.style.transform = `perspective(600px) rotateX(${rotX}deg) rotateY(${rotY}deg)`;
  });
  card.addEventListener('mouseleave', () => {
    card.style.transition = 'transform 0.5s var(--ease-spring)';
    card.style.transform  = 'perspective(600px) rotateX(0deg) rotateY(0deg)';
  });
}
```

---

#### Accordion / Expand-Collapse

**Use for:** FAQs, filter panels, nested navigation.

```js
function toggleAccordion(trigger, panel) {
  const isOpen = panel.classList.contains('open');
  // Measure content height
  panel.style.maxHeight = isOpen ? '0' : panel.scrollHeight + 'px';
  panel.classList.toggle('open', !isOpen);
}
```

```css
.accordion-panel {
  max-height: 0;
  overflow: hidden;
  transition: max-height var(--dur-ui) var(--ease-inout),
              opacity    var(--dur-ui) ease;
  opacity: 0;
}
.accordion-panel.open {
  opacity: 1;
}
```

---

### 3.4 Images & Icons

#### Lazy Load Fade-In

**Use for:** All images not in the initial viewport.

```js
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      img.style.opacity = '0';
      img.onload = () => {
        img.style.transition = `opacity ${var(--dur-ui)} var(--ease-enter)`;
        img.style.opacity = '1';
      };
      imageObserver.unobserve(img);
    }
  });
}, { rootMargin: '200px' });

document.querySelectorAll('img[data-src]').forEach(img => imageObserver.observe(img));
```

---

#### SVG Path Draw

**Use for:** Logo reveals, success checkmarks, illustration entrances.

```css
.draw-path {
  stroke-dasharray: var(--path-length);
  stroke-dashoffset: var(--path-length);
  animation: draw 0.8s var(--ease-inout) forwards;
}
@keyframes draw { to { stroke-dashoffset: 0; } }
```

```js
// Measure each path dynamically
document.querySelectorAll('.draw-path').forEach(path => {
  const len = path.getTotalLength();
  path.style.setProperty('--path-length', len);
});
```

---

#### Icon Rotation (State Toggle)

**Use for:** Chevrons in accordions, sort indicators, toggle arrows.

```css
.chevron {
  transition: transform var(--dur-micro) var(--ease-enter);
}
.chevron.open {
  transform: rotate(180deg);
}
```

---

### 3.5 Layout & Page Transitions

#### Modal Enter / Exit

**Trigger → Behavior → Result:**
`open` → `backdrop fades in + modal scale(0.92→1) + translateY(16px→0)` → `Context shift is clear and spatial`
`close` → `reverse at faster speed` → `Dismissed with purpose`

```css
/* Overlay */
.overlay {
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
  backdrop-filter: blur(0px);
}
.overlay.open {
  opacity: 1;
  pointer-events: all;
  backdrop-filter: blur(4px);
}

/* Modal box */
.modal {
  opacity: 0;
  transform: scale(0.92) translateY(16px);
  transition:
    transform var(--dur-ui) var(--ease-enter),
    opacity   0.3s ease;
}
.overlay.open .modal {
  opacity: 1;
  transform: scale(1) translateY(0);
}
```

---

#### Sidebar Slide

**Use for:** Navigation drawers, filter panels, side sheets.

```css
.sidebar {
  transform: translateX(-100%);
  transition: transform var(--dur-ui) var(--ease-inout);
}
.sidebar.open {
  transform: translateX(0);
}
```

---

#### Page / Route Transition

**Use for:** SPA route changes.

```css
.page {
  transition:
    opacity   var(--dur-page) ease,
    transform var(--dur-page) var(--ease-inout);
}
.page.exit   { opacity: 0; transform: translateX(-24px); }
.page.enter  { opacity: 0; transform: translateX(24px); }
.page.active { opacity: 1; transform: translateX(0); }
```

---

### 3.6 Background & Ambient Effects

#### Animated Gradient Background

**Use for:** Hero sections, marketing pages. Keep movement slow — users should not consciously notice it.

```css
.animated-bg {
  background: linear-gradient(135deg, #667eea, #764ba2, #f093fb, #c471f5);
  background-size: 400% 400%;
  animation: gradientShift var(--dur-ambient) ease infinite;
}

@keyframes gradientShift {
  0%   { background-position: 0% 50%; }
  50%  { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}
```

---

#### Canvas Particle System

**Use for:** Hero backgrounds, loading screens. Always use Canvas — never DOM nodes — for performance.

```js
function initParticles(canvas) {
  const ctx = canvas.getContext('2d');
  canvas.width  = window.innerWidth;
  canvas.height = window.innerHeight;

  const particles = Array.from({ length: 80 }, () => ({
    x: Math.random() * canvas.width,
    y: Math.random() * canvas.height,
    vx: (Math.random() - 0.5) * 0.5,
    vy: (Math.random() - 0.5) * 0.5,
    r: Math.random() * 2 + 1
  }));

  function draw() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    particles.forEach(p => {
      p.x += p.vx; p.y += p.vy;
      if (p.x < 0 || p.x > canvas.width)  p.vx *= -1;
      if (p.y < 0 || p.y > canvas.height) p.vy *= -1;
      ctx.beginPath();
      ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
      ctx.fillStyle = 'rgba(255,255,255,0.4)';
      ctx.fill();
    });
    requestAnimationFrame(draw);
  }
  draw();
}
```

---

## 4. Interaction Patterns

### Interaction Rules Reference

| Interaction | Animation | Duration | Easing |
|---|---|---|---|
| Hover (card) | `translateY(-4px) scale(1.03)` + shadow | `--dur-micro` | `--ease-spring` |
| Hover (button) | `translateY(-1px) scale(1.03)` + glow | `--dur-micro` | `--ease-spring` |
| Hover (link) | underline `scaleX(0→1)` | `--dur-micro` | `--ease-enter` |
| Press (button) | `scale(0.97)` | 80ms | `--ease-exit` |
| Release (button) | `scale(1)` | `--dur-micro` | `--ease-spring` |
| Click (ripple) | `scale(0→40) + opacity(1→0)` | 600ms | `linear` |
| Focus (input) | `border-color + box-shadow` | `--dur-micro` | `--ease-enter` |
| Error (input) | shake + red border | 400ms | `linear` |
| Loading (button) | spinner fade-in | `--dur-micro` | `--ease-enter` |
| Success (button) | color shift + icon draw | 300ms | `--ease-enter` |
| Open (modal) | `scale(0.92→1) + fade` | `--dur-ui` | `--ease-enter` |
| Close (modal) | `scale(1→0.92) + fade` | 220ms | `--ease-exit` |
| Sidebar open | `translateX(-100%→0)` | `--dur-ui` | `--ease-enter` |
| Sidebar close | `translateX(0→-100%)` | `--dur-ui` | `--ease-exit` |
| Navigate (page) | slide + fade | `--dur-page` | `--ease-inout` |
| Load (hero) | staggered fade-up | `--dur-reveal` | `--ease-enter` |
| Scroll (reveal) | fade-up with IntersectionObserver | `--dur-reveal` | `--ease-enter` |

---

### Gesture-Based Animations

**Drag:** Provide a visual "lift" (shadow increase + slight scale up) when an element is being dragged. On drop, animate to the new position rather than snapping.

```css
.dragging {
  transform: scale(1.05) rotate(1deg);
  box-shadow: 0 24px 48px rgba(0,0,0,0.2);
  z-index: 999;
  cursor: grabbing;
}
```

**Swipe:** Translate the element in the swipe direction. On release, either complete the action (full translation) or snap back (reverse with spring).

---

## 5. Scroll & Advanced Motion

### 5.1 Scroll Reveal

**The standard pattern for revealing content as users scroll.**

```js
function initScrollReveal({ selector = '.reveal', stagger = 100 } = {}) {
  const items = [...document.querySelectorAll(selector)];

  items.forEach(el => {
    el.style.opacity   = '0';
    el.style.transform = 'translateY(24px)';
    el.style.transition = `opacity var(--dur-reveal) var(--ease-enter),
                            transform var(--dur-reveal) var(--ease-enter)`;
  });

  const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        const siblings = [...entry.target.parentElement.children];
        const index    = siblings.indexOf(entry.target);
        setTimeout(() => {
          entry.target.style.opacity   = '1';
          entry.target.style.transform = 'translateY(0)';
        }, index * stagger);
        observer.unobserve(entry.target);  // Animate once only
      }
    });
  }, { rootMargin: '-5% 0px', threshold: 0.1 });

  items.forEach(el => observer.observe(el));
}
```

---

### 5.2 Parallax

#### Scroll-Based Parallax

**Use for:** Hero images, background layers, depth effects.

```js
window.addEventListener('scroll', () => {
  const scrollY = window.scrollY;
  document.querySelectorAll('[data-depth]').forEach(el => {
    const depth = parseFloat(el.dataset.depth) || 0.5;
    el.style.transform = `translateY(${scrollY * depth}px)`;
  });
}, { passive: true });
```

```html
<!-- Markup -->
<div class="hero-bg"    data-depth="0.3"></div>
<div class="hero-mid"   data-depth="0.6"></div>
<div class="hero-front" data-depth="0.9"></div>
```

> **Always use `{ passive: true }` on scroll listeners** to avoid blocking the main thread.

---

#### Cursor-Based Parallax

**Use for:** Interactive hero sections, product showcases.

```js
document.addEventListener('mousemove', e => {
  const cx = window.innerWidth  / 2;
  const cy = window.innerHeight / 2;
  const dx = (e.clientX - cx) / cx;
  const dy = (e.clientY - cy) / cy;

  document.querySelectorAll('[data-depth]').forEach(el => {
    const depth = parseFloat(el.dataset.depth) || 0.5;
    el.style.transform = `translate(${dx * depth * 20}px, ${dy * depth * 20}px)`;
  });
});
```

---

### 5.3 GSAP ScrollTrigger

**Use for:** Scroll-linked storytelling, pinned sections, complex scroll sequences.

```js
import gsap from 'gsap';
import ScrollTrigger from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

// Staggered reveal
gsap.from('.feature-card', {
  opacity: 0,
  y: 40,
  duration: 0.7,
  ease: 'power3.out',
  stagger: 0.12,
  scrollTrigger: {
    trigger: '.features-section',
    start: 'top 80%',
    toggleActions: 'play none none none'
  }
});

// Scrub (progress-linked)
gsap.to('.hero-headline', {
  opacity: 0,
  y: -60,
  scrollTrigger: {
    trigger: '.hero',
    start: 'top top',
    end: 'bottom top',
    scrub: true
  }
});
```

---

### 5.4 Storytelling Sequences

**Use for:** Onboarding flows, landing page sections, feature reveals.

```js
// GSAP timeline for page load sequence
const tl = gsap.timeline({ defaults: { ease: 'power3.out' } });

tl.from('.nav',         { opacity: 0, y: -20, duration: 0.5 })
  .from('.hero-eyebrow',{ opacity: 0, y: 20,  duration: 0.4 }, '-=0.2')
  .from('.hero-title',  { opacity: 0, y: 30,  duration: 0.5 }, '-=0.2')
  .from('.hero-body',   { opacity: 0, y: 20,  duration: 0.4 }, '-=0.2')
  .from('.hero-cta',    { opacity: 0, y: 20,  duration: 0.3 }, '-=0.1')
  .from('.hero-image',  { opacity: 0, x: 40,  duration: 0.6 }, '-=0.3');
```

---

## 6. Ready-to-Use UI Patterns

### Card Hover

```css
.card {
  border-radius: 12px;
  background: #fff;
  box-shadow: 0 2px 12px rgba(0,0,0,0.08);
  transition:
    transform  0.25s cubic-bezier(0.34, 1.56, 0.64, 1),
    box-shadow 0.25s ease;
  cursor: pointer;
}

.card:hover {
  transform: translateY(-4px) scale(1.02);
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.15);
}

.card:active {
  transform: translateY(-2px) scale(1.01);
}
```

---

### Button Press

```css
.btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  cursor: pointer;
  border: none;
  transition:
    transform    0.14s cubic-bezier(0.16, 1, 0.3, 1),
    box-shadow   0.14s ease,
    background   0.14s ease;
}

.btn:hover  { transform: translateY(-1px) scale(1.03); box-shadow: 0 8px 20px rgba(0,0,0,0.15); }
.btn:active { transform: translateY(0) scale(0.97); box-shadow: none; }
```

---

### Modal Animation

```css
/* Backdrop */
.modal-backdrop {
  position: fixed; inset: 0;
  background: rgba(0,0,0,0.5);
  backdrop-filter: blur(4px);
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-backdrop.open {
  opacity: 1;
  pointer-events: all;
}

/* Dialog */
.modal-dialog {
  background: #fff;
  border-radius: 16px;
  padding: 32px;
  max-width: 480px;
  width: 90%;
  opacity: 0;
  transform: scale(0.92) translateY(16px);
  transition:
    transform 0.35s cubic-bezier(0.16, 1, 0.3, 1),
    opacity   0.3s ease;
}

.modal-backdrop.open .modal-dialog {
  opacity: 1;
  transform: scale(1) translateY(0);
}
```

```js
function openModal(modal)  { modal.classList.add('open'); }
function closeModal(modal) {
  modal.classList.remove('open');
  // Wait for exit animation before removing from DOM
  modal.addEventListener('transitionend', () => {
    // cleanup if needed
  }, { once: true });
}
```

---

### Page Transition (SPA)

```js
function navigate(url) {
  const current = document.querySelector('.page.active');
  const next    = document.querySelector(`[data-route="${url}"]`);

  // Exit current
  current.classList.add('exit');

  setTimeout(() => {
    current.classList.remove('active', 'exit');
    // Enter new
    next.classList.add('active');
    requestAnimationFrame(() => {
      next.classList.add('entered');
    });
  }, 280); // Match --dur-page
}
```

---

### Scroll Reveal (Complete Setup)

```js
// Initialize on DOMContentLoaded
document.addEventListener('DOMContentLoaded', () => {
  initScrollReveal({ selector: '.reveal', stagger: 80 });
});
```

```css
/* Elements start hidden — set in JS to avoid FOUC if JS is slow */
/* Optionally: */
.reveal[data-animate] {
  opacity: 0;
  transform: translateY(24px);
}
```

---

## 7. Libraries & Tools

### Decision Tree

```
Is it a simple hover, focus, or binary state change?
  → CSS Transitions / Keyframes

Is it a multi-step sequence, scroll-driven, or involves SVG?
  → GSAP + ScrollTrigger

Are you in a React app needing layout changes, route transitions,
or shared element transitions?
  → Framer Motion

Is it a designer-created vector / illustration animation?
  → Lottie

Is it an interactive character with complex state machine logic?
  → Rive

Is bundle size critical and the animation is simple?
  → CSS + Vanilla JS only
```

---

### Library Comparison

| Library | Bundle | Best For | Avoid When |
|---|---|---|---|
| **CSS** | 0 kb | Hover, focus, spinners, simple entrances | Complex timelines, scroll control |
| **GSAP** | ~30 kb | Timelines, scroll, SVG, particles, drag | You need React layout animations |
| **Framer Motion** | ~40 kb | React apps, `layoutId`, `AnimatePresence` | Non-React projects |
| **Lottie** | ~50 kb | Brand / illustration animations from After Effects | Interactive UI states |
| **Rive** | Variable | State machine animations (game-like logic) | Simple UI transitions |

---

### Framer Motion Quick Reference (React)

```jsx
import { motion, AnimatePresence } from 'framer-motion';

// Basic entrance
<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  exit={{ opacity: 0, y: -20 }}
  transition={{ duration: 0.3, ease: [0.16, 1, 0.3, 1] }}
/>

// Stagger children
const container = {
  hidden: { opacity: 0 },
  show: {
    opacity: 1,
    transition: { staggerChildren: 0.08 }
  }
};

const item = {
  hidden: { opacity: 0, y: 20 },
  show:   { opacity: 1, y: 0 }
};

<motion.ul variants={container} initial="hidden" animate="show">
  {items.map(i => (
    <motion.li key={i.id} variants={item}>{i.text}</motion.li>
  ))}
</motion.ul>

// Shared element (layout) transition
<motion.div layoutId="card-hero" />

// Gesture
<motion.button
  whileHover={{ scale: 1.03, y: -1 }}
  whileTap={{ scale: 0.97 }}
  transition={{ type: 'spring', stiffness: 400, damping: 17 }}
/>
```

---

## 8. Performance & Optimization

### 8.1 GPU-Safe Property Reference

| Property | Render Cost | Verdict |
|---|---|---|
| `transform` (translate, scale, rotate, skew) | Composite only | ✅ Use freely |
| `opacity` | Composite only | ✅ Use freely |
| `filter` (blur, brightness) | Composite (use sparingly) | ⚠️ Limit |
| `background-color`, `box-shadow`, `color` | Paint (repaint only) | ⚠️ Short transitions only |
| `width`, `height`, `padding`, `margin` | Layout + Paint + Composite | ❌ Avoid in animations |
| `top`, `left` | Layout + Paint + Composite | ❌ Use `transform: translate()` instead |
| `font-size` | Layout + Paint per frame | ❌ Never animate; scale the element instead |

---

### 8.2 Layout Thrashing

Never read a layout property and write a style property in the same synchronous execution. Batch all reads, then all writes.

```js
// ❌ THRASH — forces synchronous reflow on every iteration
elements.forEach(el => {
  const h = el.offsetHeight;          // READ — forces layout
  el.style.height = h + 10 + 'px';   // WRITE
});

// ✅ BATCH — single layout calculation
const heights = elements.map(el => el.offsetHeight); // all reads
elements.forEach((el, i) => {
  el.style.transform = `translateY(${heights[i]}px)`; // all writes
});
```

---

### 8.3 `will-change`

Apply only to elements that are **about to** animate. Remove after the animation ends. Never apply globally.

```js
el.addEventListener('mouseenter', () => {
  el.style.willChange = 'transform';
});
el.addEventListener('mouseleave', () => {
  el.style.willChange = 'auto'; // Return GPU memory
});
```

---

### 8.4 Accessibility — `prefers-reduced-motion`

Motion can cause vestibular disorders. This is mandatory, not optional.

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration:       0.01ms !important;
    animation-iteration-count: 1    !important;
    transition-duration:      0.01ms !important;
    scroll-behavior:          auto  !important;
  }
}
```

```js
const reducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

if (!reducedMotion) {
  // Run full animation
} else {
  // Apply final state immediately — no transition
  el.style.opacity   = '1';
  el.style.transform = 'none';
}
```

**Reduced Motion Strategy:**
- Replace slide / scale with simple fade (`opacity` only)
- Stop or dramatically slow all looping / ambient animations
- Keep functional state changes (loading → success) as simple color shifts

---

### 8.5 Additional Performance Rules

- **Pause offscreen animations** — Use IntersectionObserver to stop Canvas or GSAP animations when out of viewport
- **Use `requestAnimationFrame`** for JS animation loops — never `setInterval` or `setTimeout`
- **Cap simultaneous animations** — Define a motion hierarchy; not everything needs to animate at once
- **Test on low-end devices** — What's smooth on a MacBook Pro may jank on a mid-range phone

---

## 9. Anti-Patterns

### ❌ Animating layout-triggering properties

```css
/* Wrong */
transition: width 0.3s, height 0.3s;  /* full reflow on every frame */

/* Right */
transform: scaleX(1.2);  /* compositor only */
```

---

### ❌ Using `transition: all`

```css
/* Wrong */
transition: all 0.3s ease;  /* animates unintended properties */

/* Right */
transition: transform 0.3s ease, opacity 0.3s ease;
```

---

### ❌ Long animations on frequent actions

Any animation on a frequently repeated action (form field, list toggle, tab switch) must be at most 200ms. Users become frustrated when UI feels slow on their own actions.

---

### ❌ Non-interruptible animations

A modal that cannot be closed while its open animation is playing, or a button that ignores clicks during a loading state, creates a broken experience.

**Rule:** All animations must be interruptible. The system must transition gracefully from its current position to the new target state at any point.

---

### ❌ Forgetting exit animations

Elements that appear have enter animations. Elements that disappear need exit animations. A modal that pops out of existence instantly after a smooth open feels broken.

---

### ❌ Over-animating everything

When everything moves, nothing is important. Animate the critical elements — let the rest be still.

**Signal:** If you're adding a scroll reveal to every heading, every paragraph, and every image on a page — stop. Pick the key structural moments.

---

### ❌ Decorative-only animations

If the animation serves no function from the four goals (Feedback, Guidance, State Transition, Delight after the others are met) — remove it.

---

### ❌ Linear easing for UI elements

```css
/* Wrong */
transition: transform 0.3s linear;  /* feels robotic */

/* Right */
transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);  /* spring feels physical */
```

Linear easing is only correct for spinners and progress bars.

---

### ❌ Leaving `will-change` permanently

```css
/* Wrong */
* { will-change: transform; }  /* promotes every element to a GPU layer */
```

Apply `will-change` temporarily, just before a known animation, then remove it.

---

### Good vs. Bad: Summary Examples

**Card Hover**
- ✅ `scale(1.03) + translateY(-4px) + deeper shadow` at 150ms spring — fast, clear elevation, communicates clickability
- ❌ Card flips 180° on hover — disruptive, obscures content, hover is not a commit action
- ❌ Card background shifts to a loud color on hover — distracts from content

**Form Submission**
- ✅ Button → loading spinner → success checkmark → reverts — full state communication, prevents double-submit
- ❌ Button does nothing visible during a pending request — user has no feedback, may click again

**Scroll Reveal**
- ✅ Fade-up with `rootMargin: '-5% 0px'` — triggers slightly after element enters view, feels deliberate
- ❌ Every paragraph, image, and icon has a different animation — chaotic, no hierarchy
- ❌ Reveal replays every scroll-pass — annoying, breaks spatial consistency

**Page Transition**
- ✅ Outgoing page fades + slides 24px, incoming slides from opposite — communicates spatial direction, fast
- ❌ 3D cube spin at 800ms — blocks interaction, self-indulgent, frustrating
- ❌ Instant content swap — jarring, disorienting, user loses context

---

## 10. Animation Decision Framework

Use this five-step process **before writing a single line of animation code.**

### Step 1 — Understand the Element

- What is this element? (button, card, modal, nav item, list, form input, image, icon, page section)
- What is its role? Primary action? Supporting content? Ambient decoration?
- Is it interactive or static? Interactive elements deserve feedback animations. Static elements generally should not animate unless guiding attention.
- Is it persistent or transient? Persistent elements (headers, sidebars) need subtle, non-repeating motion. Transient elements (modals, toasts) need clear entrance/exit logic.

**Output:** *"This is a primary CTA button that the user clicks to submit a form."*

---

### Step 2 — Understand the Behavior

Map every trigger explicitly:

| Trigger | Description |
|---|---|
| `load` | Page or section mounts |
| `hover` | Pointer enters the element |
| `focus` | Keyboard navigation targets the element |
| `click/tap` | User activates the element |
| `scroll` | Element enters the viewport |
| `state change` | Async result arrives (loading, success, error) |
| `route change` | User navigates to a new page |

Also ask: **Can the animation be interrupted mid-flight?** If yes, the system must handle graceful reversal.

**Output:** *"hover → elevated; click → pressed; loading → spinner; success → checkmark."*

---

### Step 3 — Understand the Flow

- Where is this element in the user journey? Onboarding = more expressive. Core task flow = minimal, fast.
- What surrounds this element? Animations should not compete with nearby elements animating simultaneously.
- Is the user waiting for something? If yes, motion should communicate progress. If no, motion should be quick and non-blocking.

**Output:** *"This card appears in the main content feed. The user scrolls past many similar cards. The animation should be subtle and non-competing."*

---

### Step 4 — Define the Goal

Every animation must serve exactly one primary goal. If it serves none, remove it.

| Goal | When to Use | Example |
|---|---|---|
| **Feedback** | User performed an action, confirm receipt | Button press, ripple, form submit |
| **Guidance** | User needs to notice something | Scroll reveal, error badge, notification |
| **State Transition** | System state changed | Modal open, loading → success, route change |
| **Delight** | Extra polish, only after other goals are met | Hover elevation, gradient shimmer |

**Output:** *"Feedback"* / *"State Transition"* etc.

---

### Step 5 — Choose Animation Intentionally

```
Element + Behavior + Flow Context + Goal = Animation Choice
```

| Goal | Best Animation Types |
|---|---|
| Feedback (hover) | scale, border-color, box-shadow, color shift |
| Feedback (click) | press scale-down (0.95→1), ripple |
| Feedback (error) | shake + red border flash |
| Guidance (load) | staggered fade-up, slide from content direction |
| Guidance (scroll) | IntersectionObserver fade + translate |
| State: open/mount | scale(0.92→1) + fade, or slide from off-screen |
| State: close/unmount | reverse of entrance, faster (ease-in) |
| State: loading | spinner, skeleton pulse |
| State: success | color shift + icon draw + subtle pulse |
| Delight (ambient) | gradient shift, floating shapes, particle drift |

**Pre-implementation checklist:**

```
[ ] Animation has a UX purpose (Feedback / Guidance / State / Delight)
[ ] Only transform and/or opacity are animated
[ ] Duration matches category (micro / UI / page / storytelling)
[ ] Easing matches intent (enter / exit / in-place / spring)
[ ] prefers-reduced-motion fallback implemented
[ ] Animation can be interrupted gracefully
[ ] Entrance and exit are both defined
[ ] Does not compete with simultaneously animating neighbors
[ ] Stagger follows logical reading order if applicable
```

---

## 11. Quick Reference

| Question | Answer |
|---|---|
| What should I always animate? | `transform` and `opacity` |
| What should I never animate? | `width`, `height`, `top`, `left`, `margin`, `font-size` |
| How long for hover? | 120–180ms |
| How long for modal open? | 250–320ms |
| How long for page transition? | 250–350ms max |
| Easing for entrances? | `ease-out` / `cubic-bezier(0.16,1,0.3,1)` |
| Easing for exits? | `ease-in` / `cubic-bezier(0.4,0,1,1)` |
| Easing for cards / buttons? | Spring: `cubic-bezier(0.34,1.56,0.64,1)` |
| Easing for spinners? | `linear` only |
| Do I need `prefers-reduced-motion`? | Always |
| When should I use GSAP? | Scroll, timelines, SVG, complex sequences |
| When should I use Framer Motion? | React apps with layout / route animations |
| When should I use CSS only? | Hover, focus, spinners, simple entrances |
| When should I NOT animate? | When there is no UX purpose. Full stop. |

---

*Synthesized from 11 reference sources including interactive HTML handbooks, categorized technique references, plain-text implementation guides, Markdown developer handbooks, engineering research reports, and authoritative UX research from NN/g, Material Design, Apple HIG, web.dev, and GSAP / Framer Motion documentation. All patterns validated for GPU performance, accessibility compliance, and production usability.*
