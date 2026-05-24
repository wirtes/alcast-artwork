# Cover Wall

`covers.html` is a self-contained artwork wall that pulls album cover images from Mastodon playlist accounts and fills the browser window with a responsive tiled grid.

The page is designed for passive display use. It continuously refreshes, redraws the wall with animation, and shows hover text pulled from the related Mastodon post.

## URL Parameters

The page supports one query parameter:

- `station`

## `station`

`station` selects which Mastodon account the page uses as its artwork source.

The value is normalized to lowercase before matching, so uppercase and mixed-case values are accepted.

If `station` is missing, the page defaults to `alcast`.

If `station` is present but not recognized, the page also falls back to `alcast`.

### Supported `station` values

- `alcast`
  Uses `https://mastodon.social/@alcastradio`
  Page title: `Alcast Radio`

- `kvcu`
  Uses `https://mastodon.social/@1190playlist@mastodon.social`
  Page title: `Radio 1190 Playlist`

- `kuvo`
  Uses `https://mastodon.social/@kuvo_playlist`
  Page title: `KUVO Playlist`

- `kjac`
  Uses `https://mastodon.social/@the_colorado_sound_playlist`
  Page title: `The Colorado Sound Playlist`

- `kvoq`
  Uses `https://mastodon.social/@indie1023_playlist`
  Page title: `Indie 102.3 Playlist`

### `station` examples

- `http://127.0.0.1:8000/covers.html`
  Defaults to `alcast`

- `http://127.0.0.1:8000/covers.html?station=alcast`
  Uses the Alcast feed

- `http://127.0.0.1:8000/covers.html?station=kvcu`
  Uses the Radio 1190 feed

- `http://127.0.0.1:8000/covers.html?station=kuvo`
  Uses the KUVO feed

- `http://127.0.0.1:8000/covers.html?station=kjac`
  Uses The Colorado Sound feed

- `http://127.0.0.1:8000/covers.html?station=kvoq`
  Uses the Indie 102.3 feed

- `http://127.0.0.1:8000/covers.html?station=UNKNOWN`
  Falls back to `alcast`

## Refresh Behavior

The page checks for new artwork every 15 seconds.

## Feed Behavior

The page looks up the selected Mastodon account through Mastodon’s public account lookup API, then loads public statuses from that account.

It only uses posts that contain image attachments.

Important detail:

- The most recent Mastodon post is not always the most recent artwork post.
- If a station publishes a text-only post, the page skips it and uses the newest post that actually includes artwork.

The page also has an RSS fallback path if the API request fails.

## Artwork Selection

The page fetches enough posts to fill the visible wall with unique covers.

Behavior:

- Covers are deduplicated by image URL.
- Covers are sorted newest-first.

## Overlay Text Cleanup

Hover text is derived from the Mastodon post text and then cleaned for display.

This cleanup is intended to remove trailing hashtag blocks commonly appended by some stations.

Behavior:

- Trailing hashtag-only lines are removed.
- Trailing inline hashtag blocks at the end of the last content line are removed.
- Earlier hashtags that are part of the main sentence are preserved when possible.

## Interaction Behavior

Hover behavior works like this:

- A cover must be hovered for 1 full second before the overlay appears.
- A click or tap on the cover also triggers the overlay
- The overlay appears near the mouse position.
- The hovered tile gets a brief jiggle when the overlay appears.
- The overlay disappears after 5 seconds or as soon as the mouse moves.
- If the wall redraws and the same hovered item is still present, the hover will be preserved across the redraw.

## Visual Behavior

The page:

- Fills the viewport with a responsive grid
- Uses responsive square tiles sized to fit the window width cleanly
- Crops artwork to square using `object-fit: cover`
- Refreshes dynamically without a full page reload

On first load, the page waits until the visible artwork set has loaded before starting the opening animation.

## Notes

- The page depends on Mastodon public endpoints being reachable from the browser.
- Because the content source is live, visible artwork changes over time.
- Different stations may have slightly different post formatting, so overlay text cleanup is intentionally conservative and targeted at trailing metadata.


