<p>
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="brand/logo-dark.png">
    <img alt="VocaLink" src="brand/logo-light.png" height="64">
  </picture>
</p>

# VocaLink website

The website for VocaLink, live voice translation for calls. The home page is a working translator that runs in the browser, set over a night-time Three.js koi pond. The same page is where people add the VocaLink bot to their Discord server.

Live site: [vocalinktest.vercel.app](https://vocalinktest.vercel.app)

The Discord bot lives in [datArtist-io/VocaLink-discord-bot](https://github.com/datArtist-io/VocaLink-discord-bot).

## Pages

| Path | What it is |
|---|---|
| `/` | The translator, with the "For Discord" panel (also opened by `/#discord`) |
| `/terms/` | Terms of use |
| `/privacy/` | Privacy |
| `/brand/` | The logo, colours and type, with files to download |
| `/app` | Redirects to `/` (the translator's old address) |

## The translator

Nothing runs on a server, so the only cost is Deepgram's speech recognition, paid with each visitor's own key. New Deepgram accounts come with free credit.

1. Audio comes from the microphone, or from a shared browser tab or screen. Sharing the tab where Discord is open lets it follow a call with no extra software.
2. An audio worklet converts the audio to 16 kHz, lifts quiet voices, and streams it to Deepgram's live API. It uses `nova-3`, and falls back to `nova-2` for languages `nova-3` does not cover.
   - **Auto-detect (the default)** runs two models on the same audio: Deepgram's multilingual model (English, Spanish, French, German, Hindi, Russian, Portuguese, Japanese, Italian and Dutch) and its Tagalog model, which also handles Taglish. For each sentence it keeps the version heard more confidently. If one model is unavailable, the other carries on.
   - **Choosing one language** uses a single model.
3. Words appear while people are still talking. Each finished sentence is translated with Google Translate's free web endpoint, or with Claude if the visitor adds an Anthropic key in Settings.
4. The browser can read each translation aloud with its own voices. While it speaks, the microphone input is muted, so it never translates itself.

Optional services, each with the visitor's own key (Settings):

- **Soniox** instead of Deepgram: recognises Tagalog and Taglish natively, follows language switches mid-sentence, and translates in the same stream, so Google is not used.
- **ElevenLabs** instead of the browser voice: natural voices for read-aloud, with seven to choose from. If the key or quota fails, the browser voice takes over.

With Deepgram, auto-detect locks each speaker to their language after four clear sentences and then listens with that single language model; it goes back to detecting if the language clearly changes.

It also tells speakers apart (click a name to rename it), floats the transcript in an always-on-top window in Chrome and Edge, shows full-screen captions for screen sharing, and saves the conversation as text or `.srt` subtitles. Shortcuts: space starts and stops, C opens captions, F floats the transcript, P explores the pond.

Keys and settings are stored only in the visitor's browser.

## The Discord panel

"For Discord" explains the bot in three steps and links to Discord's screen for adding it to a server.

## The pond

The pond button in the header (or P) fades the app away so the pond can be explored: drag to look around and scroll to move closer. Translation keeps running underneath.

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
