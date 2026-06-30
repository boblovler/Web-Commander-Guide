# Thetis Commander Web — User Guide

**by Bob Lovler, KC4VO**

**User Guide V2.1**

*A universal browser-based remote control for Thetis SDR*

Based on Thetis 2.10.3.x — runs on any device with a web browser

---

## Contents

1. Introduction — What is Thetis Commander Web?
2. Universal Compatibility — Run on Anything
3. How It Works — The Server-and-Browser Model
4. Installation and First Run
5. The Interface at a Glance
6. Connecting to Thetis
7. Frequency Display and Tuning
8. The S-Meter
9. Mode, Band, and Filter — The Custom Button Grid
10. The Bandscope
11. The Waterfall
12. The Mic Meter — Transmit Audio Monitoring
13. The MOX Button and Transmit Control
14. RX1 / RX2 Operation
15. Poor Connection Mode — Operating Over a Bad Network
16. Adding to Your Home Screen — The PWA Experience
17. The Configuration Page — Buttons, Colors, and Flic
18. Settings, Persistence, and Resetting
19. Network Setup
20. Audio — Set Up Your Own Audio Path
21. MOX Bus Integration — Talking to Other Programs
22. Compatibility and Interoperability
23. Troubleshooting
- Appendix A — Configuration File Reference
- Appendix B — Bandscope Color Thresholds
- Appendix C — Glossary

---

## 1. Introduction — What is Thetis Commander Web?

Thetis Commander Web is a browser-based remote control for the Thetis SDR application. It runs as a small server program on the PC where Thetis is running, and presents a clean, dark-themed control panel inside any standard web browser — on any device, on any operating system, anywhere on the network. Open a single URL and you have your radio in front of you. There is nothing to install on the client side and no app store involved.

The program is built around the standard Thetis CAT and TCI protocols. All radio functionality — tuning, mode changes, band switching, filter selection, MOX, the spectrum and waterfall, the S-meter, the microphone level meter, MultiRX operation — is driven through those protocols against an unmodified, stock copy of Thetis. Nothing about Thetis itself is changed. Commander Web is a new face on the front of the machine; the machine inside is the full Thetis you already know.

A few important things to understand before you go any further:

- Commander Web is not a replacement for Thetis. Thetis still runs on its host PC and does all of the real radio work — the hardware, DSP, audio paths, CAT command processing, and the IQ data stream that the bandscope is built from. Commander Web is a remote front end that connects to it.
- Commander Web does not carry audio. The audio path between your radio and your remote device is something you set up separately, outside this app. See Section 20.
- Commander Web does not require any modification to Thetis. It uses the CAT server (TCP 13013) and the TCI server (WebSocket 50001) that ship with every recent Thetis build.
- Everything you customize in Commander Web — your button layout, your bandscope and waterfall preferences, your color choices, your hotkey binding — is saved in your browser and comes back automatically the next time you open the page.

## 2. Universal Compatibility — Run on Anything

This is the part of Commander Web worth understanding before anything else: there is no client to install, there is no operating system requirement, and there is no app store. The client is your existing web browser — whatever browser, on whatever device, on whatever operating system you happen to have in your hand.

If a device has a modern web browser, it works:

- **iPhone or iPad.** Safari, Chrome, Firefox, Edge — all work. The page can be added to the home screen for a full-screen, app-like experience (see Section 16).
- **Android phone or tablet.** Chrome, Firefox, Samsung Internet, Edge, Brave — all work. Same home-screen install path as iOS.
- **Windows PC.** Edge, Chrome, Firefox, Brave, Opera — all work. Use it on a second monitor while your main Thetis screen runs on the primary monitor.
- **Mac.** Safari, Chrome, Firefox, Edge — all work. Same convenience as Windows; the Thetis PC itself can stay headless in the shack while you operate from a MacBook in the kitchen.
- **Linux desktop or laptop.** Firefox, Chrome, Chromium, Edge — all work. No driver, no compatibility layer needed.
- **Chromebook.** Built-in Chrome handles it natively. Useful as a cheap, dedicated control surface.
- **Linux ARM single-board computer (Raspberry Pi, etc.).** Any browser that runs on the device works.

There is one client and it is the browser you already have. There is nothing to download, no app store account required, no platform-specific code path, no signing or notarisation hurdle, no certification process. When you tap the URL on your phone or type it into your laptop, the entire user interface — frequency display, S-meter, custom button grid, real-time bandscope and waterfall, mic meter, MOX control — arrives as a single HTML page that runs locally in the browser, talking back to the server over WebSocket.

The practical consequence: the same operating experience is available on every device you own without any per-device configuration. A new phone with a fresh browser just needs the URL. A friend visiting the shack can pull up the radio on their own device without installing anything. The radio follows you around the house, the office, or the campsite, wherever you can reach the Thetis PC over the network.

> **Important:** The browser is the universal client. That is the entire value proposition of Commander Web, and it is also why a few design decisions — no audio in the page, no HTTPS, no installable app bundle — were made deliberately. See Section 19 for the network-and-security reasoning.

## 3. How It Works — The Server-and-Browser Model

Commander Web has two parts: a small server program that runs on the same PC as Thetis, and a single web page that the server delivers to any browser that asks for it.

The server program talks to Thetis on your behalf, using Thetis's own built-in services (no Thetis modifications required). The browser talks to the server. Everything you do in the browser — tap a button, move a slider, type a frequency — goes to the server, which forwards it to Thetis. Everything that happens at Thetis — the VFO moves, a signal arrives, you key the transmitter — comes back through the server and updates the browser in real time.

Multiple browsers can be connected at once, each with its own session. When you change something from one device, the other devices see the change within a fraction of a second.

## 4. Installation and First Run

### 4.1 Prerequisites

- A working installation of Thetis with the CAT server enabled (default port 13013) and the TCI server enabled (default port 50001). Both are standard features of recent Thetis builds.
- A Windows PC (10 or 11) for the Commander Web server. The server is shipped as a single self-contained .exe; no Python installation is required.
- A network — a home LAN is enough. Remote access requires the usual port forward or VPN considerations (see Section 19).
- At least one client device with a web browser. Anything from a phone to a desktop computer.

### 4.2 Starting the Server

Copy Universal_Web_Commander_TCI.exe to a folder of your choice on the Thetis PC and double-click it. A small status window opens, showing the version, the current configuration, and a URL of the form http://<your-PC-IP>:8765/ which you can give to your browser.

On first run, the status window asks for five values: the Thetis CAT host and port (defaults 127.0.0.1 and 13013), the server port that browsers will connect to (default 8765), and the TCI host and port (defaults 127.0.0.1 and 50001). For most installations — where Thetis and Commander Web run on the same PC — the defaults are correct. Click Save; the server tries the configured connections and reports the result right below the button.

The status window shows live information while the server is running: the TCI connection state, the number of currently-connected browsers, the URL each browser is using, and any error messages from Thetis or the network.

### 4.3 Opening the Page in a Browser

On the Thetis PC itself, open http://localhost:8765/. From any other device on the same network, replace localhost with the LAN IP address of the Thetis PC — for example http://192.168.1.42:8765/. The status window displays the exact URL to use; copy it to your phone or tablet by typing it into the address bar.

The page loads in a fraction of a second on any modern browser. The first thing you see is the connection bar at the top, where you tap Connect to establish the WebSocket link to the server. After Connect lights green, the rest of the interface comes alive — the frequency display populates with your current VFO, the S-meter starts reading, the bandscope begins streaming.

### 4.4 First-Time Browser Setup

Commander Web saves its settings in your browser's local storage. The very first time the page loads, defaults apply for everything — bandscope visible, waterfall on, all buttons visible in the grid, neutral colors. Two save patterns:

