<div align="center">

# TokioHub

### Selective AI software and automation agency. Three projects per month. No more.

Web Apps · AI Agents · Workflow Automation · Mobile · Cloud · Security

[![Live Site](https://img.shields.io/badge/Visit_Live-tokiohub.com-7c3aed?style=for-the-badge&logoColor=white)](https://tokiohub.com/)
[![Status](https://img.shields.io/badge/Status-Production-22c55e?style=for-the-badge)](https://tokiohub.com/)
[![License](https://img.shields.io/badge/Showcase-MIT-blue?style=for-the-badge)](#license)

</div>

---

## What is this?

**TokioHub** is a selective digital agency based in Santo Domingo, Dominican Republic. We build custom software — AI agents, workflow automation systems, and web applications — for businesses that refuse to settle for templates.

The constraint is intentional: three new projects per month, no exceptions. Every client gets one engineer's full attention from spec to deploy.

This repository is a **showcase**, a public portfolio of the agency's design, engineering, and product decisions. The source code is private.

> 🌐 **Live product:** [tokiohub.com](https://tokiohub.com/) · 📍 Santo Domingo, Dominican Republic · ⚡ Bitcoin-native team

---

## The Product

TokioHub's own website is also a demonstration of what we sell. Every feature on the page — the live AI chat, the bilingual switcher, the scroll animations — is a working example of production-grade implementation, not a mockup.

### 🤖 Live AI Agent Demo

The homepage features a real n8n-powered AI agent that responds in Spanish and English, understands context, and can discuss the agency's services in detail. It's not a canned FAQ bot. It's the same architecture deployed for clients.

- **Webhook integration:** n8n handles routing, context management, and response formatting
- **Session isolation:** each visitor gets a `crypto.randomUUID()` session ID, preventing conversation bleed across users
- **Rate limiting:** 2-second cooldown between messages, 500-character input cap — client-side guards against abuse
- **Graceful degradation:** connection errors surface a localized fallback message instead of a broken UI
- **Suggestion chips:** pre-built prompts guide new users into the conversation without a blank-slate moment

### 🌐 Bilingual Interface (ES/EN)

The entire page switches language without a reload. Every visible string — navigation, headings, body copy, button labels, chat placeholder text, and even the AI greeting — is driven by a flat translation object.

- **Zero dependencies:** no i18n library, no router, no build step. One JS object, one `setLanguage()` function
- **Persistent preference:** language state survives in-session navigation
- **Flag switcher:** SVG flag icons inline in the nav, no external icon font

### 📱 Responsive Without Compromise

Two different layouts coexist in the same HTML: CSS Grid desktops for services and automation sections, scroll-snap sliders for mobile. No layout breaks, no hidden overflow.

- **Services section:** 3-column grid on desktop, snap-scrollable card carousel on mobile with dot indicators
- **Automation section:** same pattern, independently controlled
- **Navigation:** hamburger menu at 1024px, full-screen overlay with centered links, closes on any nav link click
- **Hero CTAs:** stack vertically below 768px, full-width buttons

### ✨ Scroll Animations

Every section and card uses `IntersectionObserver` with staggered delays. No GSAP, no ScrollMagic, no dependency.

- `threshold: 0.08` — triggers early enough to feel instant, late enough to avoid premature reveals
- `rootMargin: '0px 0px -60px 0px'` — prevents reveals from firing at the very bottom of the viewport
- Stagger delays from `0.1s` to `0.6s` via utility classes
- `prefers-reduced-motion` media query disables all animations entirely — no exceptions

### 💬 Floating Chat Button

A fixed chat button pulses in the corner when the AI demo section is off-screen. It disappears when the section is visible. Implemented with a second `IntersectionObserver` instance at `threshold: 0.2`.

---

## Portfolio Highlights

Three of TokioHub's past projects are featured on the homepage as case studies:

### ₿ Bitcoin Lab Bolivia
Bitcoin education academy with Firebase Auth, Firestore-backed course progress, a real-time BTC/BOB calculator using Binance P2P data, an interactive business map (Leaflet + BTCMap), and Maxi — a Spanish-speaking AI agent with live price awareness. Built on shared PHP hosting with zero build step.

→ [See the showcase](https://github.com/tokiopy/bitcoinlab-bolivia-showcase) · [Live site](https://btclabbolivia.com/)

### ⚡ Satoshi's Playroom
Real-time multiplayer gaming platform with a 100% Bitcoin economy. Domino, poker, and chess against live opponents via Lightning Network. Includes a full Bitcoin Academy with certifications.

→ [See the showcase](https://github.com/tokiopy/satoshis-playroom-showcase) · [Live site](https://satoshiplayroom.com/)

### 🟢 KabraCoin
Community platform for a Dominican Republic crypto project. Modern design, AI assistant integration, and automated engagement systems.

→ [Live site](https://kabracoin.com/)

---

## Tech Stack

<div align="center">

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript_ES6+-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)

</div>

### Frontend
- **HTML5 + CSS3 + Vanilla JavaScript (ES6+):** no framework, no build pipeline
- **CSS custom properties:** full design system in `:root` — colors, fonts, spacing, easing curves, max widths
- **Google Fonts:** Space Grotesk (headings), Inter (body), JetBrains Mono (labels, data, code accents)
- **No external JS dependencies:** animations, sliders, language switching, and scroll effects are all vanilla

### AI & Automation
- **n8n:** powers the live AI agent via webhook, handles session routing and response formatting
- **Fetch API:** async webhook calls with proper error handling and session ID injection

### Hosting
- **Hostinger:** static files, `.htaccess` for routing and cache headers

---

## Engineering Highlights

### 🎯 No-build architecture

The site ships as static files. No webpack, no Vite, no npm install, no CI/CD pipeline. The entire site deploys with one FTP upload. This is not a limitation — it's a deliberate match between complexity and maintenance budget.

The tradeoff: no tree-shaking, no TypeScript, no module imports. The gain: zero build failures, zero dependency CVEs in the dependency graph, and a page that loads in under 2 seconds from cold cache.

### 🌐 Flat translation system

The bilingual feature is implemented in under 80 lines of JavaScript. The translation object is a two-level keyed record (`translations.es.hero.title1`, `translations.en.chat.greeting`). Every translatable element carries a `data-translate` attribute. `setLanguage()` walks the DOM once, looks up each key, and sets `innerHTML`.

No virtual DOM diffing. No reconciliation. No hydration. One function, one loop, done.

### 💬 AI chat security model

The chat input applies two client-side guards: a 2-second rate limit between messages (`Date.now()` delta check) and a 500-character hard cap via `substring()`. Both are intentional UX constraints, not security measures — actual rate limiting and input validation happen at the n8n webhook level. Client-side guards exist to prevent accidental abuse from fast typists and to give users instant feedback on limits.

### 📱 Two-layout responsive pattern

Rather than adapting a single grid layout down to mobile, the services and automation sections ship two complete layouts: a CSS Grid version for desktop (`display: grid`) and a scroll-snap slider for mobile (`display: flex`, `overflow-x: auto`, `scroll-snap-type: x mandatory`). CSS `@media` queries toggle visibility between them. The duplication in HTML is the cost; the benefit is that neither layout is a compromise of the other.

### ⚡ IntersectionObserver scroll system

All scroll animations use a single shared observer instance with `{threshold: 0.08, rootMargin: '0px 0px -60px 0px'}`. Elements carry `.reveal` plus optional `.reveal-delay-N` classes. The observer adds `.active` when an element enters the viewport. CSS handles the transition. No JavaScript reads layout metrics, no `getBoundingClientRect()` loops, no scroll event listeners firing at 60fps.

---

## Services

| Service | What it actually means |
|---|---|
| **Web Apps & Sites** | Fast, responsive, SEO-ready. Loads under 2 seconds, ranks from day one |
| **AI Agents & Chatbots** | Trained on your data, available 24/7, integrated where your clients already are |
| **Workflow Automation** | CRM, email, invoicing, and databases connected into one automated system |
| **Mobile Apps** | Cross-platform, feels native, built to retain users |
| **Cloud & DevOps** | Scales with traffic, zero-downtime deploys, monitoring that alerts before problems hit |
| **Security** | Auth, encryption, access controls — your users' trust, intact |

---

## About the build

Designed and developed by [@tokiopy](https://github.com/tokiopy).

TokioHub is a one-engineer agency. Every technical decision, design choice, and line of code on this site was written by the same person who builds the client projects. The site is the portfolio. The portfolio is the site.

**Status:** Live at [tokiohub.com](https://tokiohub.com/)
**Location:** Santo Domingo, Dominican Republic
**Capacity:** 3 projects/month

---

## Other showcases

- **[Bitcoin Lab Bolivia](https://github.com/tokiopy/bitcoinlab-bolivia-showcase):** Bitcoin education platform for the Bolivian community
- **[Satoshi's Playroom](https://github.com/tokiopy/satoshis-playroom-showcase):** Lightning-native Bitcoin gaming platform

---

## Connect

- 🌐 **Agency:** [tokiohub.com](https://tokiohub.com/)
- 💬 **GitHub:** [@tokiopy](https://github.com/tokiopy)
- 🐦 **X:** [@tokiobtc](https://x.com/tokiobtc)
- 📱 **WhatsApp:** [+1-809-586-9999](https://wa.me/18095869999)

---

## License

This showcase repository (text and documentation) is released under the MIT License. The underlying source code is **proprietary**.

---

<div align="center">

**From Tokio With ⚡**

</div>
