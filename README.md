[README.md](https://github.com/user-attachments/files/33799646/README.md)
# LabelSprint: Thermal Shipping Label Generator (Card Collector Edition)

A single-file, browser-based tool for turning a pasted address into a clean 4×6" thermal shipping label. Built for trading card sellers who ship a lot of small packages, with "DO NOT BEND" banners, card badges, and order reference lines built in.

No install, no build step, no backend. Open `index.html` in a browser and start printing.

<!-- ![LabelSprint screenshot](docs/screenshot.png) -->

---

## Features

- **Smart address parsing**: paste a raw address (from an order, email, or forum message) and the recipient fields fill in automatically
- **Editable fields**: fix the name, street lines, city, state, or ZIP by hand; the label preview updates live
- **Live 4×6" preview**: monochrome label that matches what your thermal printer will output
- **Card collector extras** (each can be toggled on or off):
  - "DO NOT BEND - TRADING CARDS ENCLOSED" banner
  - Trading card icon badge
  - Scannable Code 128 barcode (encodes your order / reference number)
  - Custom order / reference number (up to 20 characters)
- **Default sender profile**: set your return address once; it's saved in your browser
- **Recent labels log**: the last 20 printed labels are saved locally, with one-click reload
- **Print-optimized CSS**: `@page` is set to 4in × 6in with zero margins so only the label prints
- **Private by design**: everything is stored in your browser's `localStorage`; nothing is sent to a server
- **Print validation**: won't print until name, street, city, state, and ZIP are filled in

---

## Getting Started

1. Save the file as `index.html` (or any name ending in `.html`).
2. Open it in a modern browser (Chrome, Edge, Firefox, or Safari).
3. Click **Default Sender** and enter your return address, then **Save Sender Profile**.
4. Paste a recipient address into the text box, or click **Load Sample Order** to try it out.
5. Adjust the badges and add an optional order/reference number.
6. Click **Print Direct to Thermal Printer** and choose your label printer.

> **Note:** The page loads Tailwind CSS, Font Awesome, and Google Fonts from CDNs, so an internet connection is needed the first time (and any time the browser cache is cleared).

---

## Address Format

Paste the address with one item per line. The first line is treated as the recipient name, the last line as city, state, and ZIP, and the lines in between as the street address.

```
John Doe
742 Evergreen Terrace, Apt 2B
Springfield, OR 97477
```

The parser expects:

| Part     | Format                                            |
|----------|---------------------------------------------------|
| Name     | First line                                        |
| Street 1 | First line after the name                         |
| Street 2 | Any additional lines before the city/state/ZIP    |
| City     | Letters, spaces, and periods                      |
| State    | 2-letter US state code                            |
| ZIP      | 5 digits or ZIP+4 (`12345-6789`)                  |

If the auto-parse gets something wrong, just edit the fields in the **Parsed Recipient Details** panel.

**Limitations:** the parser is designed for US addresses. International addresses will need to be entered manually.

---

## Printer Setup

LabelSprint prints through your browser's standard print dialog. For best results:

1. Install your printer's driver (AiYin, Rollo, Zebra, MUNBYN, etc.).
2. In the print dialog, select your thermal printer.
3. Set **Paper size** to 4×6 in (or your driver's equivalent).
4. Set **Margins** to *None* and **Scale** to *100%* (not "Fit to page").
5. Turn off **Headers and footers**.

Your printer's driver settings (darkness, speed, label calibration) still apply as usual.

---

## Data & Privacy

- Your sender profile and the last 20 labels are stored in your browser's `localStorage` on your own device.
- No data is uploaded anywhere.
- Clear the **Recent Labels Log** with the **Clear Log** button, or clear your browser's site data to remove everything.
- Labels are saved per browser and per device; they won't sync between computers.

---

## Customizing

Everything lives in one HTML file, so changes are easy:

| To change...                         | Edit...                                                       |
|--------------------------------------|---------------------------------------------------------------|
| Default return address               | The `value` attributes in the **Default Sender** modal         |
| Banner text                          | The `preview-badge-fragile` element                            |
| Default reference text               | The `DEFAULT_REF` constant in the script                       |
| Label size                           | `@page` size and `#thermal-print-area` width/height in CSS     |
| Colors and fonts                     | The Tailwind config and Google Fonts import at the top         |
| History length                       | The `20` limit in `saveToHistory()`                            |
| Barcode max length                   | The `slice(0, 20)` in `code128Svg()` and the input `maxlength` |

---

## Important Notes

- **The barcode is a real Code 128 barcode, but it only encodes your reference number.** It is generated as an SVG with a proper checksum, so any scanner can read it. It is not a carrier-issued tracking barcode, and carriers won't recognize it.
- **This tool does not buy postage.** It only formats and prints address labels. For prepaid shipping labels (USPS, UPS, etc.), use your carrier's or shipping platform's own label.
- **Print confirmation still appears.** The browser's print dialog opens when you click print; browsers don't allow a page to skip it.

---

## Troubleshooting

**Label prints with the wrong size or extra blank pages**
Set the paper size to 4×6 in, margins to *None*, and scale to *100%*.

**The black banner prints as white or gray**
Try Chrome or Edge, and make sure "Background graphics" is enabled if your browser offers that option.

**The barcode won't scan**
Raise your printer's darkness setting and make sure scale is 100%. Keep the reference to 20 characters or fewer.

**Fonts or icons look wrong**
The CDNs may be blocked or offline. Reconnect and refresh the page.

**City, state, or ZIP are blank after pasting**
Make sure the last line looks like `City, ST 12345`, or fill the fields in manually.

**My sender address or history disappeared**
Browser data was likely cleared, or you're using a private/incognito window.

---

## Browser Support

Any current version of Chrome, Edge, Firefox, or Safari. Chrome and Edge generally give the most reliable 4×6" print output.

---

## License

MIT, or choose the license that fits your project.