- Bandscope and waterfall sliders, the step size selector, and the connection-bar Auto-Reconnect toggle save themselves the moment you change them.
- Configuration page choices (button visibility, button colors, font size, button height, unkey delay, Flic key binding, Up/Down button colors) only persist when you click Save on the configuration page. Cancel discards the in-progress changes.

The next time you open the page on the same browser, the workspace comes back exactly as you left it.

> **Note:** Settings are per-browser, per-device. The same URL opened on your phone and your laptop are two independent configurations. You can give one device a CW-heavy button set and another device a digital-modes-heavy set, and they will not interfere with each other.

## 5. The Interface at a Glance

The Commander Web page is organized top to bottom as a single column that fits the width of whatever screen you're using. On a phone it is one long scrollable strip; on a desktop monitor everything fits on one screen without scrolling.

### 5.1 The Connection Bar

Across the very top is the connection bar, with elements in this order from left to right:

- A small colored status circle: grey when not connected, green when connected, amber and pulsing while reconnecting, red on error.
- A URL text box, pre-filled with the server address you're currently using.
- An Auto-Reconnect checkbox.
- The Connect / Disconnect button.

### 5.2 The Title Bar

Just below the connection bar is the program title — "Thetis Commander Web v1.0 — KC4VO". The version number tells you which release of the software is being served, useful when troubleshooting or when checking that your phone's cached copy is up to date.

### 5.3 The Frequency Display and S-Meter

A large green-on-dark frequency display (showing the current VFO in kHz.Hz format) takes up about 60% of the next row; an S-meter readout (e.g. "S7" or "S9+12dB") takes the remaining 40%. The frequency display updates in real time as you tune, whether the tuning originates from this browser, another browser, the Thetis main UI, or any external CAT controller.

### 5.4 Frequency Entry and Tuning Rows

Two rows sit below the frequency display:

- The top row holds the Enter Freq button (which opens a numeric keypad for typed-in tuning) and a Step size selector.
- The bottom row holds the four tuning buttons — Down 5, Down 1, UP 1, UP 5.

See Section 7 for details on tuning.

### 5.5 The Custom Button Grid

Below the tuning row is the custom button grid — the most user-configurable element of the interface. The grid draws from a fixed, hand-picked library of the most commonly-used radio controls: HF/MF band buttons, SSB / FM / AM mode buttons, filter-width presets, the noise-reduction and noise-blanker variants, Mute, RX2, PureSignal, Tune, VOX, VAC1/VAC2, and a few more. On a fresh install every button in this library is on the grid; the configuration page (gear icon, lower right) is where you tailor it — hide the buttons you never use, reorder the ones you do, and recolor them. See Section 9.

### 5.6 The Bandscope and Waterfall

Below the button grid is the bandscope panel — a real-time spectrum trace at the top, a scrolling waterfall below it. Both are fed from the same IQ stream that Thetis is processing. A drag handle at the bottom of the bandscope lets you resize the whole panel; a second drag handle between the bandscope and the waterfall lets you rebalance how much vertical space each gets. See Sections 10 and 11.

### 5.7 The Mic Meter

Tucked at the bottom of the bandscope panel is a small horizontal mic-level meter that comes alive whenever you transmit. It shows your microphone level. See Section 12.

### 5.8 The MOX Button

Below the bandscope panel is the large MOX button. Press to transmit. The button color and text change to indicate the current transmit state. See Section 13.

### 5.9 The RX1 / RX2 Selector

Below the MOX button is a two-position selector: RX1 and RX2. This switches the entire interface to control the other receiver. Each receiver maintains its own button grid, so you can have a CW-oriented set on RX1 and a digital-modes set on RX2 (or whatever combination suits you). See Section 14.

### 5.10 The Utility Toolbar

Across the bottom of the page is a small toolbar with four icon buttons, labelled:

- **Scope** toggles the bandscope panel on and off.
- **Sliders** toggles the slider panel for additional controls.
- **Refresh** forces an immediate state refresh from Thetis — useful if you suspect a display has gone stale.
- **Config** (gear icon) opens the configuration page for buttons, colors, and Flic hotkey binding. See Section 17.

## 6. Connecting to Thetis

### 6.1 What Thetis Needs to Have Enabled

For Commander Web to work, Thetis itself needs two of its standard services running — the CAT server (typically on TCP port 13013) and the TCI server (typically on port 50001). Both are built-in features of recent Thetis builds, accessed through the Thetis Setup screens; consult your Thetis documentation for the exact location of the CAT and TCI enable settings in the version you are running. Bind both services to 127.0.0.1 if the Commander Web server runs on the same PC as Thetis, or to 0.0.0.0 if you need other machines on your LAN to reach them directly.

Switching these services on is all that is required — no add-on, plug-in, or modified Thetis build is needed.

### 6.2 The Configuration Page

On first launch the Commander Web server's status window shows a small configuration form. Fill in the Thetis CAT host and port (defaults 127.0.0.1 and 13013) and the TCI host and port (defaults 127.0.0.1 and 50001). Click Test; the status window confirms that both connections succeed. Click Save to write the configuration to thetis_web_config.json (which lives next to the .exe) and bring the main server online.

### 6.3 Browser Connection

In the browser, the connection bar at the top shows the WebSocket URL the page is using. Tap or click Connect. The status indicator turns green and the rest of the interface becomes active. The browser maintains a once-per-second heartbeat to the server while the connection is live so that genuine network drops are detected promptly.

### 6.4 Auto-Reconnect

The connection bar carries a small Auto-Reconnect checkbox. When it is on (the default), the page automatically tries to reconnect to the server after any network drop. Useful on flaky WiFi or when switching between cellular and home network.

When using the page as a home-screen shortcut on iPhone, you may want to turn Auto-Reconnect off — the iPhone background-task limits sometimes cause the page to cycle through false disconnect/reconnect events when you bring it back into focus. With auto-reconnect off you tap Connect manually after waking the page; some operators find that predictability more comfortable than the automatic but occasionally noisy reconnect cycles.

> **Note:** Whatever you choose, the setting is remembered per browser. It does not affect any other device.

## 7. Frequency Display and Tuning

### 7.1 Reading the Display

The frequency display shows the current VFO frequency in kHz.Hz format with three decimal digits — for example 14205.010 means 14,205.010 kHz, which is 14.205010 MHz. The display updates as soon as the VFO changes, whether the change was caused by this browser, another browser, Thetis itself, or any other CAT controller (logging program, MIDI controller, etc.).

When the active receiver is RX1, the frequency shown is VFO A. When the active receiver is RX2, the frequency shown is RX2's tuned frequency. Switching between RX1 and RX2 with the selector at the bottom of the page swaps which VFO drives the display — the cached value for each receiver is shown immediately, so you do not have to wait for the next notification from Thetis.

### 7.2 The Up/Down Buttons

Four buttons sit below the frequency display: Down 5, Down 1, Up 1, Up 5. Each button changes the VFO by one step (Up/Down 1) or five steps (Up/Down 5) of the currently-selected Step size.

Tap once for a single step. "Press and hold" any of the four buttons and the VFO tunes continuously at a brisk repeat rate — useful for skipping rapidly across a band, jumping to a different segment, or finding a band edge without tapping dozens of times. The repeat starts after a short delay so a normal tap still produces a single step. Release the button to stop.

The four buttons have configurable per-RX colors so you can tell at a glance whether the display is showing RX1 or RX2 (see Section 17).

### 7.3 The Step Selector

Next to the Enter Freq button on the row above the tuning buttons is the Step selector — a dropdown listing the available step sizes:

- 10 Hz — SSB fine tuning.
- 100 Hz — casual band-scanning.
- 500 Hz — CW spacing.
- 1 kHz — jumping across a busy band.
- 100 kHz — jumping across band sub-segments.
- 1 MHz — jumping between bands.
- Snap — a special option that rounds the current VFO frequency to the nearest multiple of the currently-selected step size. Useful for getting back onto exact frequency boundaries after free-tuning. Selecting Snap fires the snap action immediately; the step selector returns to its prior step size afterward.

