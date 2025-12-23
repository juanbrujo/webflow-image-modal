# Webflow Image Modal

> 🎉 **Free & Open Source** - Beautiful, responsive image zoom modal for Webflow galleries. Zero dependencies, pure vanilla JavaScript. Copy-paste solution that works out-of-the-box.

![Webflow Image Modal](https://i.imgur.com/UE31GvE.jpeg)

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GitHub Stars](https://img.shields.io/github/stars/juanbrujo/webflow-image-modal?style=social)](https://github.com/juanbrujo/webflow-image-modal)

---

## ✨ Features

- **🎯 Dead Simple** - Copy CSS and JavaScript. No dependencies, no build steps. Works immediately on your Webflow site.
- **📱 Fully Responsive** - Adapts perfectly to all devices. 70vw on desktop, 90vw on mobile. Touch-friendly interactions.
- **♿ Accessible** - WCAG compliant with proper ARIA labels, keyboard navigation (Escape key), and focus management.
- **🎨 Elegant Design** - Modern dark overlay with backdrop blur effect. Minimalist close button. Premium feel.
- **⚡ Zero Performance Impact** - Vanilla JavaScript with no external libraries. Minimal CSS. Lightning-fast load times.
- **🔓 Open Source** - MIT License. Contribute, fork, and customize. Community-driven development.

---

## 🚀 Quick Start

### 1. Add Gallery Wrapper Class

Add `webflow-gallery` as a CSS class to your gallery wrapper element in Webflow.

### 2. Copy the CSS Styles

Go to your **Webflow site settings → Custom Code → Head Code** and paste:

```html
<style>
  /* Webflow Image Modal */
  /* Free, Open Source Gallery Solution for Webflow */
  /* https://github.com/juanbrujo/webflow-image-modal */

  .webflow-gallery img {
    cursor: zoom-in;
  }

  .modal-overlay {
    position: fixed;
    inset: 0;
    display: none;
    align-items: center;
    justify-content: center;
    background: rgba(0, 0, 0, 0.7);
    backdrop-filter: saturate(130%) blur(2px);
    z-index: 9999;
    padding: 24px;
    cursor: zoom-out;
  }

  .modal-overlay.open {
    display: flex;
  }

  .modal-content {
    position: relative;
    display: inline-block;
    box-shadow: 0 12px 40px rgba(0, 0, 0, 0.35);
    overflow: visible;
  }

  .modal-img {
    width: auto;
    height: auto;
    max-width: 70vw;
    max-height: 90vh;
    display: block;
    background: transparent;
  }

  .modal-close {
    position: absolute;
    top: 0;
    right: -20px;
    transform: translateY(-50%);
    appearance: none;
    border: none;
    border-radius: 999px;
    padding: 8px 12px;
    font-size: 16px;
    line-height: 1;
    background: rgba(255, 255, 255, 0.9);
    color: #111;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
    cursor: pointer;
    transition: background 0.2s ease, transform 0.1s ease;
  }

  @media (max-width: 767px) {
    .modal-img {
      max-width: 90vw;
    }
    .modal-close {
      top: 8px;
      right: 8px;
      transform: none;
    }
  }
</style>
```

### 3. Add the JavaScript

Go to your **Webflow site settings → Custom Code → Footer Code** and paste:

```html
<script>
  (function () {
    // Create modal markup dynamically to avoid modifying Webflow HTML
    const modalHTML = `
      <div id="modal-overlay" class="modal-overlay" inert>
        <div class="modal-content" role="dialog" aria-modal="true" aria-label="Imagen ampliada">
          <button class="modal-close" type="button" aria-label="Cerrar">✕</button>
          <img class="modal-img" alt="" />
        </div>
      </div>
    `;
    document.body.insertAdjacentHTML('beforeend', modalHTML);

    // Get references after insertion
    const overlay = document.getElementById('modal-overlay');
    const modalImg = overlay.querySelector('.modal-img');
    const btnClose = overlay.querySelector('.modal-close');
    const gallery = document.querySelector('.webflow-gallery');

    let lastFocused = null;

    function openModal(fromImg) {
      // Copy essential attributes to preserve quality and alt text
      modalImg.src = fromImg.currentSrc || fromImg.src;
      if (fromImg.srcset) modalImg.srcset = fromImg.srcset; else modalImg.removeAttribute('srcset');
      if (fromImg.sizes) modalImg.sizes = fromImg.sizes; else modalImg.removeAttribute('sizes');
      modalImg.alt = fromImg.alt || '';

      lastFocused = document.activeElement;

      overlay.classList.add('open');
      overlay.removeAttribute('inert');
      document.body.style.overflow = 'hidden';
      // Move focus to close for accessibility
      btnClose.focus({ preventScroll: true });
    }

    function closeModal() {
      overlay.classList.remove('open');
      overlay.setAttribute('inert', '');
      document.body.style.overflow = '';
      // Restore focus
      if (lastFocused && typeof lastFocused.focus === 'function') {
        lastFocused.focus({ preventScroll: true });
      }
    }

    // Open on image click (delegate within the gallery)
    const gallerySelector = gallery;
    if (gallerySelector) {
      gallerySelector.addEventListener('click', function (e) {
        const target = e.target;
        if (target && target.tagName === 'IMG') {
          openModal(target);
        }
      });
    }

    // Close when clicking outside the image (on overlay area)
    overlay.addEventListener('click', function (e) {
      if (e.target === overlay) {
        closeModal();
      }
    });

    // Close button
    btnClose.addEventListener('click', closeModal);

    // Escape key closes
    document.addEventListener('keydown', function (e) {
      if (e.key === 'Escape' && overlay.classList.contains('open')) {
        closeModal();
      }
    });
  })();
</script>
```

### 4. Publish Your Site

Publish your Webflow site to make the changes live.

### 5. That's It! 🎉

1. Click any image in your gallery
2. Watch the beautiful modal appear
3. Close with button, ESC key, or click outside

---

## 💡 How It Works

This solution dynamically creates the modal structure, so you **don't need to modify your Webflow HTML**. Just add CSS styles and JavaScript code to your site's custom code sections.

**The Flow:**
1. **Add Gallery Wrapper Class** - Add `webflow-gallery` as CSS class to the gallery wrapper
2. **Copy CSS Styles** - Add the modal styles to your Webflow site's custom head code
3. **Add JavaScript** - Paste the vanilla JS that handles clicks, opens/closes modal, and manages focus
4. **Publish** - Publish your Webflow site to make the changes live
5. **Click Images** - Click any image in your gallery and watch the elegant modal appear instantly
6. **Customize (Optional)** - Tweak colors, sizes, and animations to match your brand in seconds

---

## 📖 Example

Check out the [live example](https://juanbrujo.github.io/webflow-image-modal/) to see the modal in action with placeholder images.

---

## 🎨 Customization

You can easily customize the modal appearance by modifying the CSS variables:

- **Modal size**: Change `max-width: 70vw` (desktop) and `max-width: 90vw` (mobile)
- **Overlay color**: Modify `background: rgba(0, 0, 0, 0.7)`
- **Close button position**: Adjust `right: -20px` or mobile overrides
- **Animation**: Add transitions to `.modal-overlay.open` class

---

## 🔧 Browser Support

- ✅ Chrome/Edge (latest)
- ✅ Firefox (latest)
- ✅ Safari (latest)
- ✅ Mobile browsers (iOS Safari, Chrome Mobile)

Uses modern CSS (`inset`, `backdrop-filter`) and JavaScript (`inert` attribute). Gracefully degrades on older browsers.

---

## 🤝 Contributing

Contributions are welcome! Feel free to:

- Report bugs via [GitHub Issues](https://github.com/juanbrujo/webflow-image-modal/issues)
- Submit pull requests with improvements
- Share your implementations and use cases
- Suggest new features

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**Jorge Epuñan**

- GitHub: [@juanbrujo](https://github.com/juanbrujo)

---

## ⭐ Show Your Support

If this project helped you, please give it a ⭐️ on [GitHub](https://github.com/juanbrujo/webflow-image-modal)!

---

Made with ❤️ for the Webflow community
