---
applyTo: "**/web-speech-api/**"
---

## Spec for Web Speech API Application

Build a minimal web application that demonstrates the Web Speech API with the following requirements:

- Use vanilla HTML, bare minimum CSS, and JavaScript only (no external libraries or frameworks)
- Implement speech recognition using the Web Speech API's continuous recognition mode
- Create a button that users can hold down to start voice recording
- Also support holding the spacebar as an alternative to the button
- Accumulate and display all transcribed text continuously while the button/spacebar is held down
- Clear the text the next time user starts recording.
- When the user releases the button or spacebar, stop recording and use the Speech Synthesis API to play back the complete accumulated recognized text
- Display the recognized text on the page as it's being transcribed
- Handle browser compatibility and provide appropriate error messages
- Include basic styling for a clean, functional interface