The step selection is saved per browser, so the step size you used last time comes back the next time you open the page.

### 7.4 Direct Frequency Entry

Press the Enter Freq button to open the numeric keypad overlay. The button's text changes to "Hide Keypad" while the overlay is open. Type a frequency in kHz (use the decimal point for Hz precision, e.g. 14205.010), then press Enter to commit — the VFO jumps to the typed frequency and the overlay closes automatically. The backspace key on the keypad removes the last digit if you make a typo. To close the overlay without committing a frequency, press the Hide Keypad button.

The keypad is sized for fingertip use on a phone or tablet — large, well-spaced buttons including digits 0-9, the decimal point, backspace, and a wide Enter button.

## 8. The S-Meter

### 8.1 Reading the S-Meter

The S-meter shows the current signal strength on the active receiver. The scale is the standard amateur convention: S9 corresponds to —73 dBm, with each S-unit representing 6 dB. Above S9 the reading shows "S9+nndB" — for example "S9+15dB" for a strong broadcast or local station.

The reading is fed from Thetis's own S-meter via the TCI rx_channel_sensors push notification — the same number Thetis itself shows on its on-screen meter. No polling, no additional load on Thetis: the radio simply broadcasts each new sample as it computes it, and the browser repaints the display.

### 8.2 Readable Display

The meter uses analog-style ballistics so the reading is easy to read at a glance: voice peaks latch onto the digit and hold long enough to register before falling back during pauses. Strong signals snap up instantly. The display does not flicker between values, even on a noisy SSB voice signal.

During transmit the S-meter is blanked to S0 — the receiver is muted while you transmit, so any value the radio reports would be meaningless. Live operation resumes automatically when you unkey.

## 9. Mode, Band, and Filter — The Custom Button Grid

The custom button grid is the most flexible part of the interface. Unlike Thetis's fixed control panels, you decide which buttons appear and in what order — so the controls you actually use sit on the page, and the ones you never touch do not.

### 9.1 What Is in the Library

The button library is a fixed, hand-picked set of the controls that are most useful on a remote control surface. It is not a catalog of every CAT command Thetis understands — it is a curated list. The current library includes:

- **Bands.** 160 m, 80 m, 60 m, 40 m, 20 m, 17 m, 15 m, 12 m, 10 m. (HF and 60 m only; the higher VHF and UHF bands are not in the library at this time.)
- **Modes.** LSB, USB, FM, AM. (CW and the digital modes are not currently in the library.)
- **Filter widths.** 5.0 k, 4.4 k, 3.8, 3.3 k, 2.9, 2.7, 2.4.
- **Receive-side toggles.** Two complete sets of noise-reduction and noise-blanker controls (NR2, NB1, NB2, ANF), Mute 1, Mute 2, RX2 on/off.
- **Transmit-related toggles.** Tune, VOX, 2 Tone, PS-A (PureSignal), Monitor.
- **Virtual audio cable toggles.** VAC1, VAC2.
- **Misc.** Lock A, Lock B, VFO Sync, Power.
- **Client-side action buttons.** Mobile (shrink to the compact MOX-only overlay) and Poor Cnx (enter Poor Connection Mode — see Section 15). These are not CAT commands; they affect only this browser session.

If a control you want is not in the library, it is not currently available as a button. New library entries are added in future versions of Commander Web as use cases come up.

### 9.2 Tailoring the Grid

The first time you open Commander Web, every available button is already on the grid. Most operators find that to be too many — a CW operator does not need the DIG modes visible, an SSB ragchewer does not need the 60-meter and 30-meter bands always on screen. The configuration page is where you trim the grid down to what you actually use.

Open the Configuration page from the gear icon in the utility toolbar (bottom right). The button library section lists every available button. Each row has a checkbox to show or hide that button, color swatches for its on and off states, and a pair of up (▲) and down (▼) arrow buttons for reordering. Uncheck the buttons you don't use, recolor the ones you do, tap the up/down arrows on any row to move it earlier or later in the grid, click Save, and the grid is rebuilt with your selection.

Toggle buttons (NR, MOX-style states, BIN, etc.) automatically light up when they are active and dim when they are not, mirroring the underlying Thetis state. Their on and off colors are independently configurable so the active state stands out.

### 9.3 Per-RX Button Sets

The button grid is saved per active receiver. When you switch from RX1 to RX2 (Section 14), the grid changes to whatever configuration you saved for RX2. This is useful when the two receivers are used differently — for example a band-busy RX1 with the full set of noise-reduction and band buttons in view, paired with a stripped-down RX2 showing just the few controls you care about there. Each receiver gets the layout that suits it.

## 10. The Bandscope

The bandscope is a real-time spectrum trace centered on the active receiver's tuned frequency. It shows you what is on the air around you at this moment — signals stand out as colored peaks against the noise floor, the colors tell you how strong each signal is, and a thin white vertical line shows the exact frequency you are tuned to.

### 10.1 Turning the Bandscope On and Off

The leftmost button in the bottom utility toolbar toggles the bandscope panel on and off. It is on by default. The waterfall (below the trace) has its own enable checkbox in the bandscope settings, so the bandscope can be shown without the waterfall, or both together.

### 10.2 The Trace and Its Colors

The color of the trace tells you how strong each signal is in S-meter units. The colors are tuned for normal ham-radio operating: green is noise and background, yellow says "there is something there", orange is a comfortable copy, red is a strong signal, and bright red is a booming one.

The colors are tied to absolute signal strength, not to screen position, so an S9 signal looks the same on every band — the meter does not interpret a strong 80-meter signal as different from the same strength on 20 meters. See Appendix B for the exact S-unit thresholds at which each color transition happens.

### 10.3 The Center Frequency Marker

A thin white vertical line runs from the top of the bandscope to the bottom at the center, marking the receiver's tuned frequency. White was chosen so the marker stays visible against the bandscope's red and bright-red colors when a strong signal sits on top of the tuned frequency.

### 10.4 Gain

The Gain slider in the bandscope settings controls the vertical scaling of the trace. Increasing Gain makes weak signals more visible by stretching the entire trace upward; decreasing Gain compresses everything down. The default 1.5x is a useful starting point that brings ordinary signals up to a readable height while keeping strong ones from disappearing off the top. Gain does not change the absolute color calibration — it just changes how much of the dynamic range is in view.

### 10.5 Zoom

The Zoom slider controls how much of the band you see. At minimum zoom the bandscope shows the widest possible view; at maximum zoom it shows a narrow slice centered on your tuned frequency. Useful for separating close-together signals or for dialing in on the carrier of one specific station.

### 10.6 Auto Ref and Recal

With Auto Ref on, the bandscope automatically adjusts itself so the noise floor sits at a comfortable position on screen, regardless of which band you are on. Switch bands and the display re-tunes itself within a second or two — you do not need to fiddle with controls each time.

The Recal button forces an immediate manual re-tune. Use it if you switch antennas, if a local noise source comes on or goes off, or any other time the band conditions change more sharply than the automatic tracker keeps up with.

### 10.7 Smooth

The Smooth slider is the bandscope's smoothness control. Higher values give a calm, easy-to-read display that averages out the jitter from one moment to the next; lower values make the display react faster at the cost of a noisier-looking baseline.

- Default 150 ms. A good general-purpose value.
- Raise toward 300—500 ms for steady SSB or digital modes where you want a calm display.
- Lower toward 50—100 ms for CW or fast-changing band conditions where you want the display to react instantly.

Smooth does not affect the calibrated colors — a strong signal still shows its true peak at any Smooth setting. It only changes how the noise floor looks.

