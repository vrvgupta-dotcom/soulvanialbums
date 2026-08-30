# SoulVani Listening Room

A single-page showcase for SoulVani's three Hindi neo-soul albums, with inline 30-second previews for all 25 songs. Built as a plain static site — no build step, no framework, no server required.

## Structure

```
index.html          the whole site (markup + CSS + JS in one file)
audio/a{N}_{TT}.mp3  30-second preview clips, N = album (1-3), TT = track number
images/cover{N}.jpg  album cover art (compressed for web)
images/logo.png      SoulVani brand mark
```

Track order within each album matches the numbered subfolders in the source
Dropbox project (`Album-1 - इश्क़ बेक़रार`, etc.). If tracks are ever reordered
or added, rename files to keep the `aN_TT` pattern and update the `DATA`
object in the `<script>` block at the bottom of `index.html` (title + mood
tag per track — the audio filename is derived from album/track number).

## How the previews were made

Each clip is the first 30 seconds of the mastered/remastered mix (auto-picked:
"(Remastered)" version preferred where one exists), re-encoded to 56kbps mono
MP3 with a 1.5s fade-out, via ffmpeg. Source masters are the full-quality
files still sitting in the Dropbox project folder — these are lightweight
derivatives for web delivery only.

## Design system (for anyone extending this)

- **Concept**: "a night listening room" — deliberately single-theme dark
  design (see comment block at the top of the `<style>` tag in index.html
  for the full rationale and token list).
- **Colors**: `--ink` (page ground), `--marigold` (single accent / play
  state), plus one identity color per album — `--rose` (Album I),
  `--lavender` (Album II), `--slate` (Album III) — pulled directly from each
  album's own tagline ("pink skies", "twilight, lavender", "midnight").
- **Type**: Martel (serif display, Devanagari + Latin) for headings, Hind
  (sans, Devanagari + Latin) for body/track titles, IBM Plex Mono for
  numerals/mood-tag captions. Loaded from Google Fonts.
- **Player**: one shared `<audio>` element; each track button draws an SVG
  ring that fills over the 30-second clip and auto-resets on `ended`. Only
  one track plays at a time.

## Known open items / things to decide before this goes live as the real site

1. **No purchase/streaming links included** — this build is a pure listening
   showcase, by design (previous decision). The real site at
   `albums.soulvanimusic.com` currently has Deluxe (₹2,999) / Personal
   (₹4,999) collector's-edition tiers and streaming links — decide whether
   those get added here or this stays a separate "listen first" page that
   links out to the main site.
2. **The real site's streaming links are currently placeholders** — every
   Spotify/Apple Music link on albums.soulvanimusic.com points to the
   generic platform homepage, not an actual artist/track page, and no
   working SoulVani presence was found on Spotify/Apple Music/YouTube
   during research. If this page will link out to streaming, those real
   URLs need to exist first.
3. **Album III (अधूरे लम्हें) has no video/canvas/thumbnail assets yet** —
   audio-only in the source project. Doesn't block this listening page, but
   relevant if the deploy also needs to power video/social assets.
4. **Hosting/domain**: not yet decided. This is a static site — deployable
   as-is to Netlify, Vercel, GitHub Pages, or dropped into whatever currently
   serves albums.soulvanimusic.com.

## Deploying

Any static host works — this is one HTML file plus two asset folders. E.g.:

```bash
# Netlify (from this folder)
npx netlify-cli deploy --prod

# Vercel
npx vercel --prod

# or just upload index.html + audio/ + images/ to existing hosting
```
