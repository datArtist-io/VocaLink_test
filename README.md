# VocaLink (3D landing page)

Landing page for VocaLink, a real-time voice translator for Discord, built around an interactive 3D koi pond. The whole page sits on top of a night-time Three.js scene: a koi follows your cursor, clicks send ripples across the water, and greetings in ten languages float above the pond and turn into English when the koi swims under them.

Live site: [vocalinktest.vercel.app](https://vocalinktest.vercel.app)

The Discord bot itself lives in a separate repository: [datArtist-io/VocaLink-discord-bot](https://github.com/datArtist-io/VocaLink-discord-bot).

## What is on the page

The site is one file, `index.html`, with its CSS, shaders and JavaScript inline. The content scrolls over the pond in frosted-glass panels, with open stretches of water between some sections. From top to bottom:

1. Hero: "Every voice in the channel, understood by everyone", buttons to add VocaLink to Discord and to try the demo, and a mock `#voice-channel` with Italian, French and Portuguese lines translated into English.
2. Headline facts: 10 languages auto-detected with live switching, captions seconds after a pause, audio not stored once translated, and voice cloning only with the speaker's consent.
3. How it works (`#how`): four steps, from someone talking to the translation showing up as a caption or spoken audio.
4. Try it: a translation demo, described below.
5. Features: automatic detection across 10 languages (English, Spanish, French, German, Hindi, Russian, Portuguese, Japanese, Italian and Dutch), fast captions, opt-in voice cloning, and no separate app to install.
6. Included: what every tier gets, with cloned-voice dubbing marked as Pro only.
7. Setup: four steps to install the bot, and its slash commands: `/translate start`, `/translate stop`, `/translate enroll-voice`, `/translate remove-voice` and `/translate voices`.
8. Pricing (`#pricing`), per Discord server: Free ($0, 60 minutes a month), Starter ($5, 5 hours a month) and Pro ($12, unlimited minutes, cloned-voice dubbing, priority support).
9. FAQ (`#faq`): storage, languages, admin rights, voice enrollment, what happens when a subscription ends, and the bot's Discord permissions.
10. Closing call to action and footer.

The "Add to Discord", "Log in" and footer links currently point to sections on the same page.

## The 3D scene

The scene is drawn with Three.js and a set of custom `ShaderMaterial` shaders:

- a sky dome with stars and a moon, and fog over the water
- a pond bed lit by moving caustics
- a koi that follows the pointer and swims to wherever you click on the water
- a water surface that ripples where you click, and again under the koi each time a translation lands in the demo
- lily pads, a lotus, reeds, fireflies and drifting mist
- ten floating greetings ("hello", "hola", "bonjour" and so on) that switch to English as the koi passes beneath them

The camera follows the scroll position. When the browser asks for reduced motion, CSS animations are turned off and the scene runs slower.

## The translation demo

The demo takes a typed phrase, or one of the example chips, and shows it in a live feed with the detected language and a translation into one of the ten target languages.

- Live translation of any text runs only when the page is opened inside Claude, where it uses the Claude artifact runtime (`window.claude`) to detect the language and translate. The same runtime enables the "Export transcript" button.
- Opened anywhere else, including the live site, the example phrases translate into English from built-in examples, and other text shows a note explaining this.
- Where the browser supports speech recognition, a "Speak instead" button fills the phrase from the microphone.
- "Read translations aloud" uses the browser's speech synthesis.

Nothing typed into the demo is stored.

## Tech

- Plain HTML, CSS and JavaScript in a single file, with no build step and no package manager
- [Three.js r128](https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js) from cdnjs, with custom GLSL shaders for the water, caustics and sky
- Google Fonts: Fraunces, Manrope and JetBrains Mono
- Browser APIs: `SpeechRecognition`, `speechSynthesis`, canvas textures for the floating words
- Claude artifact runtime, optional, for the live translation demo

## Run it locally

Open `index.html` in a browser. It needs an internet connection for Three.js and the fonts. If you want the microphone button to work, serve the folder over localhost instead, because browsers restrict the microphone on `file://` pages:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Deployment

The site is deployed on Vercel as a static site at [vocalinktest.vercel.app](https://vocalinktest.vercel.app). There is no build command or Vercel config; Vercel serves `index.html` as is.