### 10.8 During Transmit

The bandscope behaves very differently while you are keyed up, because it is doing a different job. In receive it is a wide-band signal hunter calibrated to S-units; in transmit it is an IMD shoulder viewer calibrated to dBc relative to your own carrier. The two modes share the same canvas but use entirely independent vertical mappings, which is what lets the transmit display do its job properly: tell you at a glance how clean your transmit signal is and whether your predistortion is working.

At key-down, four things happen automatically:

- The bandscope picks up the outgoing carrier in the center of the passband, finds its peak, and pins that peak to a fixed position about 15% from the top of the visible bandscope area. The peak stays there for the duration of the transmission, regardless of your output power, the band you are on, the antenna you are using, or how recently the noise-floor tracker calibrated. The display from peak downward becomes a calibrated dBc ruler — every pixel below the carrier corresponds to a known number of dB below the carrier.
- Auto Zoom (if enabled) snaps the bandscope to maximum zoom so the carrier and its IMD shoulders fill the spectrum width. You can see clean shoulder structure rather than a narrow spike lost in a wide-band view.
- The noise-floor tracker pauses (the spectrum during transmit is not representative of the receive noise floor anyway) and the receive-mode color gradient is set aside (the S-meter dBm thresholds it uses are meaningless on a peak-relative dBc scale). The trace is drawn as a clean line in the trace color so the shoulder shape reads clearly.
- An IMD3 estimate begins sampling. After a brief warm-up period a numeric "Est avg IMD3" readout appears near the TX Ref line and updates once per second, giving you a measurable companion to the visual shoulder you already see. When you unkey, the same display area switches to "Average IMD3 last transmission" — the average across all valid samples from the transmission you just made.

At unkey, everything reverts: the noise-floor tracker resumes, the zoom returns to your manual setting, the receive gradient and S-unit color thresholds come back. There is no manual recovery step.

Four controls in the bandscope settings panel govern the transmit display:

- **Show IMD3 estimate.** Master switch for the on-screen IMD3 measurement — both the live readout during transmit and the average displayed after you unkey. Untick to suppress those numbers without affecting any of the visual bandscope behavior (the carrier still peak-locks, TX Range and TX Ref still work). Default on.
- **Auto Zoom on TX.** When ticked (the default), the bandscope automatically snaps to the zoom level set by the TX Zoom slider below at key-down so the carrier and its IMD shoulders fill the display, and restores your manual Zoom setting at unkey. Untick if you prefer a fixed zoom across receive and transmit. If you toggle the setting while keyed, the zoom changes immediately — you do not have to unkey and re-key for it to take effect.
- **TX Zoom.** Target zoom percentage Auto Zoom uses when it snaps in at key-down. Higher values zoom tighter on the carrier and immediate shoulders; lower values leave more spectrum visible on either side. The right value depends on your Thetis sample rate — at 192 kHz a high zoom can crowd the signal and shoulders together, while at 384 kHz and above you have more breathing room. The slider is greyed out when Auto Zoom is off. Moving it while keyed applies the new value immediately, so you can find the level you like by eye during a real transmission. Range 0 to 95, default 85.
- **TX Range.** How many dB below the carrier the bandscope shows during transmit, measured in dBc (dB below carrier). The carrier always sits near the top of the visible area; this control sets where the bottom of the visible ruler is. Smaller values (e.g. 40—50 dBc) zoom in on the carrier's immediate shoulders, making small IMD products look prominent — useful for fine evaluation. Larger values (e.g. 90—100 dBc) zoom out, so the ruler stretches down to where the noise lives and only badly-degraded IMD reaches above the floor. The default of 70 dBc keeps clean PureSignal residuals visible without making them dominate. Range 40—100 dBc.
- **TX Ref.** An amber dashed reference line drawn horizontally across the transmit display at the dBc level you choose, with a small label in the top-right corner echoing the slider value. The point of the line is to mark the IMD floor you expect your predistortion to hold — PureSignal typically holds shoulder products around -50 to -60 dBc. Set the line to your target and the bandscope becomes a pass/fail tool every time you key up: shoulders staying below the line means predistortion is doing its job; shoulders climbing above the line means it has lost ground (wrong drive level, calibration drifted, band issue, etc.). The checkbox to the left of the slider switches the line and its label off entirely for operators who do not run PureSignal, in which case the dBc ruler still works as a measurement scale but has no marked threshold. Range -90 to -30 dBc, default -55 dBc, on.

Beyond the visual bandscope, two numeric readouts give you a measurable estimate of how much intermodulation distortion your transmit signal is producing:

- **Est avg IMD3 (during transmit).** Appears in white near the TX Ref line a few seconds after you key up. The scan covers the close-in IMD3 region (about 150 Hz to 1.5 kHz from carrier on the unwanted side, selected automatically based on the active mode), samples the loudest bin in that region every FFT frame, and smooths the result with a 2-second rolling median. The displayed value refreshes once per second so it stays readable rather than ticking with every frame. For about the first five seconds of each transmission it reads "Acquiring data..." while the FFT and PureSignal lock up and the rolling window fills with trustworthy samples; after that a dBc value (e.g. "Est avg IMD3: -53 dBc") takes its place and updates once per second for as long as you stay keyed. The 2-second median means the displayed value is stable enough to read but still reacts within about a second to real changes — PureSignal toggling on or off, a drive adjustment, amp warm-up, etc.

It is worth understanding what this measurement is and is not, because Commander Web is doing something different from a formal two-tone IMD3 test:

- **What this measurement is:** an estimated, time-averaged reading of the IMD3-region shoulder amplitude during your actual voice transmission. Over the course of a full transmission it converges to a stable representative number for whatever combination of radio, amplifier, drive level, and predistortion you have in play.
- **What this measurement is not:** a two-tone IMD3 test. A formal two-tone test uses a controlled, stationary signal and reads specific predicted distortion-product locations — the resulting number is precise and repeatable to a spec sheet. This continuous voice-driven measurement is different by design; it tells you what is actually happening with your voice signal, sample by sample, averaged for stability. A two-tone test on the same radio will give you a different but similar number; the difference is normal and reflects the different methodology, not a fault in either measurement.
- **What it is good for:** comparing settings on your own radio (PureSignal on vs. off, different drive levels, amp on vs. off), tracking changes over time, and giving you a number to discuss that is grounded in your actual operating signal rather than a test-tone scenario. For a spec-grade IMD3 number, run a two-tone test — Thetis can do this directly, and any bench spectrum analyzer will.
- **Average IMD3 last transmission (after unkey).** As soon as you unkey, the same screen area switches to the AVERAGE of every valid sample from the just-completed transmission, e.g. "Average IMD3 last transmission: -56 dBc". Stays on screen until your next key-down, giving you a single objective number per transmission rather than asking you to mentally average the live ticks. Useful for tracking "what was that one overall?" or comparing one transmission to the next. Very short transmissions (under about seven seconds total — three seconds of warm-up plus at least four seconds of sampling) collect too few samples to be meaningful and show "insufficient data" instead.

> **Note:** The redesigned transmit display assumes your transmit signal comes back through the Thetis IQ stream at a usable level. With normal PureSignal feedback wiring (a sample of the antenna port routed back to the radio), the carrier peak finds itself near the top of the FFT every time. If you transmit into a fully-isolated dummy load with no feedback path, the bandscope will still peak-lock onto whatever the strongest in-passband bin happens to be, which may not be your carrier — the visual is most meaningful when the radio is actually seeing its own transmit signal.

### 10.9 Appearance

Additional controls in the bandscope settings panel let you adjust the look without changing the underlying calibration. These all affect the receive-mode display only — during transmit the bandscope uses its own peak-locked rendering described in Section 10.8 and ignores these settings.

