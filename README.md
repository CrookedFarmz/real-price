# The Real Price of Vegetables
### An open-source full-cost calculator for CSA farms

**Built by [Crooked Farmz](https://crookedfarmz.net) · East York, Toronto**

---

## What this is

A free, self-contained web tool that helps prospective CSA subscribers understand the *true* cost of their food — not just what they pay at the checkout, but what they spend downstream to compensate for what industrial food doesn't provide.

The tool lets visitors:
- Set their weekly supermarket vegetable spend using a slider
- Adjust their personal supplement, OTC medicine, and digestive aid spending using vertical sliders
- See a live calculated weekly premium (or saving) for choosing a local CSA over the conventional path
- Save their results as an image or share directly to Facebook

The argument is grounded in the work of Masanobu Fukuoka — *"Chemically grown vegetables may be eaten for food, but they cannot be used as medicine"* — and backed by Statistics Canada, OECD, and industry data on Canadian household health spending.

This is a marketing and educational tool, not a medical claim.

---

## Who built this

**Crooked Farmz** is a regenerative urban microfarm in East York, Toronto, now in its 8th season of operation. We grow vegetables and brew freshly-made compost teas — living microbial extracts that feed soil biology — for home gardeners, landscapers, arborists, and farms across the Greater Toronto Area.

We are the first Canadian operation to offer compost tea on a just-in-time CSA subscription basis.

- **Website:** [crookedfarmz.net](https://crookedfarmz.net)
- **CSA subscriptions:** [crookedfarmz.localline.ca](https://crookedfarmz.localline.ca)
- **Instagram:** [@crookedfarmz](https://instagram.com/crookedfarmz)
- **Email:** info@crookedfarmz.net

We built this tool for ourselves, then decided to release it openly because small farms shouldn't have to reinvent the wheel. If it helps you explain your value to your community, that's good for everyone.

---

## Who this is for

Any small-scale CSA farm that wants to make an honest, data-backed case for why their food costs what it costs. The tool is designed to be:

- **Honest first** — it acknowledges upfront that the CSA costs more at the checkout
- **Interactive** — visitors set their own numbers, which makes the conclusion feel earned rather than asserted
- **Portable** — one self-contained HTML file, no framework, no database, no server-side code required
- **Editable** — all text, pricing, and data lives in a single CONFIG block at the top of the file

---

## Technical overview

The entire tool is a single `index.html` file. It has:

- No backend
- No database
- No npm, no build step, no framework
- Two external dependencies: Google Fonts (typography) and html2canvas (save-as-image feature), both loaded from CDN
- Pure HTML, CSS, and vanilla JavaScript

This means it runs on **any static web host** — GitHub Pages (free), Netlify (free), Vercel (free), or any shared hosting that can serve an HTML file.

---

## Quick start: deploying your own version

### Option A — GitHub Pages (recommended, free)

1. Create a free account at [github.com](https://github.com)
2. Create a new public repository (e.g. `my-farm-real-price`)
3. Upload `index.html` to the repository
4. Go to **Settings → Pages → Source: main branch** → Save
5. Your tool is live at `https://yourusername.github.io/my-farm-real-price/`
6. Add your images (see below) and upload them to the same repository

### Option B — Netlify or Vercel (free, also recommended)

1. Create a free account at [netlify.com](https://netlify.com) or [vercel.com](https://vercel.com)
2. Drag and drop your project folder (containing `index.html` and your images) onto the Netlify/Vercel dashboard
3. Your tool is live at a generated URL (e.g. `my-farm.netlify.app`)
4. You can add a custom domain in the dashboard settings

### Option C — Your existing web host

If you already have hosting (WordPress, Squarespace, any shared host), upload `index.html` to a folder on your server and access it directly. Most hosts support static HTML files alongside a CMS.

> **WordPress note:** WordPress.com Free and Personal plans block JavaScript in custom HTML blocks. To embed this tool on a WordPress.com site, host the file on GitHub Pages or Netlify and embed it using an `<iframe>` on your WordPress page.

---

## Adding your images

The tool has three image slots. Upload your images to the same folder as `index.html` and update the URLs in the CONFIG block.

| CONFIG key | Purpose | Recommended size |
|---|---|---|
| `heroImageUrl` | Full-width banner at the top of the page | 1400 × 600px landscape |
| `sectionImageUrl` | Divider image before the Philosophy section | 900 × 400px landscape |
| `logoUrl` | Your farm logo, appears at the bottom above the footer | Max 400px wide |

**Example:** if your file is hosted at `https://myfarm.github.io/real-price/` and you upload `hero.jpg` to the same repository, set:
```
heroImageUrl: "https://myfarm.github.io/real-price/hero.jpg",
```

If an image fails to load, the tool falls back gracefully to a coloured placeholder — so the tool always works even before images are added.

---

## Personalising the tool

Open `index.html` in any text editor (TextEdit on Mac, Notepad on Windows, VS Code, etc.). Everything you need to change is in the **CONFIG block** at the top of the file, between the lines:

```javascript
const CONFIG = {
  ...
};
/* END CONFIG — do not edit below this line */
```

You never need to touch the HTML, CSS, or JavaScript below the CONFIG block.

### Text formatting inside CONFIG fields

Most text fields accept HTML formatting:

| What you want | What to type |
|---|---|
| **Bold** | `<strong>your text</strong>` |
| *Italic* | `<em>your text</em>` |
| Underline | `<u>your text</u>` |
| Line break | `<br>` |
| Hyperlink | `<a href="https://...">link text</a>` |

### Key fields to personalise

**Your farm identity:**
```javascript
eyebrow:  "Your Farm Name · Season · Location",
footerHTML: `Your Farm · Your City<br>
  <a href="https://yourfarm.com">yourfarm.com</a> · your@email.com · your phone<br>
  <em>"Your tagline."</em>`
```

**Your pricing:**
```javascript
ctaSub:        "20-week full share · $XXX<br>10-week alternating share · $XXX",
ctaButtonUrl:  "https://your-ordering-platform.com",
```

**Your CSA season and logistics:**
```javascript
ctaMain: "Your pickup day. Harvested that day.<br>Your location and delivery area.",
```

**The weekly grocery slider range:**
```javascript
sliderMin:     25,   // minimum $/week (adjust for your market)
sliderMax:     55,   // maximum $/week
sliderDefault: 35,   // starting position
```

**The supplement/medicine sliders** — set the `annual` value for each to match the seasonal allocation relevant to your market. The defaults are based on Canadian household data; adjust if you're in a different country or want to reflect different assumptions:
```javascript
addons: [
  {
    id: "supps",
    annual: 65.64,   // seasonal allocation in your currency — this is the slider default
    ...
  },
  ...
]
```

**The Philosophy section** — rewrite the `argumentParagraphs` array to reflect your farm's story and voice. Use `{{WEEKLY_NUMBER}}` anywhere in the text to inject the live-calculated figure:
```javascript
`...the real weekly figure is <strong>{{WEEKLY_NUMBER}}</strong> at your current assumptions.`
```

**Section navigation labels:**
```javascript
sections: [
  { id:"s-intro",    num:1, label:"Introduction",   color:"#4A7C25" },
  { id:"s-app",      num:2, label:"The Calculator", color:"#2D5016" },
  ...
]
```

---

## Adjusting the colours

The colour palette is defined in the CSS `:root` block, just below the CONFIG. Change these values to match your brand:

```css
:root {
  --gd: #2D5016;   /* primary dark green — buttons, checkboxes, CF table row */
  --gm: #4A7C25;   /* mid green — links, eyebrow text */
  --bg: #F7F4EE;   /* page background — warm off-white */
  --bgc: #EFEBE2;  /* card backgrounds */
  --st: #C8A96E;   /* straw/amber — slider highlight, CTA button */
  --ru: #8B3A1A;   /* rust — premium number colour, lock badge */
  --bd: #C4B99A;   /* border colour */
}
```

---

## The data behind the defaults

The default values in the supplement/medicine sliders are based on Canadian household data. If you're adapting this for another country, replace with the appropriate national statistics:

| Category | Default (CAD, 20-wk season) | Source |
|---|---|---|
| Vitamins & supplements | $65.64 | Grand View Research 2024; Canadian market ÷ 14.4M households |
| OTC non-prescribed medicines | $14.63 | OECD 2021 per-capita spend |
| Digestive aids & antacids | $23.10 | Canadian OTC market estimate |

**Important:** These figures represent a defensible *share* of household health spending attributable to dietary quality gaps — not a claim that all supplement spending is caused by food quality. The tool is designed to be honest about this. Don't overstate the case; it will undermine trust.

---

## The full-cost argument in brief

The philosophical foundation is Masanobu Fukuoka's observation that food grown in living soil is medicine, while food grown in chemically-managed soil is merely calories. The economic argument follows: if industrial food fails to deliver the nutritional complexity that living-soil food provides, the cost migrates — to the supplement aisle, to the pharmacy, to the antacid shelf.

The tool doesn't prove this. It invites the visitor to test it against their own spending. The interactivity is the argument: when someone adjusts the sliders to match their own household and watches the number change, the conclusion feels earned.

For the full argument, see the Philosophy section of the tool itself.

---

## Licence

This project is released under the [MIT Licence](https://opensource.org/licenses/MIT).

You are free to use, copy, modify, and distribute this tool for any purpose, including commercial use, with or without attribution. If you find it useful, a mention of Crooked Farmz is appreciated but not required.

---

## Contributing

Found a bug? Have an improvement? Open an issue or submit a pull request. This tool is simple by design — improvements that keep it simple and portable are most welcome.

---

## A note to fellow farmers

This tool took a full season of thinking and several weeks of iteration to get right. The honest-first framing, the interactive sliders, the dynamic number that updates as you adjust — all of it is designed to do one thing: earn trust before asking for a sale.

If you adapt it, keep that spirit. The strongest marketing a small farm can do is to be more honest than the industrial food system, not to imitate it.

Good growing.

**— Sean Smith, Crooked Farmz**

---

*Crooked Farmz · East York, ON · [crookedfarmz.net](https://crookedfarmz.net)*
*"We must heal the soil. It's that simple."*
