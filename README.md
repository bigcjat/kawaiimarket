# 🌸 KawaiiMarket: Standalone Serverless NFT Explorer & Marketplace

KawaiiMarket is a **100% serverless, client-side, single-page application (SPA)** designed for exploring NFT collection rarities, browsing listing prices, and executing trades on the XRP Ledger. It is optimized to run statically on **GitHub Pages** with zero backend infrastructure.

This open-source demo is configured for the **Dropchans** collection.

---

## 🚀 Key Features

* **📉 Zero Backend Serverless Architecture:** Runs entirely in the browser. Hosted for free on GitHub Pages.
* **📊 Triple Rarity Ranking Models:** Toggle between three standard rarity models calculated directly from pre-computed static databases:
  * **Standard Rarity:** Classic `rarity.tools` sum-of-attribute-scores.
  * **Statistical Rarity:** Joint probability product of attribute frequencies.
  * **Average Rarity:** Simple average of all trait percentages.
* **🔍 In-Memory Trait Filtering:** Filter the entire 10,000-token collection instantly by trait category and specific values in-memory.
* **✨ Click-to-Filter Traits:** Interactive Details Modal lets users click any trait badge on an NFT to instantly filter the main gallery to match.
* **🔒 Secure Xaman (Xumm) PKCE Integration:** Safe client-side wallet connection using OAuth2 PKCE without exposing API secret keys.
* **⚡ Real-Time WebSockets:** Uses native WebSocket endpoints (`wss://xumm.app/sign/...`) to track signing events instantly, replacing heavy polling loops.
* **📦 Local Thumbnail Cache:** SVGs are cached locally under `/cache/thumbnails` for instantaneous, offline-ready image loading, with sequential gateway fallback.

---

## 🛠️ Installation & Deployment

### 1. Host on GitHub Pages
1. Push this repository to your GitHub account (e.g., `github.com/yourusername/kawaiimarket`).
   * *Note: It is highly recommended to add `cache/` to `.gitignore` to avoid pushing the 982 MB of images to GitHub.*
2. Navigate to your repository **Settings > Pages**.
3. Under **Build and deployment**, set the source to deploy from the `public` folder (or root, depending on your folder layout) and select your branch.

### 2. Configure Custom Image Hosting (Recommended)
Since the SVG collection cache is ~1 GB, pushing it directly to GitHub Pages can exceed repository limits. Instead:
1. Upload the `cache/` folder directly to your own static web server or CDN (e.g. via SFTP or SCP).
2. Configure the `IMAGE_BASE_URL` constant inside `index.html` to point to your server:
   ```javascript
   const IMAGE_BASE_URL = "https://yourserver.com/cache/thumbnails/";
   ```
If you do not configure this, the site will automatically fallback to loading images from public IPFS gateways, but using your own server yields instant loading.

### 2. Configure Xaman developer dashboard
Because OAuth authentication redirects back to your hosted page, you must whitelist your domain:
1. Log into the [Xaman Developer Console](https://developer.xumm.app/).
2. Open your Application settings.
3. Add your GitHub Pages URL to the **Allowed Redirect URLs / Origins** settings:
   ```text
   https://yourusername.github.io/kawaiimarket/
   ```
4. Update the Xumm App ID in `index.html` (if you are using a custom app record):
   ```javascript
   xumm = new XummPkce("YOUR-XUMM-APP-ID");
   ```

---

## ⚙️ Customizing for Another Collection

To adapt this codebase to a different XRP Ledger NFT collection:
1. **Asset Cache:** Replace the SVG/AVIF files in `public/cache/thumbnails/{ISSUER}/{TAXON}/{item_id}.svg`.
2. **Rank Database:** Generate your own `dropchan_ranks.js` matching your collection attributes and rankings.
3. **Traits Database:** Generate a `dropchan_metadata.js` file mapping token numbers to attributes to enable offline trait filtering:
   ```javascript
   window.DROPCHAN_METADATA = {
       "1": [{"trait_type": "Background", "value": "Pink"}, ...],
       ...
   };
   ```
4. **Header Variables:** Edit the global constants in the `<script>` block of `index.html`:
   ```javascript
   const ISSUER = "YOUR_COLLECTION_ISSUER";
   const TAXON = 0; // Taxon index
   const IMAGE_CID = "YOUR_IPFS_CID_FOR_FALLBACK";
   ```

---

## ⚖️ Rarity Formula Disclaimer

The calculations provided on this dashboard are mathematical reference evaluations. Due to implementation details—such as whether a platform counts total trait counts, normalizes case/whitespaces, or handles missing traits as `None` values—individual ranks can shift slightly between marketplaces. No promise or guarantee is made that these models use an identical formula as `xrp.cafe` or `bidds.com`.

---

## 📝 License

This project is open-source and available under the [MIT License](LICENSE). Feel free to fork, customize, and deploy for your own collections!