- **Palette.** Several built-in color schemes. The default Classic palette is what was described in Section 10.2 with its S-unit thresholds; the others use smooth gradients for a different look.
- **Gradient Fill.** Toggles the color fill below the trace. Off gives a line-only display.
- **Opacity.** How solid the gradient fill is. Lower opacity is more transparent; higher is bolder.
- **Trace Color.** When the gradient fill is off, the trace line is a single chosen color.

## 11. The Waterfall

The waterfall is the scrolling spectrogram below the bandscope trace. Each new horizontal line represents one moment in time, with the spectrum at that moment painted across the line as a row of colored pixels. Older lines scroll downward over time, building a visual history of activity on the band.

### 11.1 Enabling the Waterfall

In the bandscope settings panel, a Waterfall checkbox toggles the waterfall on and off. When on, the bandscope panel is divided between the trace (top) and the waterfall (bottom), with a drag handle between them to set the split. When off, the trace takes the full panel.

### 11.2 Low and High

Low and High set where the waterfall's color range starts and ends. They track the current noise floor automatically, so once you set them to your taste they keep working as you change bands.

- **Low.** Move down (more negative) to bring weaker activity into view — the noise itself starts showing low colors. Move up (less negative) for a cleaner background where only real signals get colored.
- **High.** Move down for higher contrast — ordinary signals saturate to the top color sooner. Move up for more dynamic range — only the loudest signals reach the top, weaker ones show in between.

Both inputs have — / + buttons next to them so you can step the value with one tap. On a phone or tablet the on-screen keyboard is intentionally suppressed; the buttons are the only way to change the value there.

### 11.3 Fade and Hide

Two sliders that work together to set what gets shown and how brightly:

- **Hide.** The threshold below which pixels stay black. Raise to clean up noise; lower to let faint activity show.
- **Fade.** The threshold above which pixels get the palette's full color. Raise to require stronger signals before they look bold; lower to let weaker signals brighten quickly.

### 11.4 Brightness

The Bright slider sets overall vividness. Higher for bold colors that are easy to read in daylight; lower for a softer look that is easier on the eyes during long evening sessions.

### 11.5 Palette

The waterfall shares the same palette dropdown as the bandscope. Switching the palette changes both at once.

## 12. The Mic Meter — Transmit Audio Monitoring

The mic meter is a slim horizontal bar at the bottom of the bandscope panel that becomes active during transmit. It shows the current microphone level in dB — the same kind of reading that Thetis's own on-screen mic meter shows.

### 12.1 What It Shows

The bar fills from left to right as your microphone level rises. The bar itself is a single solid color that you choose from the Mic Hue control in the bandscope settings panel — pick whatever color works for your eyes or matches the rest of your scheme. A numeric dB readout sits next to the bar showing the precise level.

