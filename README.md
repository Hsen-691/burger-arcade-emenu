# Burger Arcade e-Menu

A restaurant e-menu in an arcade (Pac-Man) style. Customers scan a QR code at the table, browse the menu, build their own burger, and send the order to the restaurant on WhatsApp.

It works on both **phones** and **desktop browsers**, with no app to install.

---

## Features

- **Arcade header:** a small Pac-Man eats dots, a ghost chases him, and a score counter goes up as items are added. There is a mute button for the sound.
- **Pac-Man delivery:** when you add an item, a big Pac-Man runs to the item's picture, eats it, and carries it to the cart.
- **Category tabs:** Bundles, New Items, Burger, Appetizer, Sauce and Beverage. They stay at the top and highlight as you scroll.
- **Build Your Burger:** the burger stacks up live as you pick each layer. A patty and a bun are required.
- **Mystery Burger:** strike out up to 5 ingredients you don't want, then "Lock it in".
- **Bundles with options:** choose the burger type, drinks, and so on.
- **Cart:** change quantities, add a table number, name and notes, then **Send on WhatsApp**. The full order is written out in the message.
- **Table QR codes:** `?table=5` at the end of the link fills in the table number by itself.
- **Layouts:**
  - Phone: one column, with an order bar at the bottom.
  - Tablet: two columns.
  - Desktop: two or three columns, with a "My Order" panel always shown on the right.

---

## Languages and technologies used

