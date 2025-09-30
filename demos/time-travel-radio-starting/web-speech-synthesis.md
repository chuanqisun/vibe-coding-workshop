# Interruptible Web Speech Synthesis Implementation Guide

## Core Concepts

### 1. Speech Synthesis API Basics

```javascript
const utterance = new SpeechSynthesisUtterance(text);
speechSynthesis.speak(utterance);
```

### 2. State Management

```javascript
let speechTimeout; // For debouncing rapid changes
let currentSpeech; // Current SpeechSynthesisUtterance instance
```

## Implementation Pattern

### Core Variables

```javascript
let speechTimeout; // For debouncing rapid changes
let currentSpeech; // Current SpeechSynthesisUtterance instance
```

### Feature Detection

```javascript
if (!("speechSynthesis" in window)) {
  // Handle unsupported browsers
  status.textContent = "Speech synthesis not supported in this browser.";
  return;
}
```

### Speech Function Implementation

```javascript
function speak(text, additionalParams = {}) {
  // Create new utterance
  currentSpeech = new SpeechSynthesisUtterance(text);

  // Apply voice settings (rate, pitch, volume)
  const settings = getVoiceSettings(additionalParams);
  currentSpeech.rate = settings.rate;
  currentSpeech.pitch = settings.pitch;

  // Set up event handlers
  currentSpeech.onstart = () => {
    status.textContent = "Speaking...";
    playBtn.disabled = true;
    stopBtn.disabled = false;
  };

  currentSpeech.onend = () => {
    status.textContent = "Finished";
    resetButtons();
  };

  // Start speech synthesis
  speechSynthesis.speak(currentSpeech);
}
```

## Key Implementation Strategies

### 1. Debouncing Rapid Changes

```javascript
function updateContent(newValue) {
  // Cancel any existing speech
  if (currentSpeech) {
    speechSynthesis.cancel();
  }

  // Clear existing timeout
  clearTimeout(speechTimeout);

  // Update UI immediately
  updateUI(newValue);

  // Debounce speech synthesis
  speechTimeout = setTimeout(() => {
    speak(getContentForValue(newValue), newValue);
  }, 200); // 200ms delay
}
```

### 2. Proper Interruption Handling

```javascript
function stopSpeech() {
  if (currentSpeech) {
    speechSynthesis.cancel();
  }
  resetButtons();
  status.textContent = "Stopped";
}
```

### 3. Button State Management

```javascript
function resetButtons() {
  playBtn.disabled = false;
  stopBtn.disabled = true;
}
```

### 4. Voice Customization

```javascript
function getVoiceSettings(contextParam) {
  // Example: Historical context affects voice characteristics
  if (contextParam < 1950) return { rate: 0.8, pitch: 1.2 };
  if (contextParam <= 1980) return { rate: 0.9, pitch: 1.1 };
  return { rate: 1, pitch: 1 };
}
```

## Event Handling Patterns

### User Input Events

```javascript
// Slider input with debouncing
slider.addEventListener("input", (e) => {
  updateContent(e.target.value);
});

// Manual play button
playBtn.addEventListener("click", () => {
  speak(getCurrentContent(), getCurrentContext());
});

// Stop button
stopBtn.addEventListener("click", () => {
  stopSpeech();
});
```

### Keyboard Shortcuts

```javascript
document.addEventListener("keydown", (e) => {
  if (e.ctrlKey && e.key === "Enter") {
    playBtn.click();
  }
});
```

## Best Practices

### 1. Always Clean Up Previous Speech

```javascript
if (currentSpeech) {
  speechSynthesis.cancel();
}
```
