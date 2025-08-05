# ⚠️ [Archived] ⚠️ Simple Surprise - "Happy Birthday, Dylan!"

> [!WARNING]
> **This repository is archived and no longer maintained.**
>
> - **Archived**: 2025-08-05 by [@KemingHe](https://github.com/KemingHe)
> - **Archive reason**: deployed and shown to Dylan on his birthday on 2025-08-05

A single-file interactive birthday card featuring responsive animations, configurable content, and zero dependencies.

## Dev Onboarding Guide

### Architecture Overview

- **Single File Design**: Everything lives in `index.html` - no build process, bundlers, or external dependencies.
- **Config-Driven**: All behavior controlled by the `config` object - modify this, not the code logic.

```javascript
const config = {
    confettiEmojis: ['🎉', '🎂', '🥳', '🎈'],  // Emoji pool for confetti
    confettiCount: { small: 50, medium: 100, large: 150 },  // Responsive counts
    confettiSpawnDuration: 2500,             // Time to spawn all confetti
    numLightBeams: 7,                        // Radial light beam count
    beamAnimation: { 
        totalDuration: 2000,                 // Sequential beam activation time
        length: 360,                         // Beam height (px)
        width: 90                            // Beam width (px)
    },
    rockingAnimation: { 
        maxDegrees: 10,                      // Max rotation angle
        maxSpeed: 0.25,                      // Animation speed multiplier
        acceleration: 0.001                  // Speed ramp-up rate
    },
    book: {                                  // Content payload
        title: "Agentic Artificial Intelligence",
        subtitle: "Harnessing AI Agents to Reinvent Business, Work and Life",
        author: "Pascal Bornet, et al.",
        description: "A research-backed non-technical roadmap...",
        recommender: "Keming He"
    }
};
```

### Code Organization

- **CSS Structure**: Organized with clear section comments and CSS custom properties for dynamic values.
- **JavaScript Modules**: Functions are modular and well-documented with JSDoc comments.
- **State Management**: Global variables track animation states (`animationFrameId`, `beams`, `beamTimeouts`).

## Key Patterns

### Dynamic CSS Variables

Config values control styling through CSS custom properties:

```javascript
// JS sets variables
root.style.setProperty('--beam-length', `${config.beamAnimation.length}px`);
```

```css
/* CSS uses variables */
.light-beam { height: var(--beam-length); }
```

### Fade Transitions

Smooth content swapping without layout shifts:

```javascript
presentContainer.addEventListener('click', () => {
    mainContainer.style.opacity = '0';
    setTimeout(() => {
        presentContainer.style.display = 'none';
        finalMessageContainer.style.display = 'flex';
        mainContainer.style.opacity = '1';
    }, 500);
}, { once: true });
```

### Event-Driven Animations

#### Hover System

- `mouseenter`: Shuffles beam array → sequential activation with delays → starts physics-based rocking
- `mouseleave`: Clears all timeouts → deactivates beams → stops rocking animation

#### Click Interaction

- Single-use event (`{ once: true }`) to prevent multiple triggers
- Initiates global fade transition + confetti shower simultaneously

#### Confetti Physics

- Responsive count based on viewport width (mobile/tablet/desktop)
- Spawned over `confettiSpawnDuration` with random delays
- Each piece auto-removes after fall animation completes (prevents memory leaks)

## Core Functions Reference

### Animation Controllers

```javascript
startRockingAnimation()     // Physics-based pendulum motion with acceleration
stopRockingAnimation()      // Cancels animation frame + resets position
triggerConfettiShower()     // Spawns responsive confetti with staggered timing
```

### Setup Functions

```javascript
applyDynamicStyles()        // Maps config values to CSS custom properties
setupLightBeams()           // Creates radial beam elements (360° / numLightBeams)
populateFinalMessage()      // Injects book data into HTML template
getConfettiCount()          // Returns responsive confetti count
```

### Utilities

```javascript
shuffleArray()              // Fisher-Yates shuffle for beam randomization
```

## Responsive Design Features

- **Typography**: Uses `clamp()` for fluid scaling (heading: 2rem-3.5rem, present: 6rem-10rem)
- **Confetti Counts**: Breakpoint-based scaling (768px, 1024px thresholds)
- **Layout**: Flexbox centering with `100dvh` for mobile viewport handling

## Dev Patterns to Understand

### CSS Custom Property Bridge

```javascript
// Config drives styling through CSS variables
root.style.setProperty('--beam-length', `${config.beamAnimation.length}px`);
```

### Animation State Cleanup

```javascript
// Prevents animation frame leaks
if (animationFrameId) cancelAnimationFrame(animationFrameId);
// Clears timeout arrays
beamTimeouts.forEach(clearTimeout);
```

### Memory Management

```javascript
// Confetti self-cleanup prevents DOM bloat
setTimeout(() => confetti.remove(), timeToRemove);
```

## Customization Patterns

- **Content Swap**: Replace `book` object + update `populateFinalMessage()` + change emoji
- **Animation Tuning**: Modify config values (no code changes required)
- **New Interactions**: Follow event listener pattern with cleanup logic

---

_This README is a living document, update it as you edit [`index.html`](./index.html)._
