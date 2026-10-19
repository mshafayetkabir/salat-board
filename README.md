# Salat Time Board

A single-page prayer time display for mosques, institutes, offices and digital signage. Set your own institute name, location and time zone in one place.

It is one self-contained HTML file. There is nothing to install, no server and no build step. Prayer times are calculated on the device, so it keeps working even if the internet drops after the page has loaded.

## Features

- **Flip clock** showing the current time, with the seconds in gold.
- **Countdown to the end of the present prayer.** Example: "Asr ends in 1h 12m 05s". Between sunrise and Dhuhr, when no prayer is in time, it shows "No prayer time now. Dhuhr starts in ...".
- **Progress bar** showing how much of the present prayer period has passed.
- **Five prayer cards** (Fajr, Dhuhr, Asr, Maghrib, Isha) with the jamat time, start time, end time and duration. The current prayer is highlighted "Now" and the coming one "Next".
- **Sunrise and sunset** shown in the middle of the header.
- **Three dates:** English, Bangla (Bangladesh revised calendar) and Arabic (Islamic / Hijri, written in Bangla). Each one can be shown or hidden.
- **Time zone option.** Pick any time zone by name, with daylight saving handled automatically.
- **Institute name or logo** in the header. Switch between them by commenting a line.
- **No scrolling on screens.** The page fits one screen on landscape monitors and on portrait digital signage.
- **Dark theme** (black with gold accents), easy to read from a distance.

## Quick start

1. Download `index.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. For a display, press `F11` for full screen, or use your signage player's kiosk mode.

## Host on GitHub Pages

1. Create a public repository and upload `index.html` (keep this exact name). Add `logo.png` too if you use the logo.
2. Go to **Settings > Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, then save.
4. After a minute or two the page is live at `https://YOUR-USERNAME.github.io/REPOSITORY-NAME/`.

To update the board later, upload a new `index.html`. The signage browser shows the change when the page reloads.

## Settings

All settings are in the `SETTINGS` block near the top of the `<script>` section in `index.html`. You do not need to edit anything below it.

