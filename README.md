# YouTube Video Background Wrapper

A fully-featured YouTube video background implementation with autoplay, looping, and fade-in effects. Perfect for creating cinematic landing pages.

## Features

- ✅ Autoplay with mute (browser-compliant)
- ✅ Seamless looping
- ✅ Responsive aspect ratio (covers entire viewport)
- ✅ Fade-in animation to hide title card flash
- ✅ Darkening overlay for text readability
- ✅ Disabled user interaction (no accidental pauses)
- ✅ Mobile-friendly (prevents fullscreen on iOS)

---

## HTML

```html
<div class="video-background-wrapper">
  <!-- 
      OVERLAY: 
      Sits on top of the video to make it darker (opacity-60) 
      so your website text is readable over the moving background.
  -->
  <div class="video-overlay opacity-60"></div>
  
  <!-- 
      IFRAME CONFIGURATION:
      
      1. src=".../h7MYJghRWt0": The Video ID.
      2. ?autoplay=1: Starts video automatically.
      3. &mute=1: REQUIRED. Browsers block autoplay if there is sound.
      4. &start=3: Skips the first 3 seconds of the video.
      5. &controls=0: Hides the bottom player bar (pause/play).
      6. &loop=1: Loops the video when finished.
      7. &playlist=h7MYJghRWt0: REQUIRED for the loop to work (must list ID again).
      8. &modestbranding=1: Minimizes YouTube logos.
      9. &iv_load_policy=3: Hides annotations/popups.
      10. &playsinline=1: Prevents iOS from forcing the video to fullscreen.
  -->
  <iframe 
    id="youtube-player" 
    class="video-iframe"
    src="https://www.youtube.com/embed/h7MYJghRWt0?autoplay=1&mute=1&controls=0&loop=1&playlist=h7MYJghRWt0&start=9&showinfo=0&rel=0&iv_load_policy=3&modestbranding=1&playsinline=1" 
    title="Background Video" 
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" 
    allowfullscreen
    tabindex="-1">
  </iframe>
</div>
```

---

## CSS

```css
/* --- CONTAINER --- */
/* This holds the video behind your content */
.video-background-wrapper {
  position: fixed; /* Keeps video stuck to screen, doesn't scroll away */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden; /* Hides parts of video that overflow the screen */
  z-index: -1; /* Puts the video BEHIND all other content */
  
  /* Background color is black so the user doesn't see white 
     while the video is loading/buffering */
  background-color: black; 
}

/* --- IFRAME (THE VIDEO ITSELF) --- */
.video-iframe {
  position: absolute;
  top: 50%;
  left: 50%;
  
  /* 
     ASPECT RATIO HACK (16:9 Video):
     This forces the video to zoom in and fill the screen (like object-fit: cover).
     
     1. width/height 100vw/vh ensures it covers the viewport basics.
     2. min-width: 177.77vh (16/9 = 1.77) Ensures width is enough for tall screens.
     3. min-height: 56.25vw (9/16 = 0.56) Ensures height is enough for wide screens.
  */
  width: 100vw;
  height: 100vh;
  min-width: 177.77vh; 
  min-height: 56.25vw; 
  
  /* Centers the video */
  transform: translate(-50%, -50%);
  border: none;
  
  /* 
     CRITICAL: DISABLES INTERACTION
     1. Prevents clicking Pause/Play.
     2. Prevents clicking Ads.
     3. Prevents the "Watch on YouTube" hover title.
  */
  pointer-events: none; 
  
  /* --- FADE IN ANIMATION --- */
  /* This logic hides the "Title Card" flash at the beginning */
  
  opacity: 0; /* 1. Start invisible */
  
  /* 2. Run animation 'videoFadeIn' */
  /* Duration: 2s (Takes 2 seconds to slowly appear) */
  /* Delay: 1s (Waits 1 second before starting. This 1s covers the Title Card flash) */
  /* Forwards: Stays visible (opacity: 1) after animation ends */
  animation: videoFadeIn 1s ease-in-out 2s forwards;
}

/* --- OVERLAY --- */
/* Darkens the video */
.video-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: black; 
  z-index: 1; /* Sits on top of video, but behind your text */
}

/* Utility to control darkness (0.6 = 60% black) */
.opacity-60 {
  opacity: 0.6;
}

/* --- KEYFRAMES --- */
/* Defines the fade-in motion */
@keyframes videoFadeIn {
  0% {
    opacity: 0; /* Fully invisible */
  }
  100% {
    opacity: 1; /* Fully visible */
  }
}
```

---

## Customization

### Change Video
Replace `h7MYJghRWt0` with your YouTube video ID (appears in both `src` and `playlist` parameters).

### Adjust Darkness
Modify the `opacity-60` class value (0.0 = transparent, 1.0 = fully black).

### Change Start Time
Update `&start=9` to skip different amounts of seconds.

### Adjust Fade Duration
Change `animation: videoFadeIn 1s ease-in-out 2s forwards`:
- First `1s`: Fade duration
- Second `2s`: Delay before fade starts

---

## Browser Compatibility

- ✅ Chrome/Edge
- ✅ Firefox
- ✅ Safari (including iOS)
- ⚠️ Autoplay requires muted audio on all modern browsers

---

Unfortunately, **we cannot guarantee zero ads with YouTube embeds**. YouTube can inject ads into any video at their discretion, even if the creator hasn't monetized it.

## Solutions to Avoid Ads

### Option 1: Self-Host the Video (Recommended)
Download and host the video file yourself using HTML5 `<video>`:

```html
<div class="video-background-wrapper">
  <div class="video-overlay opacity-60"></div>
  
  <video 
    class="video-iframe"
    autoplay 
    muted 
    loop 
    playsinline
    preload="auto">
    <source src="your-video.mp4" type="video/mp4">
    <source src="your-video.webm" type="video/webm">
  </video>
</div>
```

**CSS stays the same**, just update `.video-iframe` if needed:

```css
.video-iframe {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100vw;
  height: 100vh;
  min-width: 177.77vh; 
  min-height: 56.25vw;
  transform: translate(-50%, -50%);
  object-fit: cover; /* Simpler than aspect ratio hack */
  pointer-events: none;
  opacity: 0;
  animation: videoFadeIn 1s ease-in-out 2s forwards;
}
```

**Pros:**
- ✅ Zero ads guaranteed
- ✅ Faster loading (no YouTube overhead)
- ✅ More control (no YouTube branding/popups)

**Cons:**
- ❌ Uses your hosting bandwidth
- ❌ Need to optimize video file size
- ❌ You're responsible for video hosting costs

---

### Option 2: Use Ad-Free Video Hosting
Services that don't inject ads:
- **Vimeo Plus/Pro** - No ads on embedded videos (requires paid plan)
- **Bunny.net** - Video CDN with no ads
- **Cloudflare Stream** - Enterprise video hosting

---

### Option 3: Creative Commons Videos
Use stock video sites with permissive licenses:
- **Pexels Videos** (free, no ads)
- **Pixabay Videos** (free, no ads)
- **Coverr** (free background videos)

Download and self-host these for guaranteed ad-free experience.

---

## YouTube Limitations

With YouTube embeds, you **cannot**:
- Disable ads via URL parameters
- Control whether ads appear (even on your own videos)
- Prevent pre-roll, mid-roll, or overlay ads

Even videos that currently have no ads could have them added later by YouTube's algorithm or policy changes.

---

**Bottom line:** For a professional, ad-free background video, self-hosting is the only reliable solution.
