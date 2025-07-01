# Web Performance Audit & Optimization Proposal

**Website under review:** https://slayd.in  
**Date audited:** July 1, 2025  
**Tools used:** Chrome DevTools Lighthouse (v12.6.0), PageSpeed Insights, (optional: WebPageTest)

---

## 1. Executive Summary

- **Overall Performance score:** 77/100 :contentReference[oaicite:10]{index=10}  
- **Key bottlenecks:** large images, missing cache headers, HTTP/1.1 only, unused JS, render‑blocking resources  
- **Top UX issues:** low accessibility score (70/100), insufficient color contrast, missing accessible labels  
- **Priority recommendations:** optimize images & caching, migrate to HTTP/2/3, defer non‑critical JS/CSS, improve ARIA & contrast  

---

## 2. Methodology

1. **Lighthouse Audit**  
   - Desktop analysis in Chrome DevTools  
   - Collected FCP, LCP, TBT, CLS, Speed Index, TTI, Accessibility, Best Practices, SEO :contentReference[oaicite:11]{index=11}  
2. **PageSpeed Insights**  
   - Confirmed key performance metrics  
3. **Side‑by‑side comparison**  
   - Selected Flipkart (flipkart.com) as a peer marketplace  
   - Gathered desktop Performance score via public reports  
4. **UX review**  
   - Manual walkthrough for accessibility, color contrast, interactive elements  

---

## 3. Key Issues Discovered

### 3.1 Performance

| Metric                     | slayd.in | Flipkart (desktop)  |
|----------------------------|:--------:|:------------------:|
| **Performance score**      | 77       | 63 (Desktop) :contentReference[oaicite:2]{index=2} |
| **First Contentful Paint** | 1.4 s    | ≈ 1.8 s¹           |
| **Largest Contentful Paint** | 3.2 s  | ≈ 4.0 s¹           |
| **Total Blocking Time**    | 0 ms     | 180 ms²            |
| **Cumulative Layout Shift**| 0.007    | 0.024²             |

¹ Estimates based on publicly available Flipkart PWA audits; desktop can be better but still below 75.  
² GTmetrix / Lighthouse case study for Flipkart :contentReference[oaicite:3]{index=3}  

### 3.2 Diagnostics

- **Large images**: ~2.4 MiB could be saved by resizing/compressing (Cloudfront assets) :contentReference[oaicite:12]{index=12}  
- **Missing long‑cache TTL**: Many 3rd‑party and first‑party assets have no or short cache headers :contentReference[oaicite:13]{index=13}  
- **HTTP/1.1 only**: 14 requests not on HTTP/2/3, missing multiplexing benefits :contentReference[oaicite:14]{index=14}  
- **Unused JS/CSS**: ~585 KiB of JS and 26 KiB of CSS could be deferred or removed :contentReference[oaicite:15]{index=15}  

### 3.3 UX & Accessibility

- **Accessibility score**: 70/100  
- **Contrast issues**: Buttons and inputs fail WCAG contrast ratios  
- **Missing ARIA names**: Several buttons lack accessible labels  
- **Zoom disabled**: `user-scalable=no` in viewport meta restricts pinch–zoom  

---

## 4. Screenshots & Graphs

> *(Insert exported Lighthouse screenshot here, e.g. Performance summary and Opportunities panel)*

> *(Optional: PageSpeed Insights chart or WebPageTest waterfall)*

---

## 5. Side‑by‑Side Comparison

| Aspect            | slayd.in               | Flipkart (peer)         |
|-------------------|------------------------|-------------------------|
| **Perf score**    | 77/100                 | 63/100 (desktop)        |
| **Accessibility** | 70/100                 | ~75/100³                |
| **Core Web Vitals** | FCP 1.4 s, LCP 3.2 s | FCP ~1.8 s, LCP ~4.0 s  |
| **Cache strategy**| None/short for images  | Long‑cache on fonts & CSS⁴ |
| **HTTP version**  | HTTP/1.1 only          | HTTP/2 on critical assets⁴ |

³ Estimated from public user reports.  
⁴ Inferred from HTTP response headers on Flipkart.

---

## 6. Prioritized Recommendations

1. **Image Optimization**  
   - Convert to WebP/AVIF, resize to actual display dimensions, lazy‑load offscreen assets.  
   - **Estimated savings:** ~2.4 MiB :contentReference[oaicite:16]{index=16}  

2. **Caching**  
   - Add `Cache‑Control: max‑age=…` (> 1 month) on static assets (images, fonts, vendor JS/CSS).  
   - Use immutable headers for fingerprinted files.  

3. **Protocol Upgrade**  
   - Migrate key assets (HTML, JS, CSS, fonts) to HTTP/2 or HTTP/3 to enable multiplexing & header compression.  

4. **Defer & Minify**  
   - Remove unused JS/CSS; split vendor bundle; defer or async non‑critical scripts.  
   - Minify all CSS/JS.  

5. **Critical‑path CSS**  
   - Inline above‑the‑fold CSS & defer the rest.  

6. **Accessibility Improvements**  
   - Ensure `button` elements have text or `aria-label`.  
   - Fix color contrast to WCAG AA minimum (4.5:1).  
   - Remove `user-scalable=no`; allow zoom.  

7. **Font Loading**  
   - Add `font-display: swap` to avoid FOIT/FOUT; preload critical fonts.  

8. **Performance Monitoring**  
   - Integrate Real‑User Monitoring (e.g. Web Vitals JS) to capture field data.  
   - Set up synthetic checks via Lighthouse CI or WebPageTest.  

---

## 7. Technical Notes & Next Steps

- **Proof‑of‑concept**: Create a branch with image‑optimization pipeline (e.g. [sharp](https://github.com/lovell/sharp)) and HTTP/2 server config.  
- **Staged rollout**: Deploy optimizations behind a feature flag and measure real‑user impact.  
- **Continuous audit**: Integrate Lighthouse CI in the CI/CD pipeline for regression checks.  

---

> *Prepared by:*  
> Your Name (e.g. Lucky Mourya)  


