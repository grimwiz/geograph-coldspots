# geograph-coldspots

Find which of your own photographs would fill gaps on [Geograph Britain and Ireland](https://www.geograph.org.uk/),
then write them up and submit them — from a single web page that runs in your browser.

Geograph aims to photograph every 1 km grid square of Britain and Ireland and already holds millions of pictures.
Coverage is uneven at the 100 m scale, and a photo library from a holiday or a walk often contains pictures of places
Geograph has never seen. This page reads the GPS position in each of your photos, asks Geograph how many pictures
already exist in that 100 m square, and shows you the emptiest first.

> This is an independent tool. It is not made by, or affiliated with, the Geograph project.
> Please follow Geograph's guidance on [which square to submit to](https://www.geograph.org.uk/help/which_square) — moderators are volunteers.

## What it does

- **Reads a folder of JPEGs** through Chrome's folder picker, read-only. Nothing is uploaded until you press Submit.
- **Works out each photo's grid reference** from its EXIF GPS: Ordnance Survey National Grid via the official
  OSTN15 transformation (agrees with OS's worked example to within 0.2 m), Irish Grid for Ireland. Photos with no GPS,
  a large recorded GPS error, or a position that jumps implausibly between neighbouring shots are set aside.
- **Checks Geograph's coverage** once per 100 m square (not once per photo), one request a second, cached in the
  browser for 30 days. Three failures in a row, or any "slow down" reply, stops it until you press Retry.
- **Places the subject, not just the camera.** Geograph files a picture under the square of *what it shows*.
  The page shows a map with your camera position (and compass heading, if the phone recorded one), nearby Geograph
  pictures and named OpenStreetMap features; click one, drop a pin, paste a Google Maps link or coordinates, or mark
  the photo as a wide view with no single subject (Geograph then gets only the 4-figure square).
- **Drafts a title and description** with a vision model of your choice (any Ollama server, or anything that speaks
  OpenAI-style chat completions). The prompt includes the grid reference, heading, nearby Geograph titles and
  OpenStreetMap names so the model names real places, and it flags photos whose main subject is people, pets, food
  or an interior. You check and edit everything before it goes anywhere.
- **Submits** in one of two ways:
  - *Through the website* (always available): copies the details to the clipboard, saves an upload copy named after
    the subject grid reference (Geograph reads the subject from the file name and the camera position from the GPS
    EXIF), turned upright if the phone stored it rotated, and opens Geograph's submission form.
  - *Directly* via Geograph's JUppy upload interface, once your account qualifies (see below).
- Search, device filter (useful when a shared album mixes several people's phones), sorting, and per-folder memory
  of what you have skipped or submitted.

## Getting started

1. Download this repository (green **Code** button → **Download ZIP**) and unzip it. Keep `index.html` and the `lib`
   folder together.
2. Open `index.html` in **Chrome or Edge**. (Firefox and Safari lack the folder picker; the page falls back to a
   plain folder upload, which works but cannot remember the folder between visits.)
3. Press **Open photo folder** and choose a folder of photos. The page reads them and starts checking coverage.
4. Optional: open **Settings** to add a vision model and, later, your Geograph upload details.

No installation, server or build step is needed.

### Vision model (optional)

In **Settings**, give an endpoint URL, a model that accepts images (for example `qwen2.5vl`, `gemma3` or
`llama3.2-vision`) and an API key if the server needs one.

- A URL ending in `/api/chat` is spoken to as Ollama; anything else as OpenAI-style `/chat/completions`
  (Open WebUI, LiteLLM, hosted services).
- A page opened from disk sends `Origin: null`, so the server must allow it. For Ollama, start it with
  `OLLAMA_ORIGINS=*`; for other servers, allow that origin in their CORS settings.

### Submitting directly

Geograph only allows its JUppy upload interface after your first **five pictures submitted through the website have
been moderated**. Until then, each card offers **Submit through website**. Afterwards:

1. Open `https://www.geograph.org.uk/juploader.php?action=login&username=YOUR_NAME&password=YOUR_PASSWORD` once.
   (This is Geograph's own login design; delete the entry from your browser history afterwards.)
2. Copy `<user_id>` and `<validation>` from the reply into **Settings**. The cards switch to **Submit to Geograph**.

Limits of this route, from Geograph's published source code — the live site may differ: only the standard image size
is kept, there is no field for geographical context, and because Geograph sends no CORS headers on that endpoint the
page cannot read the reply. Check "Your submissions" on Geograph after sending. The website route has none of these
limits.

## What leaves your computer

- **Geograph** (`api.geograph.org.uk`): the centre of each 100 m square you have photos in, and coordinates near
  the camera when you press *Write text* or open the map.
- **OpenStreetMap Overpass** (`overpass-api.de`): the camera position, to list named features within 1.5 km.
- **Your vision model**: a 1024 px copy of the photo plus the prompt, only when you press *Write text*.
- **Map tiles** (Esri aerial imagery; OpenStreetMap as an option): the map area you look at.
- **Geograph upload**: the photo and its details, only when you press Submit.

Settings, decisions and drafted text stay in your browser's storage. *Save state…* writes them to a file you choose.
The page never writes to the photo folder; upload copies go to a separate folder you pick once.

## Notes and limits

- The aerial imagery is not aligned exactly to the OS grid; offsets of 10–20 m are common in remote areas. For a
  subject near a 100 m square boundary, check its position against a surveyed source such as a
  [Historic England listing](https://historicengland.org.uk/listing/the-list/).
- Geograph's text search only finds words in titles and descriptions, so "subject not yet covered" is a hint, not
  proof.
- Google Places is deliberately not used: its terms forbid copying its coordinates into another database.
  You can still paste a location from Google Maps by hand.
- An optional `.immich-mirror.json` manifest in the folder limits the page to the files it lists and names the folder
  after the album; any folder of JPEGs works without one.

## Licence

MIT — see [LICENSE](LICENSE). Bundled third-party components and data are listed in
[THIRD-PARTY.md](THIRD-PARTY.md) with their own licences.