| Technology | What it is used for |
|---|---|
| **HTML5** | Page structure: header, arcade strip, tabs, menu, cart and pop-up windows |
| **CSS3** | All styling: the neon arcade theme, the phone/tablet/desktop layouts (media queries, CSS Grid, Flexbox), the sticky tabs and sidebar, and the CSS animations (Pac-Man's mouth, blinking dots, the floating ghost) |
| **JavaScript (plain ES2020, no framework)** | Menu rendering, the cart, the burger builder, the Mystery Burger, the Pac-Man animations and building the WhatsApp message |
| **SVG (drawn in code)** | Every food picture (burgers layer by layer, fries, drinks, wings and so on), so there are no image files to upload |
| **Web Animations API** | Pac-Man flying to the item, eating it and carrying it to the cart |
| **Web Audio API** | The "waka-waka" and "coin" sounds, made in code with no audio files |
| **localStorage** | Remembers the cart, score, sound setting and table number if the page is reloaded |
| **Google Fonts** | *Bangers* (titles), *Press Start 2P* (pixel text) and *Inter* (body text) |
| **WhatsApp link (`wa.me`)** | Sends the order to the restaurant's WhatsApp number |

There is **no backend, no database and no build step**. The whole site is one file, `index.html`.

---

## How it is built

All the code is inside `index.html`, in this order:

```
index.html
├── <style>          CSS: theme colours, layouts, animations
├── HTML             header, arcade strip, tabs, menu, cart sidebar, order bar, pop-ups
└── <script>
    1) CONFIG        restaurant name, WhatsApp number, Instagram, phone, map link
    2) MENU          categories and items  ← edit your menu here
    3) DRAWINGS      SVG functions that draw the burger layers and the other food pictures
    4) APP           menu rendering, cart, burger builder, Mystery Burger,
                     Pac-Man delivery animation, WhatsApp sending
```

### Changing the restaurant details

At the top of the `<script>` section:

```js
const CONFIG = {
  logoTop: "BURGER", logoBottom: "ARCADE",
  name: "Burger Arcade",
  tagline: "YOUR BURGER, YOUR RULES.",
  currency: "$",
  whatsapp: "96100000000",   // digits only, with country code
  instagram: "yourrestaurant",
  phone: "+961 00 000 000",
  mapsUrl: "https://maps.google.com/?q=Beirut"
};
```

### Changing the menu

Each item in `MENU` has a `type` that sets how it behaves:

| type | Behaviour |
|---|---|
| `"simple"` | Normal item with **+ / −** buttons |
| `"builder"` | Build-your-burger window with the live burger picture |
| `"mystery"` | Strike-out list (set the list in `strikes` and the limit in `maxStrikes`) |
| `"options"` | Bundle where the customer picks choices (`groups`) |

Example of a simple item:

```js
{ id:"classic", type:"simple", name:"Classic Beef", price:8,
  desc:"Angus beef, cheddar, lettuce, tomato, pickles",
  art:{ t:"burger", bun:"classic", layers:["beef","cheddar","tomato","pickles","lettuce"] } }
```

**Picture types (`art.t`):** `burger`, `fries`, `drink`, `wings`, `rings`, `sticks`, `sauce`, `box`, `combo`

**Burger layers:** `beef`, `chicken`, `mozz`, `cheddar`, `swiss`, `lettuce`, `rocca`, `tomato`, `pickles`, `jalapeno`, `mushroom`, `rings`, `takis`, `bacon`, `egg`

**Buns:** `classic`, `black`, `brioche`

**Option groups:** use `min:1` to make a group required and `max:1` to make it pick-one.

### How the Pac-Man delivery works

1. A copy of the item's picture is placed exactly over its card.
2. Pac-Man enters from the nearest edge of the screen and moves to the picture.
3. As he crosses it, the picture is cut away behind him (CSS `clip-path`), so it looks like he is eating it.
4. A small copy of the item appears above his head, and he turns toward the cart and flies there.
5. At the cart, he shrinks away, a sound plays, and the cart bounces.

On a phone, the target is the **Send Order** button. On a desktop, it is the **My Order** panel.

---

## Running it on your computer

Double-click `index.html` to open it in any browser.

You can also run a local server, which is closer to how it behaves when it's live:

```bash
npx serve .
```

---

## Deploying on Vercel

The site is a static site, so Vercel only needs to serve `index.html`. You don't need any build settings.

### Option A: Vercel CLI (fastest)

1. Install Node.js from https://nodejs.org if you don't have it.
2. Install the Vercel CLI:
   ```bash
   npm install -g vercel
   ```
3. Open a terminal in the `emenu` folder and log in:
   ```bash
   vercel login
   ```
4. Deploy a preview:
   ```bash
   vercel
   ```
   Answer the questions:
   - Set up and deploy? → **Y**
   - Which scope? → your account
   - Link to existing project? → **N**
   - Project name → e.g. `burger-arcade-menu`
   - In which directory is your code located? → `./`
   - Override the settings? → **N** (Framework: *Other*, no build command, output is the folder itself)
5. Deploy to production:
   ```bash
   vercel --prod
   ```
   You get a link like `https://burger-arcade-menu.vercel.app`.

### Option B: GitHub and the Vercel website (updates automatically)

1. Create a GitHub repository and upload the `emenu` folder (`index.html` and `README.md`).
2. Go to https://vercel.com, then **Add New… → Project**, and import the repository.
3. Use these settings:
   - **Framework Preset:** Other
   - **Build Command:** *(leave empty)*
   - **Output Directory:** *(leave empty, or `.`)*
4. Click **Deploy**.
5. From now on, every time you push a change to GitHub, for example a new price, Vercel updates the live menu by itself.

### Custom domain (optional)

In the Vercel project, go to **Settings → Domains**, add your domain (e.g. `menu.yourrestaurant.com`), and follow the DNS steps Vercel shows.

---

## QR codes for the tables

Make one QR code per table, using the live link plus the table number:

```
https://burger-arcade-menu.vercel.app/?table=1
https://burger-arcade-menu.vercel.app/?table=2
...
```

When a customer scans it, the table number is already filled in on their order.

Any free QR code generator works. Print the codes and put one on each table.

---

## Before going live (checklist)

- [ ] Put the real WhatsApp number in `CONFIG.whatsapp` (digits only, with country code)
- [ ] Update the name, logo text, Instagram, phone and map link in `CONFIG`
- [ ] Replace the sample items in `MENU` with the real menu and prices
- [ ] Test on a phone: add items, build a burger and send a test order on WhatsApp
- [ ] Deploy with `vercel --prod` and print the table QR codes
