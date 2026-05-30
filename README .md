# Studex – Full-Stack Integration & Performance Optimization

Task 3 submission. A frontend application connected to a live REST API with performance optimization and accessibility best practices.

---

## Live Demo
🔗 [View Live Site](https://UbahJoy.github.io/studex-api-integration/)

## GitHub Repository
🔗 [View Code](https://github.com/UbahJoy/studex-api-integration)

---

## Tech Stack

| Technology | Purpose |
|---|---|
| HTML5 (Semantic) | Accessible page structure |
| CSS3 | Styling, animations, responsive layout |
| Vanilla JavaScript | API fetching, DOM manipulation, state management |
| JSONPlaceholder API | Free REST API for posts and comments data |
| localStorage | Persisting saved notes across sessions |
| IntersectionObserver API | Lazy loading implementation |

---

## API Used

**JSONPlaceholder** — https://jsonplaceholder.typicode.com

Endpoints used:
- `GET /posts` — Fetches all 100 study notes
- `GET /posts/:id/comments` — Fetches comments for a specific note

---

## Features Implemented

### Data Fetching & Dynamic Rendering
- Fetches 100 posts from JSONPlaceholder API on page load
- Displays posts as study note cards dynamically rendered from API data
- Shows loading spinner while data is being fetched
- Shows error state with retry button if API call fails
- Fetches comments dynamically when a note is opened

### Search & Filter
- Real-time search filtering by title and body text
- Filter between All Notes and Saved Notes
- Sort notes A→Z or Z→A
- Live results count updates as filters change

### Save Feature
- Save/unsave notes with bookmark button
- Saved state persists using localStorage (no data lost on page refresh)

### Modal / Detail View
- Click any note to open a full detail modal
- Modal dynamically fetches and displays comments from API
- Keyboard accessible (Escape to close)

### Pagination
- Shows 9 notes at a time
- Load More button fetches next batch without page reload

---

## Performance Optimizations

### 1. Debounced Search
Search input waits 300ms after the user stops typing before filtering. This prevents the app from re-rendering on every single keystroke, reducing unnecessary work.

```javascript
let searchTimeout;
input.addEventListener('input', function() {
  clearTimeout(searchTimeout);
  searchTimeout = setTimeout(() => { renderNotes(); }, 300);
});
```

### 2. Lazy Loading with IntersectionObserver
Images only load when they enter the viewport, not all at once. This reduces initial page load time.

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('loaded');
      observer.unobserve(entry.target);
    }
  });
});
```

### 3. Pagination (Load More)
Only 9 notes render at a time instead of all 100. This keeps the DOM small and the page fast.

### 4. localStorage Caching
Saved notes are stored in localStorage so the app doesn't need to re-fetch data to remember user preferences.

### 5. CSS Animations over JavaScript
All animations use CSS (transform, opacity, animation) instead of JavaScript, which runs on the GPU and doesn't block the main thread.

### 6. Font Preconnect
```html
<link rel="preconnect" href="https://fonts.googleapis.com"/>
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin/>
```
This tells the browser to connect to Google Fonts early, reducing font load time.

---

## Accessibility Considerations

### 1. Skip Link
A hidden "Skip to main content" link appears on focus, allowing keyboard users to bypass navigation.

### 2. Semantic HTML
Proper HTML elements used throughout:
- `<nav>` for navigation
- `<main>` for main content
- `<header>` for page header
- `<article>` for note cards
- `<footer>` for footer
- `<section>` for grouped content

### 3. ARIA Labels
Every interactive element has descriptive ARIA labels:
- `aria-label` on buttons and inputs
- `aria-pressed` on toggle buttons (filter, bookmark)
- `aria-live` on dynamic content (results count)
- `aria-modal` on the modal dialog
- `role="status"` on loading/empty states
- `role="alert"` on error state

### 4. Keyboard Navigation
- All buttons and interactive elements are focusable
- Modal closes with Escape key
- Note cards can be opened with Enter key
- Visible focus outlines on all focusable elements

### 5. Focus Management
When modal opens, focus moves to the modal. Body scroll is locked to prevent background scrolling.

---

## Folder Structure

```
studex-api-integration/
├── index.html     ← Full application
└── README.md      ← This documentation
```

---

## Key Decisions

**1. JSONPlaceholder as the API**
It's free, requires no API key, and returns structured data (posts + comments) that maps naturally to a study notes concept.

**2. Vanilla JavaScript over React**
Since the project is deployed as a static HTML file without a build tool, vanilla JS keeps things simple and performant without a framework overhead.

**3. localStorage for saved notes**
Saves persist across sessions without needing a backend, keeping the app self-contained.

**4. Debounce on search**
Without debounce, filtering 100 items on every keystroke would cause jank. 300ms debounce keeps the UI smooth.

---

## Author
Built by Ubah Joy — Week 2 Task 3, Frontend Development Internship