Below the bar is a scale line that changes color at the 0 dB mark: white to the left of zero (the safe zone, where audio is below the ALC threshold) and red to the right (where Thetis's ALC starts intervening). The white-to-red transition on the scale is what tells you how close you are to clipping; the bar itself just shows the current level.

### 12.2 The Mic Cal Slider

Thetis's on-screen mic meter shows a slightly different reading than the value we receive over TCI (the underlying readings differ by about 13 dB on voice). The Mic Cal slider lets you add a fixed dB offset so this meter agrees with what you see on Thetis itself. Default 13 dB; adjust if your installation needs a different value. The setting is per-browser.

## 13. The MOX Button and Transmit Control

### 13.1 The Big MOX Button

Below the bandscope panel is the MOX button — a large, finger-friendly button labeled "Press for MOX". Press to transmit; press again to return to receive. While transmitting, the button changes color and its text changes to "Transmitting", and the S-meter is blanked to S0 (since the receiver is muted during TX).

### 13.2 What the Page Sees and What It Does Not

Commander Web is not connected to the radio's transmit hardware directly. It only knows about MOX events in two specific cases:

- When you press the MOX button on the page yourself. The page sends the key command to Thetis and updates its own visual state.
- When the page's Flic keyboard binding fires (see Section 17.3). This is the page listening for a key combination while it has browser focus; it then keys the radio the same way a manual MOX button press would.
- When an external program broadcasts on the MOX bus (UDP port 5011) — see Section 21. The page listens for those broadcasts and lights up its MOX button to match.

That is the complete list. Anything not in this list, the page cannot see. In particular:

- Pressing the hardware PTT button on your microphone is invisible to Commander Web.
- A foot switch wired to the radio is invisible to Commander Web.
- A CW key plugged into the radio is invisible to Commander Web.
- Another CAT program (logging, digital modes, etc.) sending its own transmit command to Thetis is invisible to Commander Web — unless that program also broadcasts on the MOX bus.

Why: the radio's transmit-state changes for any of those external triggers happen inside Thetis and on the hardware itself. There is no callback that tells Commander Web about them. The MOX bus exists precisely to give cooperating programs a way to coordinate transmit state, but participating in that bus is opt-in — a program has to choose to broadcast on UDP 5011, and most do not.

If you want hardware PTT presses (or any other off-page transmit trigger) to be visible to Commander Web and to other listeners on your network, you need a separate program that observes the radio's real-time MOX state and broadcasts it onto the MOX bus. The MOX Control Server is one such program (and the one Commander Web was designed alongside), but it is not bundled with Commander Web and Commander Web does not require it to operate. Without that or a similar bridge running, Commander Web simply does not display hardware-PTT transmit events.

### 13.3 Unkey Delay

The configuration page has an Unkey Delay setting (default 0 ms). When non-zero, releasing the MOX button leaves the radio keyed for that many additional milliseconds before sending the un-key command. Useful for short pauses between SSB phrases without re-keying each time, or for letting a final CW dit clear before the radio drops back to receive. Set to 0 for the most immediate unkey behavior.

## 14. RX1 / RX2 Operation

Commander Web supports both Thetis receivers. A two-button selector below the MOX button (RX1 on the left in green, RX2 on the right in grey) chooses which receiver is the current "active" receiver. The active receiver drives:

- The frequency display (its VFO).
- The S-meter (its signal level).
- The button grid (its per-RX button configuration).
- CAT commands sent from the page (mode, filter, NR, etc.).

Switching receivers is instantaneous; the cached frequency and S-meter for the receiver you're switching to appear immediately, so you're not staring at dashes while you wait for the next notification.

> **Note:** The bandscope and waterfall in v1.0 are tied to RX1 only — they continue to show RX1's spectrum even when the RX2 selector is active. Following the active RX into the bandscope is a planned improvement for a future release.

## 15. Poor Connection Mode — Operating Over a Bad Network

Commander Web is built to stream a lot of live data to your browser: a frequency update each time the VFO moves, an S-meter sample several times a second, a microphone level reading while transmitting, and a full bandscope frame at up to 30 frames per second. On a fast LAN this is invisible; on a flaky cellular connection, a hotel WiFi, or a long latency-prone VPN, all that traffic can stutter, delay your button presses, or run up your data plan. Poor Connection Mode is the answer.

### 15.1 What It Does

Tap the Poor Cnx button on the page and Commander Web does three things at once:

- Tells the server to stop sending every push stream — no more frequency updates, S-meter samples, mic-level readings, or bandscope frames cross the network.
- Unsubscribes from the bandscope entirely so even setup traffic stops.
- Replaces the page with a compact full-screen MOX overlay — a banner across the top (blanked frequency and meter readings to remind you they are intentionally not live) and a large MOX button below. Tap MOX to transmit; tap again to return to receive.

The network traffic drops to essentially nothing except the once-per-second heartbeat and the occasional MOX command. The radio still receives and transmits normally; you are simply telling Commander Web to be quiet so the connection doesn't get overwhelmed.

### 15.2 When To Use It

Poor Connection Mode is most useful in these situations:

- Operating from a cellular hotspot where bandwidth costs money and the live bandscope frames would burn through your data allowance.
- Using the radio remotely over a slow or laggy network where the constant push traffic is making MOX feel slow to respond.
- Walking around the house with your phone where you just want a quick way to key the radio for an over and don't need to see the bandscope or the meter.
- Anywhere you are listening through VDO.Ninja audio anyway — the audio tells you what is happening, you don't need the visual streams.

### 15.3 Entering and Exiting

Tap the Poor Cnx button on the page to toggle. The setting is remembered across page reloads — if you leave Poor Cnx on and close the page, the next launch comes back straight into the compact overlay. To exit, tap the small X in the top corner of the overlay; you return to the full interface and all the live streams resume.

### 15.4 The Status Message

When you enter Poor Cnx mode, Commander Web flashes a brief status message: "audio is your TX indicator". The reminder is deliberate — since the page is no longer showing live readings, your audio path (VDO.Ninja or however else you are hearing the radio) is the only feedback you have. If that audio path is not running, you are operating blind.

## 16. Adding to Your Home Screen — The PWA Experience

Although Commander Web is a browser page, every modern phone and tablet supports adding it to the home screen as a Progressive Web App (PWA). After adding, the page launches with its own icon, runs in full-screen without the browser's address bar visible, and feels indistinguishable from a native installed app — except there is nothing to install or update from any app store.

### 16.1 iPhone and iPad (Safari)

Open Commander Web in Safari. Tap the Share button at the bottom of the screen (the square-with-up-arrow icon). Scroll the share sheet and tap "Add to Home Screen". Adjust the name if you wish, then tap Add. An icon for Commander Web appears on your home screen; tap it to launch the app in standalone mode.

> **Important:** Safari is the only iOS browser that can install a PWA. Chrome and Firefox on iOS share Safari's underlying rendering but their "Add to Home Screen" entry creates a bookmark rather than a standalone app. Use Safari for the install.

### 16.2 Android (Chrome, Edge, Firefox, Samsung Internet, Brave)

Open Commander Web in your browser. Tap the three-dot menu in the top right and choose "Install app" or "Add to home screen" (the wording varies by browser). Confirm the install. An icon appears in your app drawer and home screen; tap it to launch in standalone mode.

### 16.3 Why Use the Home-Screen Shortcut

The standalone PWA has several advantages over a regular browser tab:

- No address bar takes up vertical space; the full screen is usable for radio controls.
- Launches with a single tap on its own icon, not via opening a browser and finding a bookmark.
- Survives closing the browser; the next launch is instantaneous.
- Behaves like a separate app for multitasking, screenshotting, and notification handling.

Your saved settings (button grid, bandscope preferences, etc.) are preserved across the transition from browser bookmark to home-screen icon, because they live in the same local-storage area that the underlying browser uses.

## 17. The Configuration Page — Buttons, Colors, and Flic

The gear icon in the bottom utility toolbar opens the configuration page. This is where you set up the button grid and the per-RX appearance details. The page is divided into sections; each section's settings are saved together when you click Save.

### 17.1 Button Font and Height

Two numeric fields control the size of the buttons in the grid: font size (8—24 pt, default 13) and button height in pixels (24—80, default 44). Larger values give a more touch-friendly grid at the cost of vertical space; smaller values pack more controls into less area. The defaults are tuned for finger use on a phone.

### 17.2 Unkey Delay

As described in Section 13.3, this sets the number of milliseconds the MOX button waits between release and sending the un-key CAT command. Default 0.

### 17.3 Flic Hotkey Binding

If you use a Flic button (a Bluetooth single-button accessory) configured to send a keyboard combination, this section lets you bind that combination to the page's MOX action. Pick the modifier keys (Ctrl, Alt, Shift) and the key, and the page will treat that combination as a MOX press whenever it has focus. Convenient for hands-free keying when your phone is mounted at the operating position.

### 17.4 Up/Down Button Colors per RX

Four color swatches let you pick the background colors of the four tuning buttons (Down 5, Down 1, UP 1, UP 5). The setting is per-RX, so you can give RX1 a green set and RX2 a blue set to know at a glance which receiver the page is controlling without looking at the RX selector. Tap any swatch to open the color picker (see Section 17.5).

### 17.5 The Color Picker

Every color swatch on the configuration page — the four Up/Down swatches above, and the on and off color swatches for each button in the library below — opens the same color picker when you tap it. The picker is the same on every platform: Windows, Mac, Linux, Android, iPhone, and iPad all see the same controls and the same workflow.

What appears when you tap a swatch:

- A gradient area at the top for picking saturation and brightness by dragging.
- A hue slider beneath the gradient.
- A small color preview that compares the original color (left) with your new choice (right) — at a glance you can see exactly how the change differs from where you started.
- A hex text field where you can type a specific color value like "#ff8800" if you know exactly what you want.
- A row of swatches at the bottom containing your recently picked colors (most recent first), followed by a small curated set of preset colors.
- A Cancel button (gray) and a Save button (green) at the very bottom.

How to use it:

- **Pick visually.** Drag inside the gradient and move the hue slider to find a color you like. The preview updates live so you can see the result before committing.
- **Type a specific hex value.** Click the hex field and type a six-character color code (e.g. "#cc0000"). Useful when you want to match a particular brand color or carry one over from another program.
- **Tap a swatch.** One tap on any swatch in the bottom row immediately loads that color into the picker. Useful for re-using a color you've picked before or for jumping to a preset.
- **Save or Cancel.** Save commits the color and closes the popup. Cancel closes the popup and restores the color that was selected when you opened the picker — a safety net if you've experimented and don't like the result.

Two workflow notes worth knowing:

- **Recent colors are remembered across sessions.** Every color you save is added to your personal swatches row. Close the browser, come back tomorrow, open the picker on any button — your recent colors are still there. The list holds the twelve most recently picked colors per browser.
- **Copying a color from one button to another.** Pick a color for the source button and save it. Open the picker for the destination button — the source color is the first entry in your recents row. One tap to apply, then Save. No hex numbers to write down, no trial and error.
- **Reverting after a mistake.** If you save a color and then realize you wanted the previous one back, reopen the picker on the same swatch. The color you had before is in the recents row (likely the second or third entry depending on what you have picked since). Tap it, save, you're back where you started.

### 17.6 Button Library

The bulk of the configuration page is the button library — a long list of every available CAT-driven button. Each row contains these editable controls:

- A visibility checkbox (uncheck to hide the button from the grid).
- An editable label text field showing the button's current display name.
- A small reset arrow (↺) that restores the label to its master default.
- Two color swatches: the button's on (active) color and its off (inactive) color.
- Up (▲) and Down (▼) arrow buttons for changing the button's position in the grid.

On a fresh install every checkbox is already on, so the grid in the main window shows everything. Use this page to uncheck the buttons you don't want, recolor the ones you do (using the picker described in Section 17.5), reorder, optionally rename, and click Save.

### 17.7 Renaming a Button

Every button in the library can be given a custom display name. Click into the label text field for any row and type whatever you want, up to ten characters. Common reasons to rename:

- Shorten a long default to fit a narrow phone-screen grid (e.g. "2 Tone" into "2T").
- Translate a CAT-protocol code into something friendlier (e.g. "PS-A" into "PureSig").
- Use a label that matches your local convention or another program's naming scheme.

Whatever you type in the label field becomes the visible caption on the button in the main grid. The underlying CAT command that fires when the button is pressed is unchanged — only the on-screen text differs.

If you want a previously-renamed button back to its factory label, click the small reset arrow (↺) next to the label field. The arrow is greyed out when the current label already matches the default, so an active arrow always means "you have changed this label and can undo it."

Label changes — like every other setting on this page — only take effect when you click Save at the bottom. Cancel discards them.

### 17.8 Reset, Cancel, Save

Three buttons at the bottom: Reset (restores all configuration to factory defaults), Cancel (closes the page without saving), and Save (writes all changes and rebuilds the interface). Changes are not committed until you click Save.

## 18. Settings, Persistence, and Resetting

Commander Web saves your settings in two places:

### 18.1 Server-Side Settings

The server's configuration (Thetis CAT host/port, TCI host/port, server port) lives in thetis_web_config.json next to the executable. It is read at startup and written when you change anything in the configuration page in the server's status window.

### 18.2 Browser-Side Settings

Everything else — button grid, bandscope and waterfall preferences, color choices, Mic Cal, Flic binding, Up/Down colors, step size, Auto-Reconnect — is saved in your browser's local storage. Each browser-on-each-device has its own independent set.

### 18.3 Resetting to Defaults

Two reset paths exist:

- **Reset bandscope/waterfall defaults.** A Reset button on the bandscope settings panel restores all bandscope and waterfall controls to their shipped defaults.
- **Reset all configuration.** The Reset button on the configuration page restores everything else (buttons, colors, font sizes, Flic, Up/Down colors) to defaults.

To completely wipe a browser's state and start over, clear site data for the page from the browser's settings.

### 18.4 Settings Version Migrations

When the program's defaults are improved between releases, a settings-version mechanism allows the new defaults to take effect on first run with the new version without requiring the user to manually reset. For example, v1.0 raised the default Smooth value from 60 ms to 150 ms; users upgrading from an earlier version see the new value on their next reload, and any value they explicitly chose on the slider continues to be preserved from that point onward.

## 19. Network Setup

### 19.1 LAN

On the same network as the Thetis PC, no setup is needed beyond knowing the PC's LAN IP address. The status window in the server displays the URL you should use. Type it into your phone's browser (http://192.168.x.y:8765) and you're connected.

Most home routers assign LAN IP addresses by DHCP, so the address can change over time. For convenience, assign your Thetis PC a static IP or a DHCP reservation in the router so the URL stays the same.

### 19.2 Remote Access

From outside your LAN you have the standard options:

- **Router port-forward.** Forward port 8765 (TCP) from your router's WAN address to the Thetis PC. Combined with a dynamic DNS service like duckdns.org or noip.com, you get a stable URL (e.g. http://myradio.duckdns.org:8765/) that works from anywhere.
- **VPN.** If you already use Tailscale, ZeroTier, or a similar mesh-VPN, your Thetis PC is reachable at its mesh IP from anywhere, with no port-forwarding required.

### 19.3 No HTTPS, and Why That Is Fine Here

Commander Web serves plain HTTP, not HTTPS. This is a deliberate design decision, not an oversight. There are two reasons:

- HTTPS requires server certificates. For a self-hosted hobbyist program, obtaining and maintaining a valid certificate is a non-trivial barrier and would prevent many hams from using the software.
- The single browser-only feature that strictly requires HTTPS is the microphone API — getUserMedia(). Commander Web deliberately does not include in-browser audio (see Section 20), so HTTPS isn't needed.

For controlling your own radio on your own network or over your own VPN, plain HTTP is appropriate. If you operate over a public network and feel uncomfortable with that, use a VPN like Tailscale, where every connection between your devices is automatically encrypted.

## 20. Audio — Set Up Your Own Audio Path

Commander Web is a control surface — it does not carry the radio's receive audio to you, and it does not carry your microphone audio back to the radio. The audio path is something you set up separately, in whatever way works best for your station.

A common, free choice is VDO.Ninja (vdo.ninja), a browser-based audio service that runs on every platform without installing anything. There are also paid and self-hosted alternatives. Whichever you pick, the setup is on your side of the radio — you will need to provide virtual audio cables on the Thetis PC to route audio in and out of Thetis, and you will need to generate the URLs and bind them to the right devices. Those details are specific to the tool you choose; they are not covered here.

In normal use you will have two browser tabs open on your remote device: one for Commander Web (radio control) and one for your audio tool (receive and transmit audio). Together they provide a complete remote operating experience.

## 21. MOX Bus Integration — Talking to Other Programs

The MOX bus is a network broadcast channel (UDP port 5011) over which cooperating programs announce when the radio is being keyed or unkeyed. Programs that already understand the bus — for example the separately-developed MOX Control Server, voice recorder add-ons, web-SDR auto-mute scripts, or Flic-button bridges — can coordinate their transmit-state awareness with each other. Commander Web listens on the bus and broadcasts to it:

- When any program on the bus reports MOX-active, Commander Web's MOX button turns red on every connected browser.
- When you press MOX in Commander Web, an MOX broadcast is sent so other bus participants can react (web SDRs auto-mute themselves, voice recorder starts capturing, etc.).

None of those external programs are bundled with Commander Web. They are separate projects, optional, and not required for Commander Web to function. If you do not run any of them, the MOX bus traffic is simply Commander Web talking to itself, and the page works normally for everything you can do from the page. If you do run one or more of them, they slot in as additional participants without any further configuration on your part.

As Section 13.2 notes, the bus is the ONLY channel through which Commander Web learns about transmit-state changes that did not originate on the page. Without a bus participant that observes the radio's real-time MOX state (such as the MOX Control Server), hardware PTT presses on your microphone, foot-switch closures, and CW-key activations are invisible to Commander Web.

## 22. Compatibility and Interoperability

Because Commander Web talks to Thetis through the standard CAT and TCI servers, it is compatible with everything that already works with Thetis.

### 22.1 CAT-Driven Programs

Logging programs (Log4OM, N1MM+, DXLab, etc.), digital mode programs (WSJT-X, JS8Call, JTDX, FLDIGI), CAT controllers (MIDI knobs via Midi2Cat, hardware tuners with CAT) — all continue to work unchanged. They communicate with Thetis directly and have no awareness of Commander Web. When they change the radio's state, Commander Web sees the change via TCI push notifications and updates its display in real time.

### 22.2 The Thetis Main UI

The Thetis main UI continues to work normally. You can operate Commander Web from your phone in one room and the Thetis main UI on the same PC in another room; both control the same radio and both stay in sync.

### 22.3 PureSignal, Diversity, MultiRX

All Thetis DSP and signal-processing features — PureSignal, diversity, multi-RX, noise reduction, advanced filtering — continue to operate exactly as they always have. Commander Web can drive any of them via its custom button grid; the buttons appear, click them, the underlying CAT command runs and Thetis takes the action.

## 23. Troubleshooting

### 23.1 Browser shows "Connecting..." and never connects

The most common cause is that the server is not running or is on a different IP address than the URL you're using. Check the status window on the Thetis PC to confirm the server is alive and to see the exact URL it expects.

Second most common: a Windows firewall is blocking port 8765. Either add an exception for the Commander Web executable, or temporarily disable the firewall to confirm that's the cause.

### 23.2 Frequency display says ----.--- and never updates

The server is connected to the browser but isn't getting frequency notifications from Thetis. Check that Thetis's TCI server is enabled in Setup → TCI Server, and that the configuration page in the server's status window points at the correct TCI host and port. Restart the server after changing TCI settings.

### 23.3 Bandscope is empty / waterfall is grey

Same root cause as the frequency display issue — TCI is not connected. The bandscope is fed entirely from TCI's IQ stream; if TCI is down, there's nothing to display.

### 23.4 Bandscope colors look wrong on one band

The color calibration takes a moment to settle after a band change. Wait a few seconds after QSY, or press Recal on the bandscope settings panel to force an immediate recalibration. If colors remain off after that, the band's noise floor may be unusually busy — the calibration uses the live S-meter to anchor itself, and a busy band makes that anchor less reliable.

### 23.5 Mobile keyboard does not include a minus sign

Resolved in v1.0. The Low and High waterfall inputs no longer rely on the on-screen numeric keyboard — they have dedicated +/— step buttons next to each field that work on any platform. The on-screen keyboard is intentionally suppressed on mobile so the buttons are the only path; desktop physical keyboards work as before.

### 23.6 Page on phone home screen launches but won't connect after sleeping

iOS in particular pauses background JavaScript aggressively. When you wake the page after the device has slept, the WebSocket may have been silently closed. If Auto-Reconnect is on, the page tries to recover by itself but sometimes goes through a noisy series of failed attempts. As a workaround, turn Auto-Reconnect off in the connection bar and tap Connect manually after waking the page (see Section 6.4).

### 23.7 MOX state on the page does not match the radio

Tap Refresh in the utility toolbar. The page re-queries all state from Thetis and resyncs the displays.

## Appendix A — Configuration File Reference

Commander Web stores its server-side configuration in thetis_web_config.json, located next to the executable. The file is plain JSON with the following keys:

- server_port (default 8765) — the TCP port the Commander Web server listens on for browser connections.
- thetis_host (default 127.0.0.1) — the IP address of the Thetis PC's CAT server.
- thetis_port (default 13013) — the TCP port of Thetis's CAT server.
- tci_host (default 127.0.0.1) — the IP address of Thetis's TCI server.
- tci_port (default 50001) — the TCP port of Thetis's TCI server.

Hand-editing the file is acceptable; the server reads it at startup. Changes via the status window are written back the same way.

## Appendix B — Bandscope Color Thresholds

The default bandscope color gradient is anchored to fixed absolute S-meter dBm values. The thresholds are:

- Green — below S7 (—85 dBm).
- Green-to-Yellow transition at S7 (—85 dBm).
- Yellow-to-Orange transition at S9+5 (—68 dBm).
- Orange-to-Red transition at S9+10 (—63 dBm).
- Red-to-Bright Red transition at S9+15 (—58 dBm).
- Solid Bright Red above S9+15.

Reading the chart: green is noise and background; yellow says "there is something there"; orange is comfortably readable; red is a strong signal; bright red is a booming local or commercial broadcast.

## Appendix C — Glossary

**Auto Ref** — The bandscope's automatic noise-floor tracking feature. When enabled, the display baseline follows the band's noise floor automatically; manual Recal forces an immediate re-tracking.

**Auto Zoom on TX** — Bandscope option that snaps the zoom to the TX Zoom slider value at key-down so your transmit carrier and IMD shoulders fill the spectrum width, and restores your manual zoom at unkey. Default on. See Section 10.8.

**Average IMD3 last transmission** — White text readout that appears in the top-right area of the bandscope as soon as you unkey, showing the average IMD3 estimate across the just-completed transmission. Stays until your next key-down. For very short transmissions (under ~4 seconds) displays "insufficient data" instead. See Section 10.8.

**Bandscope** — The real-time spectrum trace at the top of the bandscope panel.

**Button library** — The list of every CAT-driven button that can be added to the custom button grid via the configuration page.

**CAT** — Computer-Aided Transceiver protocol. The textual command interface Thetis uses for radio control; standard for all SDR programs and most hardware radios.

**Commander Web** — Short form of Thetis Commander Web, the program described in this guide.

**dBc** — Decibels relative to the carrier — a level expressed as how many dB it sits below (or above) a reference carrier signal. The transmit bandscope's vertical scale is in dBc, so the carrier itself is 0 dBc and IMD shoulders sit at, e.g., -55 dBc.

**Est avg IMD3** — Numeric estimate of IMD3-region shoulder amplitude shown in the top-right area of the bandscope during transmit, in dBc relative to the carrier. Continuous voice-driven measurement (not a two-tone test): samples the loudest bin in the close-in IMD3 region (~150 Hz to 1.5 kHz from carrier on the unwanted side) every FFT frame, smooths with a 2-second rolling median, refreshes once per second. Shows "Acquiring data..." for about five seconds after key-down (FFT/PureSignal settle ~3 seconds, rolling window fills ~2 seconds). Useful for relative comparisons (PS on/off, drive levels, amp on/off). A two-tone test gives a different but similar spec-grade number. See Section 10.8.

**Flic** — A Bluetooth single-button accessory. Commander Web can bind a Flic-issued key combination to its MOX action.

**IMD** — Intermodulation Distortion. The unwanted shoulder products that appear on either side of an SSB transmit signal due to non-linearity in the transmit chain. Reducing IMD is the purpose of PureSignal adaptive predistortion.

**IQ stream** — The raw spectrum data Thetis sends to Commander Web over TCI so the bandscope can be drawn. You do not need to know anything about it; it is mentioned here only so the term is not a mystery if you see it elsewhere.

**LocalStorage** — The browser-side persistent key/value store where Commander Web saves its per-browser settings.

**Mic Cal** — A per-browser slider that adds a fixed dB offset to the mic-meter reading so Commander Web's display agrees with what you see on Thetis's own on-screen mic meter.

**Poor Cnx** — A toggle that silences every push stream from the server and replaces the page with a compact MOX-only overlay. Use it when your network is slow or unreliable, or when you want to minimize data usage on a metered connection. See Section 15.

**MOX** — Manual Operate eXternal — the CAT command that keys the transmitter. Also the name of the big transmit button on the page.

**MOX bus** — A UDP-5011 broadcast channel shared by several programs in the author's ecosystem to coordinate transmit state. Commander Web listens and broadcasts.

**PureSignal** — Thetis's adaptive predistortion feature. It samples the transmit signal at the antenna port and continuously corrects the drive so the resulting transmit signal stays clean (low IMD). The transmit bandscope and TX Ref line are designed around verifying that PureSignal is doing its job.

**PWA** — Progressive Web App. The standard mechanism that lets a browser-based page be installed to a phone's home screen and launched as a standalone full-screen app.

**Recal** — The manual "recalibrate" button on the bandscope settings panel. Forces immediate noise-floor recalibration.

**Show IMD3 estimate** — Bandscope checkbox that master-enables the on-screen IMD3 measurement (both the live "Est avg IMD3" during transmit and the "Average IMD3 last transmission" after unkey). Untick to suppress those numbers without affecting any of the visual bandscope behavior. Default on. See Section 10.8.

**Smooth** — The bandscope's temporal averaging time constant. Larger values give a calmer display; smaller values give a more responsive one.

**Step** — The amount the VFO changes when one of the Up/Down buttons is pressed once. Selected from the Step dropdown next to the buttons.

**TCI** — Transceiver Control Interface, a WebSocket-based protocol that Thetis uses to expose IQ data, audio streams, S-meter readings, mic level, VFO, and MOX state to external programs.

**Thetis** — The SDR application that runs the radio. Commander Web is a remote front end for Thetis.

**TX Range** — Bandscope control that sets, in dBc, how far below the transmit carrier the visible display extends. Default 70 dBc; range 40—100 dBc. See Section 10.8.

**TX Zoom** — Bandscope slider that sets the target zoom percentage Auto Zoom snaps to at key-down. Higher values zoom tighter on the carrier and shoulders; lower values leave more spectrum visible. Default 85; range 0—95. Greyed out when Auto Zoom is off. See Section 10.8.

**TX Ref** — Bandscope control that places a horizontal amber reference line at a chosen dBc level during transmit, with a label echoing the value. Used to mark the IMD floor you expect from predistortion. Has its own on/off checkbox. Default -55 dBc, on. See Section 10.8.

**VDO.Ninja** — A free browser-based audio service used for the audio half of remote operating. Commander Web does not carry audio; VDO.Ninja handles it.

**VFO** — Variable-Frequency Oscillator. The radio's tuned frequency.

**Waterfall** — The scrolling spectrogram below the bandscope trace. Each horizontal line is one moment in time, with the spectrum painted across it.

---

*— End of user guide —*