| Setting | What it does | Current value |
|---|---|---|
| `companyName` | Header title text (full institute name) | Full Institute Name |
| `shortName` | Short institute name, shown in the browser tab title | Short Institute Name |
| `placeName` | Small line under the title | Dhaka, Bangladesh |
| `lat`, `lng` | Location used for the calculation | 23.8103, 90.4125 |
| `timeZone` | Time zone name, for example `"Asia/Dhaka"`. `"auto"` = use the device time zone. `""` = use `utcOffset` instead. See [Time zone](#time-zone) | "Asia/Dhaka" |
| `utcOffset` | Fixed hours from UTC. Used only when `timeZone` is `""` or not recognised | 6 |
| `use24Hour` | `false` = 12-hour clock with AM/PM, `true` = 24-hour | false |
| `showDates` | Show (`true`) or hide (`false`) each date: `english`, `bangla`, `arabic`. See [Show or hide dates](#show-or-hide-dates) | all `true` |
| `hijriOffsetDays` | Adjusts the Hijri date. Use `-1` if local moon sighting is a day behind Saudi Arabia | 0 |
| `hiddenPrayers` | Prayers to hide, for example `["fajr"]` | none hidden |
| `fajrAngle`, `ishaAngle` | Sun angle for Fajr and Isha | 18 (Karachi method) |
| `asrFactor` | `2` = Hanafi Asr, `1` = Shafi/Maliki/Hanbali Asr | 2 |
| `offsetsMin` | Move a calculated time earlier or later, in minutes | all 0 |
| `fixedTimes` | Replace a calculated time with your own ("HH:MM", 24-hour) | all `null` |
| `jamat` | Jamat time: minutes after start, a fixed "HH:MM", or `null` to use the start time | Dhuhr 13:15, Asr 16:15, others `null` |

The big time on each card is the **jamat time**. If you want mosque timetable times, set them in `jamat` or `fixedTimes`.

## Time zone

Set `timeZone` in the `SETTINGS` block to the time zone name (IANA name) of your place:

```js
timeZone: "Asia/Dhaka",
```

| Value | Meaning |
|---|---|
| `"Asia/Dhaka"` | Bangladesh (UTC+6). This is the default. |
| `"Asia/Riyadh"`, `"Asia/Karachi"`, `"Asia/Kolkata"`, `"Asia/Dubai"` | Other examples. Any valid time zone name works. |
| `"Europe/London"`, `"America/New_York"`, `"Australia/Sydney"` | Zones with daylight saving. The clock changes by itself. |
| `"auto"` | Use the time zone of the device that opens the page. |
| `""` (empty) | Ignore the name and use the fixed `utcOffset` number (for example `6` or `5.5`). |

Notes:

- **Change `lat` and `lng` too.** The prayer times are calculated from your location, so a new time zone needs the coordinates of the new city.
- If the name is misspelled or unknown, the page falls back to `utcOffset` and writes a warning to the browser console.
- The clock, dates, prayer times and countdown all follow this time zone, whatever time zone the device itself is set to.
- On the night when daylight saving changes, the Isha countdown can be off by one hour until Fajr.

## Show or hide dates

Use `showDates` in the `SETTINGS` block. `true` shows a date and `false` hides it:

```js
showDates: {
  english: true,   // English date, for example "Monday, 19 October 2026"
  bangla:  true,   // Bangla date (Bangladesh revised calendar)
  arabic:  true    // Arabic (Hijri / Islamic) date, written in Bangla
},
```

A hidden date is removed from the page and the remaining dates close up. If all three are `false`, the whole date area disappears. To correct the Arabic date by a day, use `hijriOffsetDays`.

## Institute name or logo

Set the full name in `companyName` and the short name in `shortName` (both in `SETTINGS`). The full name is the header title. The short name is used in the browser tab title.

In the header of `index.html` there are two marked options:

- **Option A (default):** the institute name (`companyName`).
- **Option B:** the institute logo, in a 2:3 ratio (width 2, height 3).

To use the logo, comment out Option A, remove the comment marks around the `<img>` line in Option B, and put your logo file (for example `logo.png`) in the same folder as `index.html`. If your logo is 3 wide by 2 tall, change `aspect-ratio:2/3` to `aspect-ratio:3/2` in the `.logo` style.

## Screen layouts

| Screen | Layout |
|---|---|
| Landscape monitor or TV | Header with three parts (title, sunrise/sunset, dates), flip clock, countdown, five prayer cards in a row. No scrolling. |
| Portrait digital signage (width 600px or more, height 800px or more) | Everything stacked and centred, with the five prayers as full-width rows. No scrolling. |
| Phones and very short windows | Normal scrolling page. |

If your signage player is set to portrait in its system settings, the page switches to the vertical layout automatically.

## How the times are worked out

Times come from standard sun-position formulas, using your location and the angles in the settings. Each prayer lasts until the next one begins. Fajr ends at sunrise, and Isha lasts until the next Fajr. Calculated times can differ from a mosque timetable by a few minutes, so use `offsetsMin` or `fixedTimes` to match your local timetable.

The Arabic (Hijri) date uses the Umm al-Qura calendar, and the Bangla date uses the Bangladesh revised calendar (new year on 14 April).

## Tips for signage

- Make sure the device's date and time are correct, and set to automatic if possible. The clock follows the device time, adjusted to the time zone you set in `timeZone`.
- Set the browser to full screen or kiosk mode.
- Fonts (Bricolage Grotesque, Hanken Grotesk, Noto Sans Bengali) load from Google Fonts. Offline, the page falls back to system fonts and works as before. Bangla text needs a Bangla-capable system font in that case.
- A nightly browser restart or page reload picks up any updates you have uploaded.

## Browser support

Any modern browser. The page uses CSS `clamp()`, `min()`, `aspect-ratio` and `Intl.DateTimeFormat`, so very old signage browsers may display it incorrectly.

## License

Open source. Add a `LICENSE` file to the repository (for example MIT) to state the exact terms.

## Credit

Open source - Creator - Md. Shafayet Kabir
