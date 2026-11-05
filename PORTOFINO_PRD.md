# Product Requirements Document (PRD) — Portofino Café & Restaurant 🍕🍣☕

**Project Name:** Portofino Website & Digital Ordering System  
**Version:** 2.0 (Optimized Production)  
**Target Application:** Interactive Single-Page Application (Vanilla JavaScript SPA)  
**Live Production URL (GitHub):** [https://mohamedelsmahy15-arch.github.io/portofino-pord/](https://mohamedelsmahy15-arch.github.io/portofino-pord/)  
**Local Test Environment:** `http://localhost:5500/`  
**GitHub Repository:** `https://github.com/mohamedelsmahy15-arch/portofino-pord.git`

---

## 1. Executive Summary & Product Vision

Portofino is an established high-end restaurant and café in Alexandria, Egypt operating two flagship branches along the Mediterranean coast (Camp Caesar and Louran). 

The platform serves as an interactive, commission-free digital menu and direct ordering/reservation hub. It consolidates **3 distinct kitchens under one roof**:
1. **Portofino Kitchen:** Italian & Mediterranean dining (Pizza, Pasta, Grills, Specialty Coffee, Desserts, Shisha).
2. **Sofia’s Sushi:** An independent premium sushi brand (122 items across 15 subcategories, prepared exclusively at the Louran branch).
3. **Tahia Oriental Kitchen:** Authentic Egyptian and Alexandrian home-style dishes (Tagines, Fatteh, Mahshi).

### Key Architecture Principles
- **Zero-Commission WhatsApp Ordering:** Instead of third-party delivery platforms taking 15–25% cuts, user orders and reservations are compiled into structured, formatted WhatsApp messages sent directly to the respective branch.
- **Client-Side SPA Architecture:** Zero server-side build steps or backend databases; ultra-fast local state management (`localStorage` persistence for cart, branch, language, and theme).
- **Mobile-First & High Performance:** Fast loading (<1s FCP on 4G networks) with optimized WebP assets, H.264 FastStart streaming videos, and full Arabic RTL / English LTR support.

---

## 2. User Personas & Target Audiences

| Persona | Primary Goal | Key User Journey |
|---|---|---|
| **Mobile Dine-in / Delivery Customer** | Browse menu quickly, pick meals, and order via WhatsApp | Home -> Today’s Deals / Categories -> Cart -> WhatsApp Checkout |
| **Sushi Enthusiast** | Explore full 122-item Sofia Sushi menu with transparent tax notes | Home -> Sofia Sushi Pill -> Filter (Uramaki/Combos) -> Add to Cart |
| **Family / Parents** | Inspect kids play area, children meals, and book birthday parties | Home -> Kids Corner -> Play Video / View Gallery -> Book Occasion |
| **Café & Shisha Visitor** | View hot/cold beverage choices, shisha flavors, and evening deals | Home -> Drinks & Shisha section -> Compare coffee (Duel) -> Menu |
| **Event Planner / Group** | Reserve large tables or private occasions at Camp Caesar or Louran | Home -> Branches / Occasions -> Form -> WhatsApp Reservation |

---

## 3. Site Navigation & Information Architecture

The application uses hash-based client-side routing (`window.location.hash`):

- `#/home` — Main landing page (Hero, Quick Category Pills, Today's Deals, Chef Picks, 3 Kitchens Showcase, Drinks/Shisha, Kids Corner, Branches, Services).
- `#/menu` — Full Portofino Dining Menu with category tabs (Breakfast, Appetizers, Soup/Salads, Pizza, Pasta, Main Grills, Desserts, Kids).
- `#/sofia` — Dedicated Sofia's Sushi portal (15 categories, 122 items, 14% tax disclaimer, Louran delivery rule).
- `#/tahia` — Tahia Oriental Kitchen section (Egyptian tagines, stews, and Alexandrian specialties).
- `#/branches` — Branch selector & details (Camp Caesar & Louran addresses, Google Maps directions, phone contacts).
- `#/events` — Occasions & private celebration booking.
- `#/reserve` — Table reservation system.
- `#/kids` — Kids Corner interactive showcase, welcome film, photo gallery, kids meals menu, and party planner.
- `#/cafe` — Drinks, coffee, and shisha menu.
- `#/checkout` — Cart review, branch/service picker, and WhatsApp dispatch.

---

## 4. Functional Specifications & Testable Requirements

### 4.1 Header, Navigation & Global Controls
- **Language Switcher:** Toggles between Arabic (`ar`, RTL default) and English (`en`, LTR). All copy, prices, currency (`ج.م` / `EGP`), and layout directions must invert dynamically.
- **Theme Switcher:** Toggles between Dark Mode (default luxury theme) and Light Mode (`pf-theme` in localStorage).
- **Sticky Navigation Bar:** Remains visible on scroll with active state tracking corresponding to the current viewport section.
- **Floating WhatsApp & Quick Action Buttons:** Accessible across all views without blocking content.

### 4.2 Three Kitchens Logic & Brand Boundaries
1. **Portofino Menu:**
   - 15 main dishes + 5 lunch combo deals + 3 breakfast deals.
   - Price display includes options (`unit(it, o)`).
2. **Sofia's Sushi (Crucial Business Rule):**
   - Independent brand banner and disclaimer: *"Prices include service charge and exclude 14% VAT (printed on menu)"*.
   - Orders containing Sofia Sushi items must clearly indicate delivery origin from the Louran branch.
   - 122 items organized in 15 sub-categories with category quick-chips.
3. **Tahia Oriental Kitchen:**
   - Tagged with `brand: 'tahia'` to prevent mixing with Italian main dishes.

### 4.3 Smart Search Engine
- Search input must normalize Arabic characters:
  - `[أإآٱ]` -> `ا`
  - `[ة]` -> `ه`
  - `[ى]` -> `ي`
  - Removes diacritics / tashkeel (`[\u064B-\u0652\u0640]`).
  - Supports Arabic synonyms (e.g. searching "برجر" matches "ساندوتش", "فطار" matches "افطار/صباح").
- Filters items in real-time with multi-word `every()` matching.

### 4.4 Dynamic Deals System
- **Time-based Display:** Automatically highlights breakfast deals in morning hours and lunch combo deals in afternoon/evening hours based on client local time.
- **Counter & Slider:** Touch-enabled carousel on mobile with previous/next navigation and active item counter (`X / Y`).

### 4.5 Cart & WhatsApp Checkout System
- **Cart Operations:**
  - Add item to cart with visual confirmation badge.
  - Increment (`+`), decrement (`-`), and remove item when quantity reaches zero.
  - Item customizations (e.g., side dishes, sizes) reflected in price and description.
- **Validation Rules:**
  - Cannot proceed to checkout with an empty cart (`cart.length === 0`).
  - User must select service type: Delivery (`delivery`), Takeaway (`takeaway`), or Dine-in (`table`).
  - User must select branch: Camp Caesar (`cc`) or Louran (`lo`).
- **WhatsApp Message Generation:**
  - Formats order items cleanly: `[Item Name] x [Qty] - [Price] EGP`.
  - Calculates subtotal and applicable notes.
  - Generates deep-link URL: `https://wa.me/[Phone]?text=[EncodedMessage]`.
  - Converts numbers via international E.164 phone formatting.

### 4.6 Kids Corner Interactive System
- **Welcome Overlay:** Plays short introduction video (`portofino-kids-dark_port.mp4` / `light_port.mp4`) with skip button (`Esc` key or click).
- **Celebrations Reel:** Interactive 720x1280 video player with audio toggle control (Mute/Unmute).
- **Party Planner CTA:** Opens dedicated event booking form pre-selected for "حفلة أطفال / Kids Party".

---

## 5. Non-Functional & Performance Requirements

1. **Page Load Speed:**
   - HTML document payload < 500 KB (actual: 390 KB).
   - First Contentful Paint (FCP) < 1.0s on 4G.
   - Largest Contentful Paint (LCP) < 2.5s.
2. **Media Optimization:**
   - 63 WebP images with lazy-loading (`loading="lazy"`) and asynchronous decoding (`decoding="async"`).
   - MP4 videos encoded with `libx264` and `+faststart` (moov atom placed at file header for instant streaming).
   - Unused audio channels stripped from background loop videos.
3. **Cross-Browser & Device Compatibility:**
   - Mobile: iOS Safari 14+, Android Chrome 90+, Samsung Internet.
   - Desktop: Chrome, Safari, Edge, Firefox.
   - Screen resolutions: 360px up to 1920px without horizontal scroll.
4. **Resilience & Offline Handling:**
   - State stored in `localStorage` (`pf_state2`) persists across refreshes.
   - Range requests for video streaming handled gracefully with HTTP 206 and HTTP 416 checks.

---

## 6. Comprehensive Test Scenarios for TestSprite (Test Matrix)

| Test ID | Test Scenario | Steps to Execute | Expected Result |
|---|---|---|---|
| **TC-NAV-01** | Landing Page Render | Load `/index.html` | Page loads in <1.5s, hero title and 3 kitchen cards visible, no console errors. |
| **TC-NAV-02** | Language Switch RTL/LTR | Click "EN" button in header | Document direction changes to `ltr`, lang becomes `en`, text strings switch to English. |
| **TC-NAV-03** | Theme Switch Dark/Light | Toggle theme button in header | Root class updates, background color transitions smoothly, logo swaps between dark/light asset. |
| **TC-MENU-01** | Category Tab Switching | Navigate to `#/menu`, click "Pizza" | View scrolls to Pizza section, only pizza items displayed with correct prices. |
| **TC-MENU-02** | Sofia Sushi 122 Items & Tax Note | Navigate to `#/sofia` | 15 category chips displayed, 14% tax notice clearly visible, clicking any chip filters items. |
| **TC-SRCH-01** | Arabic Search Normalization | Type "بيتزا" in search input | Matches all pizza items regardless of hamza or spacing variations. |
| **TC-SRCH-02** | Empty Search State | Type "xyzxyz" in search input | Shows friendly empty search message with clear search CTA. |
| **TC-CART-01** | Add to Cart & Sticky Badge | Click "Add to Cart" on Chicken Dynamite Pizza | Item added to cart, floating cart badge updates count to 1, total price updates. |
| **TC-CART-02** | Cart Quantity Modification | Open cart drawer, click `+` then `-` | Quantity increments to 2 (price doubles), then decrements to 1. |
| **TC-CART-03** | Item Removal | Decrement item quantity to 0 | Item removed from cart; if cart becomes empty, empty cart placeholder appears. |
| **TC-ORD-01** | WhatsApp Checkout Flow | Add item, open checkout, fill name, click Order | Form validates, opens `https://wa.me/...` URL containing itemized order text. |
| **TC-BR-01** | Branch Selector Selection | Select "Louran" branch | Active branch highlighted, branch address updates, delivery rules updated. |
| **TC-KIDS-01** | Kids Welcome Overlay | Navigate to `#/kids` | Welcome video modal appears, clicking "Skip" or pressing `Esc` dismisses it cleanly. |
| **TC-VID-01** | Video Playback & Audio Toggle | On `#/kids`, click "Enable sound" | Video un-mutes and plays with audio; toggle updates button state to "Mute". |
| **TC-RESP-01** | Mobile Viewport Check | Set viewport to 375x667 | No horizontal overflow (`scrollWidth === innerWidth`), mobile navigation drawer works. |
