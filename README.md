# Odoo Notification Bar

> A free, native announcement / notification bar for Odoo websites — by [19prince.com](https://www.19prince.com)

A rotating, dismissible, schedulable notification bar that sits above your Odoo website header — built entirely on Odoo's native website builder. No new database models, no backend settings screens, no theme assumptions.

Because everything lives in a standard website view, you edit it exactly like any other content: click it, type, style it with the toolbar you already know. Translation, multi-website, and theme colors all work out of the box.

The Odoo module inside this repository is `website_announcement_bar` (its technical name); "Odoo Notification Bar" is the friendly name for the same thing.

- **Odoo:** 18.0. 19.0 planned.
- **License:** LGPL-3
- **Author:** [19 Prince](https://www.19prince.com) — Odoo Marketing consultancy.

---

## Features

- **Site-wide, above the header.** One bar per website, injected above your header on every page.
- **Native editing.** The message is ordinary website content — edit text, links, and inline styling (bold, size, text color) with the standard Odoo editor toolbar.
- **Rotating messages.** Add as many messages as you like. They cycle automatically using a Bootstrap carousel, with a configurable delay and a slide or fade transition. Rotation pauses on hover.
- **Visitor dismissal.** Optional close button. A dismissal lasts for the visitor's browser-tab session (re-appears in a new tab or a new session).
- **Scheduling.** Optional "Show From" and "Show Until" date/times. The bar only appears inside the window — perfect for a sale, a launch, or a holiday-hours notice you set and forget.
- **One background color control.** A single colorpicker with your theme palette — no image/video/shape clutter.
- **Per-bar font.** Font-family picker wired into Odoo's native font machinery, including Google Fonts.
- **Off by default.** After install the bar is disabled, so visitors never see placeholder content. You turn it on in the editor when you're ready.
- **Any theme.** Automatically stacks above fixed/sticky headers; sits in normal flow on themes that scroll. No per-theme configuration.
- **Multi-language & multi-website.** Content lives in a view, so Odoo's native translation and per-website copy-on-write apply automatically.

---

## Requirements

- Odoo 18.0 (Community or Enterprise).
- Depends only on `website`.

---

## Installation

### Odoo.sh / on-premise (recommended)

1. Clone this repository (or copy the `website_announcement_bar/` folder) into your addons path:
   ```
   git clone https://github.com/19prince/Odoo-Notification-Bar.git
   ```
   On Odoo.sh, add the `website_announcement_bar/` folder to the branch that backs your build.
2. **Apps → Update Apps List.**
3. Search **"Website Announcement Bar"** and click **Install**.

The bar stays **off** until you enable it in the editor (next section).

### Odoo Online (SaaS)

Odoo Online does not allow custom modules. A deployment script that reproduces the bar through the website's custom code (views + injected CSS/JS) is included at `tools/push_to_odoo_online.py`. Note: on SaaS the per-bar **Font** picker is unavailable (it needs an asset-bundle variable custom code can't reach); all other controls work.

---

## Usage

### Turn it on

1. Open your website and click **Edit**.
2. Click your **header**. In the **Customize** panel, switch **Announcement Bar** on. The page reloads with the bar in place.

### Edit the message

- Click the bar's text and type. Use the floating **Inline Text** toolbar for bold, size, links, and text color — the same toolbar as anywhere else in Odoo.

### The bar's options (click the bar)

| Control | What it does |
|---|---|
| **Enabled** | Turns the whole bar off. (Turn it back on from the header's Customize panel — once off, the bar isn't there to click.) |
| **Message Style → Font** | Font family for the bar, including Google Fonts. |
| **Message Style → Background Color** | The bar's background color, from your theme palette or a custom color. |
| **Visitors Can Close** | Shows/hides the visitor close (×) button. |
| **Show From** | Optional start date/time — the bar stays hidden before it. |
| **Show Until** | Optional end date/time — the bar hides itself after it. |
| **Message** (Add Message) | Adds another rotating message. |
| **Transition** | Slide or Fade between messages. |
| **Delay** | Seconds each message shows before rotating. |

### Per-message controls (click a message)

A selected message shows move / add / remove buttons:

- **Move Backward / Move Forward** — reorder the message.
- **Add Message** — insert another.
- **Remove Message** — delete this one.

---

## How it works

- **Dismissal** is stored in `sessionStorage` under `website_announcement_bar_closed`. It clears when the browser tab closes, so a returning visitor sees the bar again. Nothing is written server-side, so page caching is unaffected.
- **Scheduling** is enforced in the browser from data attributes on the bar. The bar renders hidden and a small script reveals it only when the schedule allows and it hasn't been dismissed — which also means no flash of the bar before it's due.
- **Rotation** is a standard Bootstrap carousel; the delay is the carousel interval, and messages pause on hover.
- **Fixed-header stacking.** If your theme pins the header to the top of the viewport, the script promotes the bar to the very top, offsets the header by the bar's height, and pads the page to compensate — reverting cleanly on dismissal. On themes with a normally-scrolling header it does nothing.
- **Self-healing carousel.** On every page load the script rebuilds the carousel indicators to match the number of messages, so editing patterns (duplicating a message, undo) can't leave the carousel in a state that would break rotation.
- **Edit-mode safe.** The runtime script never runs inside the website editor, so none of these runtime changes are ever saved back into your page.

---

## Turning it off / uninstalling

- **Temporarily:** untick **Enabled** on the bar, or the **Announcement Bar** toggle in the header's Customize panel.
- **Fully:** uninstall the module from **Apps**. Because there are no custom database models, uninstalling leaves nothing behind.

---

## Technical notes

- **Zero Python models.** Messages, styling, schedule window, and flags are all stored as content and data attributes in an editable view that inherits `website.layout`. This is what makes translation, multi-website copy-on-write, and cache-safety free.
- **Tests** are opt-in (not run by default) so hosted platforms don't run them against copies of customized production databases. Run them locally with:
  ```
  odoo-bin -d <db> -i website_announcement_bar --test-tags=announcement_bar --stop-after-init
  ```

---

## Changelog

| Version | Change |
|---|---|
| 18.0.1.0.6 | Single background control (excluded the native section-background editor from messages); grouped Font + Background Color under a "Message Style" header. |
| 18.0.1.0.5 | Self-healing carousel indicators; "Enabled" off-toggle on the bar; native move/add/remove message controls. |
| 18.0.1.0.4 | Offset fixed headers with inline-`!important` so themes that pin the nav can't cover the bar. |
| 18.0.1.0.3 | Made the test suite opt-in via the `announcement_bar` tag. |
| 18.0.1.0.2 | Kept the browser tour out of standard test runs. |
| 18.0.1.0.1 | Stack the bar above fixed/sticky headers. |
| 18.0.1.0.0 | Initial release: rotating, dismissible, schedulable bar with native styling. |

---

## License

LGPL-3. Built by [19 Prince](https://www.19prince.com) — an Odoo implementation consultancy helping mid-market companies replace disconnected systems with Odoo.
