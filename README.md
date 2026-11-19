# VocaLink website

The website for VocaLink, live voice translation for calls. The home page is a working translator that runs in the browser, set over a night-time Three.js koi pond. The same page is where people add the VocaLink bot to their Discord server and choose a plan.

Live site: [vocalinktest.vercel.app](https://vocalinktest.vercel.app)

The Discord bot lives in [datArtist-io/VocaLink-discord-bot](https://github.com/datArtist-io/VocaLink-discord-bot); its [PRODUCT.md](https://github.com/datArtist-io/VocaLink-discord-bot/blob/main/discord-voice-translator/docs/PRODUCT.md) explains how the website, the bot and Discord's shop fit together.

## Pages

| Path | What it is |
|---|---|
| `/` | VocaLink Lite, the translator, with the "For Discord" panel (also opened by `/#discord`) |
| `/terms/` | Terms of use |
| `/privacy/` | Privacy |
| `/app` | Redirects to `/` (the translator's old address) |

## VocaLink Lite, the translator

Nothing runs on a server, so the only cost is Deepgram's speech recognition, paid with each visitor's own key (about $0.46 an hour; new Deepgram accounts get $200 of free credit).

1. Audio comes from the microphone, or from a shared browser tab or screen. Sharing the tab where Discord is open lets it follow a call with no extra software.
2. An audio worklet converts the audio to 16 kHz, lifts quiet voices, and streams it to Deepgram's live API. It uses `nova-3`, and falls back to `nova-2` for languages `nova-3` does not cover. "Taglish" uses the Tagalog model; "Mixed" uses Deepgram's multilingual mode.
3. Words appear while people are still talking. Each finished sentence is translated with Google Translate's free web endpoint, or with Claude if the visitor adds an Anthropic key in Settings.
4. The browser can read each translation aloud with its own voices. While it speaks, the microphone input is muted, so it never translates itself.

It also tells speakers apart (click a name to rename it), floats the transcript in an always-on-top window in Chrome and Edge, shows full-screen captions for screen sharing, and saves the conversation as text or `.srt` subtitles. Shortcuts: space starts and stops, C opens captions, F floats the transcript.

Keys and settings are stored only in the visitor's browser.

## The Discord panel

"For Discord" opens the three server plans (Free, Plus, Pro):

- **Free** opens Discord's screen for adding the bot to a server.
- **Plus and Pro** open each plan's page in Discord's app shop, once the SKU ids are filled in at the bottom of `index.html` (`VL.sku`). Until then they also add the bot, and the bot offers the upgrade inside Discord.

## The pond

The koi follows the pointer and swims faster when someone is speaking. Each new translation sends a ripple across the water, and clicks on open water make ripples too. The scene also has:

- a sky dome with stars and a moon, and fog;
- caustics on the pond bed;
- lily pads, a lotus, reeds, fireflies and mist;
- greetings in ten languages, including "kumusta", that turn into English as the koi passes under them.

The scene uses Three.js r128 from cdnjs with custom shaders. With reduced motion turned on, everything slows right down. If WebGL is unavailable, a still gradient takes the scene's place.

## Tech

Plain HTML, CSS and JavaScript with no build step. Type is Fraunces, Manrope and JetBrains Mono from Google Fonts.

## Run it locally

The microphone needs `localhost` or HTTPS:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Deployment

Vercel deploys every push to `main`. `vercel.json` sets clean URLs and the `/app` redirect.
