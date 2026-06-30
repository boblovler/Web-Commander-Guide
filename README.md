# Thetis Web Commander

by

Robert Lovler, PhD, KC4VO

User Guide V 4.3

*A universal browser-based remote control for Thetis SDR*

Based on Thetis 2.10.3.14 — runs on any device with a web browser

# 1. Introduction — What is Thetis Web Commander?

Thetis Web Commander is a browser-based remote control for the Thetis SDR application. It runs as a small server program on the PC where Thetis is running, and presents a clean, dark-themed control panel inside any standard web browser — on any device, on any operating system, anywhere on the network. Open a single URL and you have your radio in front of you. There is nothing to install on the client side and no app store involved.

The program is built around the standard Thetis CAT and TCI protocols. All radio functionality — tuning, mode changes, band switching, filter selection, MOX, the spectrum and waterfall, the S-meter, the microphone level meter, MultiRX operation — is driven through those protocols against an unmodified, stock copy of Thetis. Nothing about Thetis itself is changed. Web Commander is a new face on the front of the machine; the machine inside is the full Thetis you already know.

A few important things to understand before you go any further:

- Web Commander is not a replacement for Thetis. Thetis still runs on its host PC and does all of the real radio work — the hardware, DSP, audio paths, CAT command processing, and the IQ data stream that the bandscope is built from. Web Commander is a remote front end that connects to it.

- Web Commander does not carry audio. The audio path between your radio and your remote device is something you set up separately, outside this app. See Sections 8 and 28.

- Web Commander does not require any modification to Thetis. It uses the CAT server (port 13013) and the TCI server (port 50001) that ship with every recent Thetis build.

- Everything you customize in Web Commander — your button layout, your bandscope and waterfall preferences, your color choices, your hotkey binding — is saved in your browser and comes back automatically the next time you open the page.

# 2. The Two Sides — Client and Server

Although Thetis Web Commander is delivered as a single program, it is easiest to understand as two sides that meet over your network. One side — the server — runs on the Windows computer beside Thetis. The other side — the client — runs on the device in your hand. This guide is organized around that split: Part I covers the server on your desktop, and Part II covers the client on your remote device.

Only the client is universal. The browser that serves as your control panel runs on virtually any device and operating system — iPhone, Android, Windows, Mac, Linux, Chromebook. The server is Windows-only, which is no limitation in practice: Thetis itself is Windows-only, so the computer running your radio is already a Windows machine.

The server is actually several servers in one. The program you launch on the desktop quietly bundles three services: the control server that delivers the web page and relays your commands to Thetis; and two audio servers — the KC4VO audio server and the VDO.Ninja audio bridge — either of which can carry sound to and from your device. You start and configure all of them from the one status window.

The client is more than the browser. For control, the client is your browser. For audio, the client is either a tab your browser opens to VDO.Ninja, or a separate native app — the KC4VO audio app — installed on your phone or tablet. The native app is a genuinely separate program; the bundled servers are not.

At a glance:

- The server (your Windows desktop). One program that bundles the control server, the KC4VO audio server, and the VDO.Ninja audio bridge. You configure all of it from the status window. Covered in Part I.

- The client (your remote device, any operating system). Your browser for control, plus — for audio — a VDO.Ninja browser tab or the KC4VO audio app. Covered in Part II.

# 3. Universal Compatibility — Run on Anything

This is the part of Web Commander worth understanding before anything else: there is no client to install, there is no operating system requirement, and there is no app store. The client is your existing web browser — whatever browser, on whatever device, on whatever operating system you happen to have in your hand.

If a device has a modern web browser, it works:

- **iPhone or iPad.** Safari, Chrome, Firefox, Edge — all work. The page can be added to the home screen for a full-screen, app-like experience (see Section 25).

- **Android phone or tablet.** Chrome, Firefox, Samsung Internet, Edge, Brave — all work. Same home-screen install path as iOS.

- **Windows PC.** Edge, Chrome, Firefox, Brave, Opera — all work. Use it on a second monitor while your main Thetis screen runs on the primary monitor.

- **Mac.** Safari, Chrome, Firefox, Edge — all work. Same convenience as Windows; the Thetis PC itself can stay headless in the shack while you operate from a MacBook in the kitchen.

- **Linux desktop or laptop.** Firefox, Chrome, Chromium, Edge — all work. No driver, no compatibility layer needed.

- **Chromebook.** Built-in Chrome handles it natively. Useful as a cheap, dedicated control surface.

- **Linux ARM single-board computer (Raspberry Pi, etc.).** Any browser that runs on the device works.

There is one client and it is the browser you already have. There is nothing to download, no app store account required, no platform-specific code path, no signing or notarisation hurdle, no certification process. When you tap the URL on your phone or type it into your laptop, the entire user interface — frequency display, S-meter, custom button grid, real-time bandscope and waterfall, mic meter, MOX control — arrives as a single page that runs locally in the browser, talking back to the server over the network.

The practical consequence: the same operating experience is available on every device you own without any per-device configuration. A new phone with a fresh browser just needs the URL. A friend visiting the shack can pull up the radio on their own device without installing anything. The radio follows you around the house, the office, or the campsite, wherever you can reach the Thetis PC over the network.

**Important:** The browser is the universal client. That is the entire value proposition of Web Commander, and it is also why a few design decisions — no audio in the page, no HTTPS, no installable app bundle — were made deliberately. See Section 7 for the network-and-security reasoning.

# 4. How It Works — The Server-and-Browser Model

Web Commander has two parts: a small server program that runs on the same PC as Thetis, and a single web page that the server delivers to any browser that asks for it.

The server program talks to Thetis on your behalf, using Thetis’s own built-in services (no Thetis modifications required). The browser talks to the server. Everything you do in the browser — tap a button, move a slider, type a frequency — goes to the server, which forwards it to Thetis. Everything that happens at Thetis — the VFO moves, a signal arrives, you key the transmitter — comes back through the server and updates the browser in real time.

Multiple browsers can be connected at once, each with its own session. When you change something from one device, the other devices see the change within a fraction of a second.

# Part I — The Server (on your Windows desktop)

# 5. Installing and Starting the Server

## 5.1 Prerequisites

- A working installation of Thetis with the CAT server enabled (default port 13013) and the TCI server enabled (default port 50001). Both are standard features of recent Thetis builds.

- A Windows PC (10 or 11) for the Web Commander server. The server is shipped as a single self-contained .exe; nothing else needs to be installed alongside it.

- A network — a home LAN is enough. Remote access requires the usual port forward or VPN considerations (see Section 7).

- At least one client device with a web browser. Anything from a phone to a desktop computer.

## 5.2 Starting the Server

Copy Universal_Web_Commander_TCI_V4.1.exe to a folder of your choice on the Thetis PC and double-click it. A small status window opens, showing the version, the current configuration, and a URL of the form http://\<your-PC-IP\>:8765/ which you can give to your browser.

The status window is organised into seven tabs across the top: “Server”, “Audio (VDO.Ninja)”, “Audio (KC4VO audio server)”, “Security”, “Guest”, “Help”, and “Reset”. The Server tab is where the Thetis connection settings live; the two Audio tabs configure the optional built-in audio transports (Section 8); Security manages your personal access code that gates every browser connection (Section 7.4); Guest manages a separate temporary code for granting time-limited access to someone else without sharing your personal code (Section 7.5); Help shows browser connection guidance; Reset wipes every setting back to first-run state. On first run you need the Server tab to enter the Thetis connection settings and a glance at the Security tab to note the access code that browsers will prompt for.

On the Server tab, the form asks for five values: the Thetis CAT host and port (defaults 127.0.0.1 and 13013), the server port that browsers will connect to (default 8765), and the TCI host and port (defaults 127.0.0.1 and 50001). For most installations — where Thetis and Web Commander run on the same PC — the defaults are correct. Click Save; the server tries the configured connections and reports the result right below the button.

The Server tab shows live information while the server is running: the TCI connection state, the number of currently-connected browsers, the URL each browser is using, and any error messages from Thetis or the network.

# 6. Connecting the Server to Thetis

## 6.1 What Thetis Needs to Have Enabled

For Web Commander to work, Thetis itself needs two of its standard services running — the CAT server (typically on port 13013) and the TCI server (typically on port 50001). Both are built-in features of recent Thetis builds, accessed through the Thetis Setup screens; consult your Thetis documentation for the exact location of the CAT and TCI enable settings in the version you are running. Bind both services to 127.0.0.1 if the Web Commander server runs on the same PC as Thetis, or to 0.0.0.0 if you need other machines on your LAN to reach them directly.

Switching these services on is all that is required — no add-on, plug-in, or modified Thetis build is needed.

## 6.2 The Server Settings Window

On first launch the Web Commander server’s status window opens on the Server tab, which holds the Thetis connection form. Fill in the Thetis CAT host and port (defaults 127.0.0.1 and 13013) and the TCI host and port (defaults 127.0.0.1 and 50001). Click Save to bring the main server online; the status line reports the result.

# 7. Network Setup

## 7.1 LAN

On the same network as the Thetis PC, no setup is needed beyond knowing the PC’s LAN IP address. The status window in the server displays the URL you should use. Type it into your phone’s browser (http://192.168.x.y:8765) and you’re connected.

Most home routers assign LAN IP addresses by DHCP, so the address can change over time. For convenience, assign your Thetis PC a static IP or a DHCP reservation in the router so the URL stays the same.

## 7.2 Remote Access

From outside your LAN you have the standard options:

- **Router port-forward.** Forward port 8765 (TCP) from your router’s WAN address to the Thetis PC. Combined with a dynamic DNS service like duckdns.org or noip.com, you get a stable URL (e.g. http://myradio.duckdns.org:8765/) that works from anywhere.

- **VPN.** If you already use Tailscale, ZeroTier, or a similar mesh-VPN, your Thetis PC is reachable at its mesh IP from anywhere, with no port-forwarding required.

## 7.3 No HTTPS, and Why That Is Fine Here

Web Commander serves plain HTTP, not HTTPS. This is a deliberate design decision, not an oversight. There are two reasons:

- HTTPS requires server certificates. For a self-hosted hobbyist program, obtaining and maintaining a valid certificate is a non-trivial barrier and would prevent many hams from using the software.

- The single browser-only feature that strictly requires HTTPS is direct microphone access from the page itself. Web Commander does not capture audio on its own page; audio is handled by a separate transport (see Sections 8 and 28), so HTTPS isn’t needed for Web Commander proper.

For controlling your own radio on your own network or over your own VPN, plain HTTP is appropriate. If you operate over a public network and feel uncomfortable with that, use a VPN like Tailscale, where every connection between your devices is automatically encrypted.

## 7.4 Access Code Authentication

Web Commander gates every connection with a shared access code. The code lives in “thetis_web_config.json” on the Thetis PC and is required for every browser on every device. Without it there is no way past the login page, the WebSocket that powers the app refuses to open, and the radio is unreachable.

On first run, Web Commander auto-generates a 12-character random code from a safe alphabet (no 0/O or 1/l/I to avoid mis-read characters) and saves it to the config file. You manage the code from the Security tab in the status window.

When you open Web Commander in a browser for the first time, the login page appears: a single input field for the code and a Submit button. Type the code from the Security tab, tap Submit, and the application loads. A session cookie is set so subsequent visits in the same browser skip the login (see the cookie note at the end of this section). A wrong code shows a small “Wrong code” message and the form is ready for another try — no rate limiting, no lock-out.

The Security tab offers two actions for managing the code:

- **Set a custom code.** An input field plus a Set button. Type any code of at least 6 characters and click Set; Web Commander asks you to confirm, then writes the new code to the config file. The custom-code path exists primarily so you can choose something short and easy to type on a mobile keyboard. There are no character-set restrictions — all lowercase, words, numbers, or any mix is accepted — only the 6-character minimum is enforced.

- **Generate New Code.** Replaces the current code with a fresh 12-character random one from the safe alphabet. Confirmation dialog appears, and after you accept the new code is revealed in the display so you can copy it to your other devices.

Both actions overwrite the current code immediately for new logins. Devices already logged in stay logged in until their cookies expire on their own — changing the code only affects new logins. To force every device to re-authenticate at once (for example if you think a device has been compromised), change the code and then close and restart Web Commander. Restarting clears the in-memory list of valid session cookies, so every existing session is invalidated and every device has to log in again with the new code.

**Important:** Treat the access code like a password. Anyone with the code and your server URL can fully control the radio — change frequency, key the transmitter, run Tune, modify configuration. Use a long random code for any internet-facing deployment (port-forwarded router, public Wi-Fi, etc.). The custom-code option is a convenience for LAN-only or VPN-only deployments where the threat is bounded.

Session cookies behave differently across browsers. Chrome on Android treats closing the tab as the end of the session and re-prompts on next visit (the browser’s password manager usually offers to save the code, making re-entry a single tap). Safari on iOS treats tabs as semi-persistent — the cookie typically survives closing Safari, locking the phone, and even restarting the phone, so you log in once and then not again until you either clear Safari’s website data or restart Web Commander. Desktop browsers vary depending on their “Continue where you left off” setting. If you want a hard reset on every connected device at once, restart Web Commander on the Thetis PC; that invalidates every cookie everywhere instantly, regardless of how the browser handles session persistence.

## 7.5 Guest Code

Web Commander supports an optional second access code for granting temporary access to someone else — a fellow operator, an Elmer, a friend you want to demonstrate the station to — without sharing your personal access code. The guest code is managed from the Guest tab in the status window and is empty by default; you only set one when you actually want to grant guest access.

The guest code is different from the primary access code in one important way: it carries an expiration time that you pick when you set it. When the time runs out the code stops working and anyone currently logged in with it is immediately kicked off. You cannot grant guest access for longer than four hours in a single sitting — this is a deliberate limit that protects you from accidentally leaving guest access open overnight or longer. If your guest needs more time, you can issue a fresh code at any point.

The Guest tab offers three actions:

- **Set a custom guest code.** Type a code of at least 6 characters and choose a duration from the radio buttons (15, 30, or 60 minutes) or type a custom number of minutes up to 240. Click Set, confirm the duration in the prompt that appears, and the code becomes active.

- **Generate Random Guest Code.** Mints a fresh 12-character random code using the selected duration. The newly-generated code is revealed on the tab so you can copy it to the guest’s device.

- **Revoke Now.** Stops the guest code from working immediately and disconnects any device currently logged in with it. Use this when the guest is done or if you want to lock the access down before the timer expires.

A status line at the top of the Guest tab shows the current state at a glance: “No guest code set.”, “Guest code valid for H:MM (HH:MM).”, or “Guest code expired.” The remaining time updates automatically.

**Important:** The guest has the SAME control over the radio as you do. They can change frequency, key the transmitter, run Tune, modify configuration. Only grant guest access to someone you trust to operate your station, and prefer the shortest duration that fits the situation.

# 8. The Bundled Audio Servers

Both audio transports are powered from the server side: their servers are bundled into the Web Commander program and run on your Windows desktop. You set them up here, at the radio computer; you choose and connect a transport from your device, which is covered in Section 28.

Both transports rely on the same prerequisite on the Thetis PC: two virtual audio cables (VACs) wired the standard way — radio output to VAC1 (receive audio), VAC2 to the radio input (transmit audio). Thetis should be configured to use the Windows DirectSound driver model for the two VACs rather than MME. DirectSound is significantly more reliable under continuous use.

**Important:** Both transports are configured from the main Web Commander status window. The status window is organised into tabs: Server, Audio (VDO.Ninja), Audio (KC4VO audio server), Security, Guest, Help, and Reset. Each transport has its own tab; the Server tab holds the Thetis CAT and TCI settings; the Security tab manages the access code (Section 7.4); the Guest tab manages a separate temporary code for granting time-limited access (Section 7.5).

## 8.1 VDO.Ninja Bridge — Setup

On the radio PC, Web Commander opens a hidden VDO.Ninja browser window in the background that connects to VDO.Ninja using your VACs. You never see this window; it runs invisibly. To set it up, open the Audio (VDO.Ninja) tab and run the Setup Wizard:

- Open the Audio (VDO.Ninja) tab in the status window.

- Type your callsign into the Callsign field. It identifies your station to VDO.Ninja.

- Click “Run Setup Wizard”. A four-page wizard walks you through device selection. A VDO.Ninja window opens for device picking, positioned beside the Web Commander window rather than on top of it.

- In the VDO.Ninja window, set “Audio Source” to the VAC carrying receive audio from the radio, and “Audio Output” to the VAC carrying transmit audio to the radio. You do NOT need to click Start in VDO.Ninja, and you can ignore any “settings will be lost” warning — your device picks are saved as you make them.

- Click “Done” in the wizard (or close the VDO.Ninja window). The audio bridge runtime launches automatically off-screen and runs invisibly from then on. Tick “Enable audio bridge on Web Commander start” and click Save if you want the bridge to relaunch automatically every time Web Commander runs.

Once set up, the Audio (VDO.Ninja) tab gives you two ways to control the bridge — the checkbox above, which decides whether the bridge auto-starts at next Web Commander launch, and the Start Bridge / Stop Bridge buttons on the same row as Run Setup Wizard and Show Host Window, which control the runtime right now. The buttons grey to indicate state: Start is greyed when the bridge is alive, Stop is greyed when it is not. They are independent of the checkbox — you can leave Enable-on-start ticked but Stop the bridge mid-session, or leave it unticked and Start manually, whichever you want.

Start Bridge stays greyed on a fresh install until two prerequisites are met: a callsign is set in the field above, AND the Setup Wizard has been run at least once. Before either is true, launching the bridge would just bring up Chromium with no saved device picks (audio wouldn’t actually route through the VACs to Thetis), so the button stays inactive to nudge you toward the wizard first. Reset All Settings and Clear Saved Devices both return you to that pre-setup state and re-grey the button.

The VDO.Ninja tab also offers:

- **Show Remote URLs.** Opens a popup listing three VDO.Ninja URLs as copyable text: the Host (runtime) URL the off-screen Chromium loads on the radio PC, the Phone speaker URL (echo cancellation on), and the Headphones URL (no echo cancellation). Each has a Copy button. Useful for bookmarking, sharing, or reproducing a side of the link manually in another browser. A “How to get the URL onto your phone” button opens a help popup describing two out-of-band delivery paths (browser bookmark sync, draft email sync) for the rare case where the operator wants the URL outside the Web Commander web UI.

- **Configure.** Opens VDO.Ninja’s device picker for changing device choices later, alongside a small helper window with a “Done” button. Clicking Done closes the VDO.Ninja window cleanly. If the audio bridge runtime was already running when you pressed Configure, it is briefly stopped for the device check and then automatically restarted when you click Done — the Enable flag is not changed.

- **Clear Saved Devices.** Wipes the bridge’s memory of which VACs to use — useful before reassigning cables. Asks for confirmation in a styled dialog first. Also re-greys Start Bridge until you run the wizard again.

- **Show Host Window.** Reloads the audio bridge in a visible window with VDO.Ninja’s full UI showing (master gain slider, device selectors, connection state) so you can see what the runtime is actually doing. Click again to send it back off-screen. Audio drops for — roughly two seconds during each switch while VDO.Ninja reconnects, so use it for inspection rather than as a normal-operation control. Greyed when no bridge is running.

- **Status line.** Tells you whether the audio bridge is alive and what mode it is in.

**Prerequisite:** Google Chrome or Microsoft Edge must be installed on the radio PC. Web Commander uses whichever is present to run the hidden VDO.Ninja window.

## 8.2 KC4VO Audio Server — Setup

This is the audio server for KC4VO’s native apps. You start it here on the desktop; you connect to it from the app on your device (Section 28.2).

Once the remote app is on your device, configure the radio side:

- Open the Audio (KC4VO audio server) tab in the Web Commander status window.

- Click “Start Audio Server”. A second window opens — this is the audio server itself. Pick your VAC RX as “Input Device” and your VAC TX as “Output Device”. The server remembers your picks, port, and window position between launches.

- Tick “Enable audio server on Web Commander start” and click Save if you want the audio server to launch automatically every time Web Commander starts.

- Back on the Web Commander Audio (KC4VO audio server) tab, click “Show Connection Info”. A popup displays this PC’s LAN IP address and the audio server’s port.

The Start and Stop Audio Server buttons reflect actual state — Start is greyed when the server is already running, Stop is greyed when it is not. Closing the audio server’s window directly is a clean shutdown; Web Commander notices and updates the tab’s status line within a few seconds.

**Note:** Web Commander refuses to close while the audio server is running, with a popup telling you to close the audio server’s window first. This is so the audio server’s own quit handler runs, persisting any device or window-position changes you made during the session before it exits.

## 8.2.1 AirBrake Reduction

When you operate remotely and listen on a phone or tablet’s built-in speaker while also using its microphone, keying the transmitter can briefly put a burst of noise on the air. At the instant you key, the last fraction of a second of received audio is still coming out of the device’s speaker, the microphone picks it up, and that sound is sent to the radio and transmitted.

AirBrake Reduction significantly reduces this burst. For a brief, adjustable moment right after you key, it holds the device’s microphone off the radio, so most of the leftover speaker sound is held back rather than transmitted. It does not eliminate the burst entirely — a short remnant at the very start can still get through — but it cuts it down to a small fraction of its original level.

The control is in the audio server window, in the “Mic Gate (TX Burst Guard)” box. The slider sets the hold time from 0 to 1000 milliseconds; the default is 650. Changes take effect at once and are remembered between sessions.

Setting it: 650 milliseconds suits most operators — that is about the natural pause between keying and beginning to speak, so it clears most of the burst without clipping your first word. If too much still gets through, raise it; if the start of your speech is being cut off, lower it. Setting it to 0 turns it off.

Listening on headphones avoids the problem entirely — with no speaker, there is nothing for the microphone to pick up — so this control only matters when you use the device’s speaker and microphone together.

# 9. The LP100A Wattmeter

The TelePost LP-100A is an external digital wattmeter that sits in your feedline and measures the power going to the antenna and the SWR at that point. If you have one, Web Commander can read it and put its forward power and SWR on the page alongside the radio’s own figures — so you can watch true output power on any device, the same browser screen you use for everything else, with nothing extra to install on the remote side.

The meter connects to the Thetis PC by USB. Web Commander finds it on its own; there is no COM port to choose. It identifies the meter by the unique serial number built into its USB-to-serial cable, so it keeps finding the same meter even when Windows assigns it a different COM port from one session to the next. If the meter does not turn up automatically — for example on a non-FTDI cable — you can point Web Commander straight at its COM port (Section 9.4).

When a meter is connected its readings appear in two places: a small power/SWR window you can open on the radio PC’s own screen (Section 9.1), and — once you add them — a pair of readout buttons in the browser (Section 9.2). When no meter is connected, both simply show nothing; the feature stays dormant and out of the way for operators who do not own one.

## 9.1 The Desktop Readout Window

The server’s Server tab has an “LP100A Window” button that opens a small black window showing forward power and SWR in large type — useful when you are sitting at the radio PC and want the reading on that screen while you operate. It reads dashes until you transmit. Close it whenever you like; it does not affect the browser readouts, which run independently.

A status line in the server window’s always-visible area reports what was found — “LP100A: found on COM9” when a meter is detected, or “LP100A: not found” when none is — so you always have explicit confirmation of whether the meter was located and on which port.

## 9.2 The Readout Buttons in the Browser

Power and SWR reach the browser as four optional buttons you add from the button library (Section 18), the same way you add any other button. They come in two pairs:

- Exciter power and SWR — the radio’s own output, as Thetis reports it. When you run an amplifier, this is the drive level out of the radio, ahead of the amplifier.

- Output power and SWR — the LP100A’s reading, taken in the feedline after the amplifier. This is the true power going to the antenna and the SWR the antenna system actually presents.

With no amplifier in line, the two pairs read the same thing. With an amplifier they differ — the exciter pair shows drive and the output pair shows amplified output — two genuinely different measurements, which is why both are offered.

On the buttons themselves, the two pairs are told apart by case: the exciter pair carries lower-case labels (pwr, swr) and the output pair upper-case (PWR, SWR). All four are off by default; check the ones you want on the Configuration page and place them wherever you like in the grid. Each carries its live reading on the button face. They sit at their resting color while you receive and light up — red, like the MOX button, by default — while you transmit, so a glance tells you the radio is making power. That transmit color is yours to change per button in the color picker, exactly like any other button.

The output (LP100A) buttons grey out and read “n/a” whenever no meter is present, so they never show a stale or misleading number. The exciter pair needs only Thetis and works on every station; the output pair needs the LP100A.

## 9.3 Choosing Among Multiple Meters

If you have more than one LP100A — for example one per feedline — the server’s “LP100A Setup” button, beside the LP100A Window button on the Server tab, lets you choose which meter drives the browser’s output power and SWR buttons. It opens a window that scans and lists every meter it finds, each shown by its COM port and serial number. Select the one you want and click “Use selected”; that meter feeds the readout from then on, and the choice is remembered by serial number, so it survives restarts and never swaps if COM numbers change. With a single meter connected you never need this window — that meter is used automatically.

## 9.4 Connecting Manually

Web Commander finds the meter automatically only when it is on an FTDI-type USB-to-serial cable — the common kind, and the one the automatic scan looks for. If your meter is on a different cable, or the scan simply does not turn it up, the same LP100A Setup window lets you point Web Commander straight at the meter’s COM port instead.

In the Manual connection box, type the COM port the meter is on — either the full name (COM9) or just the number (9) — and click Connect to this port. Web Commander goes directly to that port, confirms an LP-100A is answering there, and uses it from then on, skipping the automatic scan entirely. The choice is remembered, so it returns on the next start.

The LP-100A’s own serial values are 115200 baud, 8 data bits, no parity, one stop bit (115200, N, 8, 1). When the meter is found automatically on a USB-to-serial cable, this is handled for you. When you connect to a port manually — especially a built-in serial port rather than a USB cable — first make sure Windows itself has that port set the same way: open Device Manager, find the port under Ports (COM & LPT), open its Properties and then Port Settings, and confirm Bits per second is 115200, Data bits 8, Parity None, and Stop bits 1. If those do not match — for example a port left at the Windows default of 9600 — the port will still connect but the meter’s readings will not decode, and the power and SWR stay blank until the port is set to 115200, N, 8, 1.

A manual port takes precedence over everything else. While it is set, Web Commander uses only that port and does not fall back to scanning, so if nothing answers there — a wrong port, or the meter switched off — it simply reports the meter as not found rather than looking elsewhere, which makes a mistyped port easy to spot. Click Back to automatic detection to return to normal scanning.

If you are not sure which port the meter is on, Windows Device Manager lists it under Ports (COM & LPT).

# 10. MOX Bus Integration — Talking to Other Programs

The MOX bus is a shared channel over which cooperating programs announce when the radio is being keyed or unkeyed. Programs that already understand the bus — for example the separately-developed MOX Control Server, voice recorder add-ons, web-SDR auto-mute scripts, or Flic-button bridges — can coordinate their transmit-state awareness with each other. Web Commander listens on the bus and broadcasts to it:

- When any program on the bus reports MOX-active, Web Commander’s MOX button turns red on every connected browser.

- When you press MOX in Web Commander, an MOX broadcast is sent so other bus participants can react (web SDRs auto-mute themselves, voice recorder starts capturing, etc.).

None of those external programs are bundled with Web Commander. They are separate projects, optional, and not required for Web Commander to function. If you do not run any of them, the MOX bus traffic is simply Web Commander talking to itself, and the page works normally for everything you can do from the page. If you do run one or more of them, they slot in as additional participants without any further configuration on your part.

As Section 22.2 notes, the bus is the ONLY channel through which Web Commander learns about transmit-state changes that did not originate on the page. Without a bus participant that observes the radio’s real-time MOX state (such as the MOX Control Server), hardware PTT presses on your microphone, foot-switch closures, and CW-key activations are invisible to Web Commander.

## 10.1 Starting the MOX Control Server from Web Commander

Web Commander can start and stop the MOX Control Server for you, so you no longer have to launch it separately. The status window has a MOX Server tab, alongside the Server and Audio tabs, with its own controls.

Running the MOX Control Server powers a few specific things. It provides the Scroll Lock and Ctrl+F12 keyboard PTT hotkeys on the radio PC. It is also what a Flic button keys through when the Flic is driven by the FlicPTT app for universal control — a Flic set up as a plain keyboard, and the MOX button on the web page itself, do not need it, since they key the radio by other paths. And it keeps the live connection to Thetis that lets Web Commander see hardware PTT, foot-switch, and CW-key activations, as described above, broadcasting those events so the page and the remote audio apps stay in step. If you do not use any of those, you do not need to run it.

The MOX Server tab offers:

- **Start MOX Server.** Opens the server in its own window straight away. This is a one-time launch — it does not change the auto-start setting below. The button is greyed while the server is already running.

- **Stop MOX Server.** Closes that window. Greyed while the server is not running.

- **Enable MOX server on Web Commander start.** When ticked, the server launches automatically every time Web Commander starts, so it is always there. Leave it unticked to start the server only when you choose to.

- **Status line.** Tells you whether the server is idle or running.

The server appears in its own window, exactly as it does when run on its own, and keeps its own settings (its Thetis connection details and hotkey behaviour). When you close Web Commander it closes the MOX Control Server for you.

# 11. Server Settings

The server’s configuration (Thetis CAT host and port, TCI host and port, web server port, audio bridge callsign and enable flags, the access code, main window position) is saved on the Thetis PC next to the .exe in “thetis_web_config.json”. It is read every time Web Commander starts, and is written whenever you click Save in the server’s status window or change the access code in the Security tab.

# 12. Reducing Load on the Radio Computer

Because the bandscope is built on the server from Thetis’s data over the TCI link, remote operation does not depend on Thetis drawing its own display. That gives you a way to lighten the load on the radio computer with no loss of remote function:

- Minimize Thetis for the biggest saving. It stops rendering its window, yet your remote frequency, S-meter, and bandscope keep working exactly as before. For a dedicated remote station, this is the ideal low-load mode.

- Data Saver runs the whole program in a few percent of CPU — well suited to lower-powered radio computers and tablets.

- Poor Connection Mode is lighter still, adding essentially nothing to the computer’s load.

# Part II — The Client (on your remote device)

# 13. Opening the Page and Logging In

## 13.1 Opening the Page in a Browser

On the Thetis PC itself, open http://localhost:8765/. From any other device on the same network, replace localhost with the LAN IP address of the Thetis PC — for example http://192.168.1.42:8765/. The status window displays the exact URL to use; copy it to your phone or tablet by typing it into the address bar.

The first time a browser visits the server, a small dark-themed login page appears asking for an access code. Find the code in the Security tab of the status window, type it into the field, and tap Submit. The application loads, and the browser remembers the login so subsequent visits skip this step. Section 7.4 covers the access-code system in detail.

After login, the page loads in a fraction of a second on any modern browser. The first thing you see is the connection bar at the top, where you tap Connect to link to the server. After the button turns red and reads “Disconnect”, the rest of the interface comes alive — the frequency display populates with your current VFO, the S-meter starts reading, the bandscope begins streaming.

## 13.2 First-Time Browser Setup

Web Commander saves its settings in your browser’s local storage. The very first time the page loads, defaults apply for everything — bandscope visible, waterfall on, all buttons visible in the grid, neutral colors. Two save patterns:

- Bandscope and waterfall sliders and the step size selector save themselves the moment you change them.

- Configuration page choices (button visibility, button colors, font size, button height, unkey delay, Flic key binding, mic meter calibration and color) only persist when you click Save on the configuration page. Cancel discards the in-progress changes.

The next time you open the page on the same browser, the workspace comes back exactly as you left it.

**Note:** Settings are per-browser, per-device. The same URL opened on your phone and your laptop are two independent configurations. You can give one device a CW-heavy button set and another device a digital-modes-heavy set, and they will not interfere with each other.

# 14. Connecting and Staying Connected

## 14.1 Browser Connection

In the browser, tap or click Connect on the connection bar. The button turns red and reads “Disconnect”, and the rest of the interface becomes active. The browser maintains a once-per-second heartbeat to the server while the connection is live so that genuine network drops are detected promptly.

## 14.2 Automatic Reconnection

Web Commander reconnects automatically whenever the connection drops. There is no setting to configure — the behavior is on, always, for every browser.

Short network interruptions are absorbed without any reconnect happening. A brief stall (for example a normal cellular tower handoff) is invisible to you — the badge stays green. If the stall extends past a couple of seconds the power-icon badge turns amber as a warning that the system has gone quiet, but the connection itself is left alone and may recover on its own. Only if the silence continues for around twelve seconds does Web Commander give up on the existing connection and start trying to open a fresh one. The reconnect attempt runs every two seconds for as long as it takes, without slowing down or giving up. When the network is genuinely back, the badge returns to green typically within two seconds.

Reconnect attempts keep running even when your phone screen is off and the page is in the background. If you lose connection while your phone is in your pocket, it is usually already reconnected by the time you pull the phone out.

## 14.3 Staying Connected on the Move

Web Commander rides out the rough patches of mobile operation rather than dropping the connection at the first sign of trouble.

- Brief signal dips — a tower handoff, a weak corner of the property — are ridden out quietly. Your meters and bandscope pick up where they left off the moment the signal recovers, with no disconnect-and-reconnect cycle.

- Switching networks — when your phone moves from Wi-Fi to cellular, control reconnects within about two seconds. On Android, VDO.Ninja WebRTC audio then restarts itself about a second later. The phone’s own switch-over time is separate and up to the phone; walking slowly out of Wi-Fi range, it may cling to the fading signal for a while, so the whole change can take ten to fifteen seconds.

- Stepping away — setting the phone down or switching to another app for a few minutes does not cost a reconnect. Pick it up and everything is still live.

- If audio cannot restart itself — for example, after the page was reloaded mid-session — the status bar tells you exactly what to tap.

- The amber connection button cancels reconnection attempts when tapped.

On iPhone, audio recovery uses VDO.Ninja’s own mechanism and can take up to about thirty seconds rather than restarting automatically.

# 15. The Interface at a Glance

The Web Commander page is organized top to bottom as a single column that fits the width of whatever screen you’re using. On a desktop monitor everything fits on one screen without scrolling. The page wears a hardware front-panel style throughout — brushed-metal pushbuttons, recessed sliders, and a lit MOX button — matching the Large-Screen Layout described in Section 29.

On a phone, the controls that matter most are held in place so they are always in view. The connection bar and the frequency / S-meter readout are pinned at the top, the microphone meter sits just beneath them, and the MOX button and the utility toolbar are pinned at the bottom within easy thumb reach. Everything in between — the bandscope, your button library, and the sliders — scrolls in that middle space, so no matter how far you scroll you never lose sight of your frequency, your signal, your microphone level, or your transmit control. The tall single-column (portrait) layout keeps its header fixed in the same way.

## 15.1 The Connection Bar

Across the very top is the connection bar, centered horizontally. Elements in order:

- A small round on/off button with a standard power symbol on its face (a circle with a vertical line broken through the top). Color carries the connection state: red when disconnected, green when connected, amber (pulsing) when the connection is stalled or reconnecting. Tap to toggle. The text labels (“Connect” / “Disconnect” / “Reconnecting...”) that earlier releases used are gone — the icon and color carry the same information without consuming bar width on narrow iPhones.

- Four VDO.Ninja audio icons: 🔊 (phone speaker, AEC on), 🎧 (headphones, no AEC), ⏹ (stop), and 🎚 (open Speaker volume popup). These icons are hidden by default; they appear on the bar only when you turn on “VDO.Ninja Audio Buttons” on the Configuration page (see Section 28.1). When shown, each icon acts directly when tapped, and all four stay greyed until the host audio bridge is running. While audio is streaming, the active mode shows a gold ring and the OTHER mode is greyed to enforce Stop-first switching. Re-tapping the active mode acts as a reconnect.

## 15.2 The Title Bar

Just below the connection bar is the program title — “Thetis Web Commander V4.3”. On a phone the power (connect) button shares this title line to save vertical space. The version number tells you which release of the software is being served, useful when troubleshooting or when checking that your phone’s cached copy is up to date.

## 15.3 The Frequency Display and S-Meter

A large green-on-dark frequency display (showing the current VFO in kHz.Hz format) takes up about 60% of the next row; an S-meter readout (e.g. “S7” or “S9+12dB”) takes the remaining 40%. The frequency display updates in real time as you tune, whether the tuning originates from this browser, another browser, the Thetis main UI, or any external CAT controller.

## 15.4 Frequency Entry and Tuning Rows

Two rows sit below the frequency display:

- The top row holds the Enter Freq button (which opens a numeric keypad for typed-in tuning) and a Step size selector.

- The bottom row holds the four tuning buttons — Down 5, Down 1, UP 1, UP 5.

See Section 16 for details on tuning.

## 15.5 The Custom Button Grid

Below the tuning row is the custom button grid — the most user-configurable element of the interface. The grid draws from a fixed, hand-picked library of the most commonly-used radio controls: HF/MF band buttons, SSB / FM / AM mode buttons, filter-width presets, the noise-reduction and noise-blanker variants, Mute, RX2, PureSignal, Tune, VOX, VAC1/VAC2, and a few more. On a fresh install every button in this library is on the grid; the configuration page (gear icon, lower right) is where you tailor it — hide the buttons you never use, reorder the ones you do, and recolor them. See Section 18.

## 15.6 The Bandscope and Waterfall

Below the button grid is the bandscope panel — a real-time spectrum trace at the top, a scrolling waterfall below it. Both are fed from the same IQ stream that Thetis is processing. A drag handle at the bottom of the bandscope lets you resize the whole panel; a second drag handle between the bandscope and the waterfall lets you rebalance how much vertical space each gets. See Sections 19 and 20.

## 15.7 The Mic Meter

Just above the bandscope panel is a small horizontal mic-level meter that comes alive whenever you transmit. It shows your microphone level. See Section 21.

## 15.8 The MOX Button

Below the bandscope panel is the large MOX button. Press to transmit. The button color and text change to indicate the current transmit state. See Section 22.

## 15.9 The RX1 / RX2 Selector

Below the MOX button is a two-position selector: RX1 and RX2. This switches the entire interface to control the other receiver. Each receiver maintains its own button grid, so you can have a CW-oriented set on RX1 and a digital-modes set on RX2 (or whatever combination suits you). See Section 23.

## 15.10 The Utility Toolbar

Across the bottom of the page is a small toolbar with five icon buttons, labelled:

- **Scope** toggles the bandscope panel on and off.

- **Sliders** toggles the slider panel for additional controls.

- **Buttons** hides or shows your command-button library, giving the bandscope the full height between the pinned strips when you want it.

- **Refresh** forces an immediate state refresh from Thetis — useful if you suspect a display has gone stale.

- **Config** (gear icon) opens the configuration page for buttons, colors, and Flic hotkey binding. See Section 26.

# 16. Frequency Display and Tuning

## 16.1 Reading the Display

The frequency display shows the current VFO frequency in kHz.Hz format with three decimal digits — for example 14205.010 means 14,205.010 kHz, which is 14.205010 MHz. The display updates as soon as the VFO changes, whether the change was caused by this browser, another browser, Thetis itself, or any other CAT controller (logging program, MIDI controller, etc.).

When the active receiver is RX1, the frequency shown is VFO A. When the active receiver is RX2, the frequency shown is RX2’s tuned frequency. Switching between RX1 and RX2 with the selector at the bottom of the page swaps which VFO drives the display — the cached value for each receiver is shown immediately, so you do not have to wait for the next notification from Thetis.

## 16.2 The Up/Down Buttons

Four buttons sit below the frequency display: Down 5, Down 1, Up 1, Up 5. Each button changes the VFO by one step (Up/Down 1) or five steps (Up/Down 5) of the currently-selected Step size.

Tap once for a single step. “Press and hold” any of the four buttons and the VFO tunes continuously at a brisk repeat rate — useful for skipping rapidly across a band, jumping to a different segment, or finding a band edge without tapping dozens of times. The repeat starts after a short delay so a normal tap still produces a single step. Release the button to stop.

The four buttons render in the fixed hardware-panel style and are not separately colorable.

## 16.3 The Step Selector

Next to the Enter Freq button on the row above the tuning buttons is the Step selector — a dropdown listing the available step sizes:

- 10 Hz — SSB fine tuning.

- 100 Hz — casual band-scanning.

- 500 Hz — CW spacing.

- 1 kHz — jumping across a busy band.

- 100 kHz — jumping across band sub-segments.

- 1 MHz — jumping between bands.

- Snap — a special option that rounds the current VFO frequency to the nearest multiple of the currently-selected step size. Useful for getting back onto exact frequency boundaries after free-tuning. Selecting Snap fires the snap action immediately; the step selector returns to its prior step size afterward.

The step selection is saved per browser, so the step size you used last time comes back the next time you open the page.

## 16.4 Direct Frequency Entry

Press the Enter Freq button to open the numeric keypad overlay. The button’s text changes to “Hide Keypad” while the overlay is open. Type a frequency in kHz (use the decimal point for Hz precision, e.g. 14205.010), then press Enter to commit — the VFO jumps to the typed frequency and the overlay closes automatically. The backspace key on the keypad removes the last digit if you make a typo. To close the overlay without committing a frequency, press the Hide Keypad button.

The keypad is sized for fingertip use on a phone or tablet — large, well-spaced buttons including digits 0-9, the decimal point, backspace, and a wide Enter button.

# 17. The S-Meter

## 17.1 Reading the S-Meter

The S-meter shows the current signal strength on the active receiver. The scale is the standard amateur convention: S9 corresponds to —73 dBm, with each S-unit representing 6 dB. Above S9 the reading shows “S9+nndB” — for example “S9+15dB” for a strong broadcast or local station.

The reading is fed from Thetis’s own S-meter via the TCI rx_channel_sensors push notification — the same number Thetis itself shows on its on-screen meter. No polling, no additional load on Thetis: the radio simply broadcasts each new sample as it computes it, and the browser repaints the display.

## 17.2 Readable Display

The meter uses analog-style ballistics so the reading is easy to read at a glance: voice peaks latch onto the digit and hold long enough to register before falling back during pauses. Strong signals snap up instantly. The display does not flicker between values, even on a noisy SSB voice signal.

During transmit the S-meter shows your microphone level instead of a signal reading — the receiver is muted while you transmit, so a received-signal value would be meaningless. The signal reading returns automatically when you unkey.

# 18. Mode, Band, and Filter — The Custom Button Grid

The custom button grid is the most flexible part of the interface. Unlike Thetis’s fixed control panels, you decide which buttons appear and in what order — so the controls you actually use sit on the page, and the ones you never touch do not.

## 18.1 What Is in the Library

The button library is a fixed, hand-picked set of the controls that are most useful on a remote control surface. It is not a catalog of every CAT command Thetis understands — it is a curated list. The current library includes:

- **Bands.** 160 m, 80 m, 60 m, 40 m, 20 m, 17 m, 15 m, 12 m, 10 m. (HF and 60 m only; the higher VHF and UHF bands are not in the library at this time.)

- **Modes.** LSB, USB, FM, AM. (CW and the digital modes are not currently in the library.)

- **Filter widths.** 5.0 k, 4.4 k, 3.8, 3.3 k, 2.9, 2.7, 2.4.

- **Receive-side toggles.** Two complete sets of noise-reduction and noise-blanker controls (NR2, NB1, NB2, ANF), Mute 1, Mute 2, RX2 on/off.

- **Transmit-related toggles.** Tune, VOX, 2 Tone, PS-A (PureSignal), Monitor.

- **Virtual audio cable toggles.** VAC1, VAC2.

- **Misc.** Lock A, Lock B, VFO Sync, Power.

- **Client-side action buttons.** Mobile (shrink to a compact, MOX-focused screen that still shows your live frequency, signal, and mic level — see Section 24.6), Poor Cnx (enter Poor Connection Mode — see Section 24), and DS (Data Saver — toggles the bandscope between full rate and 5 fps for a ~75% bandwidth reduction; see Section 24.5). These are not CAT commands; they affect only this browser session. DS is hidden by default — check it on the Configuration page if you want it. Its short name is the default — you can rename it to anything you like on the Configuration page.

- **Wattmeter readouts.** Exciter power and SWR (from the radio, via Thetis) and output power and SWR (from an external LP100A wattmeter, if you have one). All four are off by default and are display-only — they show a live reading rather than sending a command. The exciter pair is labelled in lower case (pwr, swr) and the output pair in upper case (PWR, SWR). See Section 9.

If a control you want is not in the library, it is not currently available as a button.

## 18.2 Tailoring the Grid

The first time you open Web Commander, every available button is already on the grid. Most operators find that to be too many — a CW operator does not need the DIG modes visible, an SSB ragchewer does not need the 60-meter and 30-meter bands always on screen. The configuration page is where you trim the grid down to what you actually use.

Open the Configuration page from the gear icon in the utility toolbar (bottom right). The button library section lists every available button. Each row has a checkbox to show or hide that button, color swatches for its on and off states, and a pair of up (▲) and down (▼) arrow buttons for reordering. Uncheck the buttons you don’t use, recolor the ones you do, tap the up/down arrows on any row to move it earlier or later in the grid, click Save, and the grid is rebuilt with your selection.

Toggle buttons (NR, MOX-style states, BIN, etc.) automatically light up when they are active and dim when they are not, mirroring the underlying Thetis state. Their on and off colors are independently configurable so the active state stands out.

## 18.3 Per-RX Button Sets

The button grid is saved per active receiver. When you switch from RX1 to RX2 (Section 23), the grid changes to whatever configuration you saved for RX2. This is useful when the two receivers are used differently — for example a band-busy RX1 with the full set of noise-reduction and band buttons in view, paired with a stripped-down RX2 showing just the few controls you care about there. Each receiver gets the layout that suits it.

# 19. The Bandscope

The bandscope is a real-time spectrum trace centered on the active receiver’s tuned frequency. It shows you what is on the air around you at this moment — signals stand out as colored peaks against the noise floor, the colors tell you how strong each signal is, and a thin white vertical line shows the exact frequency you are tuned to.

## 19.1 Turning the Bandscope On and Off

The leftmost button in the bottom utility toolbar toggles the bandscope panel on and off. It is on by default. The waterfall (below the trace) has its own enable checkbox in the bandscope settings, so the bandscope can be shown without the waterfall, or both together.

## 19.2 The Trace and Its Colors

The color of the trace tells you how strong each signal is in S-meter units. The colors are tuned for normal ham-radio operating: green is noise and background, yellow says “there is something there”, orange is a comfortable copy, red is a strong signal, and bright red is a booming one.

The colors are tied to absolute signal strength, not to screen position, so an S9 signal looks the same on every band — the meter does not interpret a strong 80-meter signal as different from the same strength on 20 meters. See Appendix A for the exact S-unit thresholds at which each color transition happens.

## 19.3 The Center Frequency Marker

A thin white vertical line runs from the top of the bandscope to the bottom at the center, marking the receiver’s tuned frequency. White was chosen so the marker stays visible against the bandscope’s red and bright-red colors when a strong signal sits on top of the tuned frequency.

## 19.4 Gain

The Gain slider in the bandscope settings controls the vertical scaling of the trace. Increasing Gain makes weak signals more visible by stretching the entire trace upward; decreasing Gain compresses everything down. The default 1.5x is a useful starting point that brings ordinary signals up to a readable height while keeping strong ones from disappearing off the top. Gain does not change the absolute color calibration — it just changes how much of the dynamic range is in view.

## 19.5 Zoom

The Zoom slider controls how much of the band you see. At minimum zoom the bandscope shows the widest possible view; at maximum zoom it shows a narrow slice centered on your tuned frequency. Useful for separating close-together signals or for dialing in on the carrier of one specific station.

## 19.6 Auto Ref and Recal

With Auto Ref on, the bandscope automatically adjusts itself so the noise floor sits at a comfortable position on screen, regardless of which band you are on. Switch bands and the display re-tunes itself within a second or two — you do not need to fiddle with controls each time.

The Recal button forces an immediate manual re-tune. Use it if you switch antennas, if a local noise source comes on or goes off, or any other time the band conditions change more sharply than the automatic tracker keeps up with.

## 19.7 Smooth

The Smooth slider is the bandscope’s smoothness control. Higher values give a calm, easy-to-read display that averages out the jitter from one moment to the next; lower values make the display react faster at the cost of a noisier-looking baseline.

- Default 150 ms. A good general-purpose value.

- Raise toward 300—500 ms for steady SSB or digital modes where you want a calm display.

- Lower toward 50—100 ms for CW or fast-changing band conditions where you want the display to react instantly.

Smooth does not affect the calibrated colors — a strong signal still shows its true peak at any Smooth setting. It only changes how the noise floor looks.

## 19.8 During Transmit

The bandscope behaves very differently while you are keyed up, because it is doing a different job. In receive it is a wide-band signal hunter calibrated to S-units; in transmit it is an IMD shoulder viewer calibrated to dBc relative to your own carrier. The two modes share the same canvas but use entirely independent vertical mappings, which is what lets the transmit display do its job properly: tell you at a glance how clean your transmit signal is and whether your predistortion is working.

At key-down, four things happen automatically:

- The bandscope picks up the outgoing carrier in the center of the passband, finds its peak, and pins that peak to a fixed position about 15% from the top of the visible bandscope area. The peak stays there for the duration of the transmission, regardless of your output power, the band you are on, the antenna you are using, or how recently the noise-floor tracker calibrated. The display from peak downward becomes a calibrated dBc ruler — every pixel below the carrier corresponds to a known number of dB below the carrier.

- Auto Zoom (if enabled) snaps the bandscope to maximum zoom so the carrier and its IMD shoulders fill the spectrum width. You can see clean shoulder structure rather than a narrow spike lost in a wide-band view.

- The noise-floor tracker pauses (the spectrum during transmit is not representative of the receive noise floor anyway) and the receive-mode color gradient is set aside (the S-meter dBm thresholds it uses are meaningless on a peak-relative dBc scale). The trace is drawn as a clean line in the trace color so the shoulder shape reads clearly.

- An IMD3 estimate begins sampling. After a brief warm-up period a numeric “Est avg IMD3” readout appears near the TX Ref line and updates once per second, giving you a measurable companion to the visual shoulder you already see. When you unkey, the same display area switches to “Average IMD3 last transmission” — the average across all valid samples from the transmission you just made.

At unkey, everything reverts: the noise-floor tracker resumes, the zoom returns to your manual setting, the receive gradient and S-unit color thresholds come back. There is no manual recovery step.

Seven controls in the bandscope settings panel govern the transmit display:

- **Show IMD3 estimate.** Master switch for the on-screen IMD3 measurement — both the live readout during transmit and the average displayed after you unkey. Untick to suppress those numbers without affecting any of the visual bandscope behavior (the carrier still peak-locks, TX Range and TX Ref still work). Default on.

- **IMD3 text size.** Three steps — Small, Medium, Large — that scale the on-screen IMD3 readout (both the live “Est avg IMD3” during transmit and the “Average IMD3 last transmission” after unkey, including the “Acquiring data...” and “insufficient data” placeholders). Medium is the default and is comfortably readable at arm’s length on a typical monitor; Small preserves the original compact size for operators who prefer it; Large is for reading the value from across the room. The choice persists in your browser.

- **Auto Zoom on TX.** When ticked (the default), the bandscope automatically snaps to the zoom level set by the TX Zoom slider below at key-down so the carrier and its IMD shoulders fill the display, and restores your manual Zoom setting at unkey. Untick if you prefer a fixed zoom across receive and transmit. If you toggle the setting while keyed, the zoom changes immediately — you do not have to unkey and re-key for it to take effect.

- **TX Zoom.** Target zoom percentage Auto Zoom uses when it snaps in at key-down. Higher values zoom tighter on the carrier and immediate shoulders; lower values leave more spectrum visible on either side. The right value depends on your Thetis sample rate — at 192 kHz a high zoom can crowd the signal and shoulders together, while at 384 kHz and above you have more breathing room. The slider is greyed out when Auto Zoom is off. Moving it while keyed applies the new value immediately, so you can find the level you like by eye during a real transmission. Range 0 to 95, default 85.

- **TX Range.** How many dB below the carrier the bandscope shows during transmit, measured in dBc (dB below carrier). The carrier always sits near the top of the visible area; this control sets where the bottom of the visible ruler is. Smaller values (e.g. 40—50 dBc) zoom in on the carrier’s immediate shoulders, making small IMD products look prominent — useful for fine evaluation. Larger values (e.g. 90—100 dBc) zoom out, so the ruler stretches down to where the noise lives and only badly-degraded IMD reaches above the floor. The default of 70 dBc keeps clean PureSignal residuals visible without making them dominate. Range 40—100 dBc.

- **TX Trace.** A separate color slider for the trace line drawn during transmit (and, when Gradient Fill is on, the cosmetic fill beneath the trace). Independent of the RX Trace slider in the Appearance section — earlier releases used the RX trace color for both RX and TX, which forced one color choice across two very different viewing contexts. Pick whatever reads best against the amber TX Ref line and white IMD3 readout. The default is red, deliberately different from the blue RX default so you can see at a glance that the TX trace has its own setting.

- **TX Ref.** An amber dashed reference line drawn horizontally across the transmit display at the dBc level you choose, with a small label in the top-right corner echoing the slider value. The point of the line is to mark the IMD floor you expect your predistortion to hold — PureSignal typically holds shoulder products around -50 to -60 dBc. Set the line to your target and the bandscope becomes a pass/fail tool every time you key up: shoulders staying below the line means predistortion is doing its job; shoulders climbing above the line means it has lost ground (wrong drive level, calibration drifted, band issue, etc.). The checkbox to the left of the slider switches the line and its label off entirely for operators who do not run PureSignal, in which case the dBc ruler still works as a measurement scale but has no marked threshold. Range -90 to -30 dBc, default -55 dBc, on.

Beyond the visual bandscope, two numeric readouts give you a measurable estimate of how much intermodulation distortion your transmit signal is producing:

- **Est avg IMD3 (during transmit).** Appears in white near the TX Ref line a few seconds after you key up. The scan covers the close-in IMD3 region (about 150 Hz to 1.5 kHz from carrier on the unwanted side, selected automatically based on the active mode), samples the loudest bin in that region every FFT frame, and smooths the result with a 2-second rolling median. The displayed value refreshes once per second so it stays readable rather than ticking with every frame. For about the first five seconds of each transmission it reads “Acquiring data...” while the FFT and PureSignal lock up and the rolling window fills with trustworthy samples; after that a dBc value (e.g. “Est avg IMD3: -53 dBc”) takes its place and updates once per second for as long as you stay keyed. The 2-second median means the displayed value is stable enough to read but still reacts within about a second to real changes — PureSignal toggling on or off, a drive adjustment, amp warm-up, etc.

It is worth understanding what this measurement is and is not, because Web Commander is doing something different from a formal two-tone IMD3 test:

- **What this measurement is:** an estimated, time-averaged reading of the IMD3-region shoulder amplitude during your actual voice transmission. Over the course of a full transmission it converges to a stable representative number for whatever combination of radio, amplifier, drive level, and predistortion you have in play.

- **What this measurement is not:** a two-tone IMD3 test. A formal two-tone test uses a controlled, stationary signal and reads specific predicted distortion-product locations — the resulting number is precise and repeatable to a spec sheet. This continuous voice-driven measurement is different by design; it tells you what is actually happening with your voice signal, sample by sample, averaged for stability. A two-tone test on the same radio will give you a different but similar number; the difference is normal and reflects the different methodology, not a fault in either measurement.

- **What it is good for:** comparing settings on your own radio (PureSignal on vs. off, different drive levels, amp on vs. off), tracking changes over time, and giving you a number to discuss that is grounded in your actual operating signal rather than a test-tone scenario. For a spec-grade IMD3 number, run a two-tone test — Thetis can do this directly, and any bench spectrum analyzer will.

- **Average IMD3 last transmission (after unkey).** As soon as you unkey, the same screen area switches to the AVERAGE of every valid sample from the just-completed transmission, e.g. “Average IMD3 last transmission: -56 dBc”. Stays on screen until your next key-down, giving you a single objective number per transmission rather than asking you to mentally average the live ticks. Useful for tracking “what was that one overall?” or comparing one transmission to the next. Very short transmissions (under about seven seconds total — three seconds of warm-up plus at least four seconds of sampling) collect too few samples to be meaningful and show “insufficient data” instead.

**Note:** The redesigned transmit display assumes your transmit signal comes back through the Thetis IQ stream at a usable level. With normal PureSignal feedback wiring (a sample of the antenna port routed back to the radio), the carrier peak finds itself near the top of the FFT every time. If you transmit into a fully-isolated dummy load with no feedback path, the bandscope will still peak-lock onto whatever the strongest in-passband bin happens to be, which may not be your carrier — the visual is most meaningful when the radio is actually seeing its own transmit signal.

## 19.9 Appearance

Additional controls in the bandscope settings panel let you adjust the look without changing the underlying calibration. These all affect the receive-mode display only — during transmit the bandscope uses its own peak-locked rendering described in Section 19.8 and ignores these settings.

- **Palette.** Eight built-in color schemes — Classic, Thermal, Fire, Viridis, Inferno, Plasma, Grayscale, and Phosphor. The default Classic palette is what was described in Section 19.2 with its S-unit thresholds; the others use smooth gradients for a different look. The same palette choice applies to both the bandscope trace and the waterfall.

- **Gradient Fill.** Toggles the color fill below the trace. Off gives a line-only display.

- **Opacity.** How solid the gradient fill is. Lower opacity is more transparent; higher is bolder.

- **Trace Color.** When the gradient fill is off, the receive trace line is drawn in a single color of your choice. (The transmit trace has its own separate color slider in the Transmit section — see Section 19.8.)

## 19.10 Tuning from the Bandscope

You can tune directly from a bandscope, the way you would on a desktop SDR. Each scope tunes its own receiver.

- Scroll to step: roll the mouse wheel over a scope to move that receiver up or down by one current Step per notch. Change the Step selector for coarser or finer movement.

- Click or tap to tune: click a signal on the scope, or tap it on a touch screen, to send that receiver straight to that frequency. The landing point snaps to the current Step, so it is easy to hit cleanly even with a finger.

- The frequency readout updates the instant you tune, rather than waiting for the radio to answer. On a large jump the scope briefly blanks and repaints on the new frequency, so you are never looking at a stale picture.

Tuning from the scope is controlled by “Bandscope Tuning” on the Config page. With it off, the wheel scrolls the page as usual and taps do nothing.

## 19.11 The Tune Lock

Because a scope is a large touch surface, it is easy to tune by accident when you only meant to scroll the page or steady the phone in your hand. A padlock button sits just to the left of the gear on each scope. Tap it and the padlock turns amber: the scopes no longer respond to taps or the wheel, so you can touch and scroll over them freely without changing frequency. Tap it again to unlock. The lock is shared by both scopes — locking one locks both — and it is remembered between sessions.

The tune lock disarms only the scopes’ touch tuning. Your Up/Down buttons, the keypad, and the Step controls keep working normally. This is different from Thetis’s own Lock A/B, which locks the radio itself and stops every means of changing frequency, including the front panel.

## 19.12 Bandscope Controls on the Main Screen

The most-used bandscope controls — Gain, Zoom, Recal, Fill, and Palette — appear as a compact strip beside each scope’s on/off label (“RX BS 1 On” for the upper scope, “RX BS 2 On” for the lower), so you can reach them without opening the settings panel. RX1 and RX2 each have their own. The gear button that opens the rest of the settings sits in that same strip, off the scope itself, so it is not hit by accident while tap-tuning. The remaining settings — waterfall color, dB range, and the advanced and transmit options — live in the gear panel.

This strip is controlled by “Bandscope Controls on Main Screen” on the Config page; turn it off to move those controls back into the gear panel.

# 20. The Waterfall

The waterfall is the scrolling spectrogram below the bandscope trace. Each new horizontal line represents one moment in time, with the spectrum at that moment painted across the line as a row of colored pixels. Older lines scroll downward over time, building a visual history of activity on the band.

## 20.1 Enabling the Waterfall

In the bandscope settings panel, a Waterfall checkbox toggles the waterfall on and off. When on, the bandscope panel is divided between the trace (top) and the waterfall (bottom), with a drag handle between them to set the split. When off, the trace takes the full panel.

## 20.2 Low and High

Low and High set where the waterfall’s color range starts and ends. They track the current noise floor automatically, so once you set them to your taste they keep working as you change bands.

- **Low.** Move down (more negative) to bring weaker activity into view — the noise itself starts showing low colors. Move up (less negative) for a cleaner background where only real signals get colored.

- **High.** Move down for higher contrast — ordinary signals saturate to the top color sooner. Move up for more dynamic range — only the loudest signals reach the top, weaker ones show in between.

Both inputs have — / + buttons next to them so you can step the value with one tap. On a phone or tablet the on-screen keyboard is intentionally suppressed; the buttons are the only way to change the value there.

## 20.3 Fade and Hide

Two sliders that work together to set what gets shown and how brightly:

- **Hide.** The threshold below which pixels stay black. Raise to clean up noise; lower to let faint activity show.

- **Fade.** The threshold above which pixels get the palette’s full color. Raise to require stronger signals before they look bold; lower to let weaker signals brighten quickly.

## 20.4 Brightness

The Bright slider sets overall vividness. Higher for bold colors that are easy to read in daylight; lower for a softer look that is easier on the eyes during long evening sessions.

## 20.5 Palette

The waterfall shares the same palette dropdown as the bandscope. Switching the palette changes both at once.

# 21. The Mic Meter — Transmit Audio Monitoring

The mic meter is a slim horizontal bar just above the bandscope panel that becomes active during transmit. It shows the current microphone level in dB — the same kind of reading that Thetis’s own on-screen mic meter shows.

## 21.1 What It Shows

The bar fills from left to right as your microphone level rises. The bar itself is a single solid color that you choose from the mic-color control on the Config page — pick whatever color works for your eyes or matches the rest of your scheme. A numeric dB readout sits next to the bar showing the precise level.

Below the bar is a scale line that changes color at the 0 dB mark: white to the left of zero (the safe zone, where audio is below the ALC threshold) and red to the right (where Thetis’s ALC starts intervening). The white-to-red transition on the scale is what tells you how close you are to clipping; the bar itself just shows the current level.

## 21.2 The Mic Cal Slider

Thetis’s on-screen mic meter shows a slightly different reading than the value we receive over TCI (the underlying readings differ by about 13 dB on voice). The Mic Cal slider on the Config page lets you add a fixed dB offset so this meter agrees with what you see on Thetis itself. Default 13 dB; adjust if your installation needs a different value. The setting is per-browser.

# 22. The MOX Button and Transmit Control

## 22.1 The Big MOX Button

The MOX button keys the transmitter — press to transmit, press again to return to receive. Where it sits depends on the layout you are viewing. In the standard layout it is a large, finger-friendly button below the bandscope, labeled “MOX”. In the Large-Screen Layout it sits with your button library across the bottom of the console, always visible and blending in with the other buttons when idle. In either layout, while transmitting the button turns red — glowing, in the Large-Screen Layout — and its text changes to “Transmitting”, and the S-meter shows your microphone level since the receiver is muted during transmit.

## 22.2 What the Page Sees and What It Does Not

Web Commander is not connected to the radio’s transmit hardware directly. It only knows about MOX events in two specific cases:

- When you press the MOX button on the page yourself. The page sends the key command to Thetis and updates its own visual state.

- When the page’s Flic keyboard binding fires (see Section 26.3). This is the page listening for a key combination while it has browser focus; it then keys the radio the same way a manual MOX button press would.

- When an external program broadcasts on the MOX bus (UDP port 5011) — see Section 10. The page listens for those broadcasts and lights up its MOX button to match.

That is the complete list. Anything not in this list, the page cannot see. In particular:

- Pressing the hardware PTT button on your microphone is invisible to Web Commander.

- A foot switch wired to the radio is invisible to Web Commander.

- A CW key plugged into the radio is invisible to Web Commander.

- Another CAT program (logging, digital modes, etc.) sending its own transmit command to Thetis is invisible to Web Commander — unless that program also broadcasts on the MOX bus.

Why: the radio’s transmit-state changes for any of those external triggers happen inside Thetis and on the hardware itself. There is no callback that tells Web Commander about them. The MOX bus exists precisely to give cooperating programs a way to coordinate transmit state, but participating in that bus is opt-in — a program has to choose to broadcast on UDP 5011, and most do not.

If you want hardware PTT presses (or any other off-page transmit trigger) to be visible to Web Commander and to other listeners on your network, you need a separate program that observes the radio’s real-time MOX state and broadcasts it onto the MOX bus. The MOX Control Server is one such program (and the one Web Commander was designed alongside), but it is not bundled with Web Commander and Web Commander does not require it to operate. Without that or a similar bridge running, Web Commander simply does not display hardware-PTT transmit events.

## 22.3 Unkey Delay

The configuration page has an Unkey Delay setting (default 0 ms). When non-zero, releasing the MOX button leaves the radio keyed for that many additional milliseconds before sending the un-key command. Useful for short pauses between SSB phrases without re-keying each time, or for letting a final CW dit clear before the radio drops back to receive. Set to 0 for the most immediate unkey behavior.

# 23. RX1 / RX2 Operation

Web Commander supports both Thetis receivers. A two-button selector — RX1 on the left in green, RX2 on the right in grey — chooses which receiver is the current “active” receiver. In the standard layout it sits below the MOX button; in the Large-Screen Layout it is in the control column on the left. The active receiver drives:

- The frequency display (its VFO).

- The S-meter (its signal level).

- The button grid (its per-RX button configuration).

- CAT commands sent from the page (mode, filter, NR, etc.).

Switching receivers is instantaneous; the cached frequency and S-meter for the receiver you’re switching to appear immediately, so you’re not staring at dashes while you wait for the next notification.

**Note:** The bandscope and waterfall are tied to RX1 — they continue to show RX1’s spectrum even when the RX2 selector is active.

# 24. Poor Connection Mode — Operating Over a Bad Network

Web Commander is built to stream a lot of live data to your browser: a frequency update each time the VFO moves, an S-meter sample several times a second, a microphone level reading while transmitting, and a full bandscope frame at up to 30 frames per second. On a fast LAN this is invisible; on a flaky cellular connection, a hotel WiFi, or a long latency-prone VPN, all that traffic can stutter, delay your button presses, or run up your data plan. Web Commander offers two responses to this, of escalating severity: a lighter bandscope-only control called the Data Saver button (Section 24.5), which keeps the full interface live but slashes the bandscope traffic to about a quarter of normal; and full Poor Connection Mode (Sections 24.1—24.4), which suppresses every push stream from the server and shrinks the page to a minimal MOX-only overlay. Try the Data Saver button first — it is enough for most marginal-cellular situations.

## 24.1 What It Does

Tap the Poor Cnx button on the page and Web Commander does three things at once:

- Tells the server to stop sending every push stream — no more frequency updates, S-meter samples, mic-level readings, or bandscope frames cross the network.

- Unsubscribes from the bandscope entirely so even setup traffic stops.

- Replaces the page with a compact full-screen MOX overlay — a banner across the top (blanked frequency and meter readings to remind you they are intentionally not live) and a large MOX button below. Tap MOX to transmit; tap again to return to receive.

The network traffic drops to essentially nothing except the once-per-second heartbeat and the occasional MOX command. The radio still receives and transmits normally; you are simply telling Web Commander to be quiet so the connection doesn’t get overwhelmed.

## 24.2 When To Use It

Poor Connection Mode is most useful in these situations:

- Operating from a cellular hotspot where bandwidth costs money and the live bandscope frames would burn through your data allowance.

- Using the radio remotely over a slow or laggy network where the constant push traffic is making MOX feel slow to respond.

- Walking around the house with your phone where you just want a quick way to key the radio for an over and don’t need to see the bandscope or the meter.

- Anywhere you are listening through VDO.Ninja audio anyway — the audio tells you what is happening, you don’t need the visual streams.

## 24.3 Entering and Exiting

Tap the Poor Cnx button on the page to toggle. The setting is remembered across page reloads — if you leave Poor Cnx on and close the page, the next launch comes back straight into the compact overlay. To exit, tap the small X in the top corner of the overlay; you return to the full interface and all the live streams resume.

## 24.4 The Status Message

When you enter Poor Cnx mode, Web Commander flashes a brief status message: “audio is your TX indicator”. The reminder is deliberate — since the page is no longer showing live readings, your audio path (VDO.Ninja or however else you are hearing the radio) is the only feedback you have. If that audio path is not running, you are operating blind.

## 24.5 The Data Saver Button — A Lighter Alternative

For most marginal-cellular situations the full Poor Connection Mode is more aggressive than you need. The bandscope stream is by far the largest contributor to the data traffic Web Commander sends to your browser — on the order of 180 kbps for the full-rate bandscope versus the entire rest of the interface (frequency, S-meter, mic level, MOX state, command responses) which together add maybe 5 kbps. Reducing the bandscope rate alone captures essentially all the bandwidth savings of Poor Connection Mode while keeping the rest of the interface fully live.

The DS button (available in the button library, hidden by default — check it on the Configuration page if you want to add it to your grid) is a simple two-state toggle: off (full bandscope rate) or on (5 frames per second, about a quarter of the data). The button shows its name (default “DS” — short for Data Saver, kept abbreviated so it fits cleanly on a single button row; you can rename it to anything via the Configuration page) followed by the live bandwidth in kilobits per second, e.g., “DS 184k” when off or “DS 45k” when on. The button color follows the standard convention: white (inactive) when off, blue (active) when on. To turn the bandscope itself entirely off, use the Scope button in the utility toolbar — DS doesn’t duplicate that function.

- **Off (white).** Full bandscope rate. The button shows the configured name followed by the actual kilobits-per-second the server is sending to your browser, measured live in a 5-second rolling window. Expect roughly 150—220 kbps in this mode, depending on band activity and Smooth setting.

- **On (blue).** Bandscope rate dropped to 5 frames per second. Bandwidth typically falls to around 45 kbps — about a 75% reduction from full rate, or roughly one quarter of the data. Crucially, the bandscope still looks smooth and continuous on screen: Web Commander interpolates between received frames in the browser, so a 5-fps data stream renders as a 60-fps smooth display. For normal operating (voice, digital, general band-watching) you cannot tell the difference visually. The only situations where 5 fps becomes noticeable are fast CW work, where a 50—200 ms dot can occasionally fall between two arrived frames, and rapid frequency scrolling, where the bandscope lags behind your tuning more than at full rate. For everything else, the saver-on mode is essentially free.

The button label and color update every second so you can watch the actual measured bandwidth change in real time when you toggle.

**Important:** For metered cellular use, leave Data Saver on. The bandwidth saved is substantial — approximately 60 MB per hour of operation compared to full rate — and the visual cost is essentially zero for normal use. Only turn the saver off if you specifically need the fastest possible bandscope response, for example while doing fast CW work or rapid band scanning.

## 24.6 Mobile Mode vs Poor Connection Mode

The button library includes a Mobile button whose overlay can look much like the Poor Connection overlay — both replace the page with a large MOX button. They are not the same thing, and the difference is what happens to your data.

**Mobile** is a simplified view, not a traffic cut. It collapses the page to a compact, MOX-focused screen for one-handed, eyes-free operation, but the connection stays full: the strip across the top keeps showing your live frequency, signal, and microphone level, and the radio’s streams keep flowing in the background. Use it when you want a clean, thumb-sized transmit screen but still want to see what the radio is doing. Tap the × in the corner to return to the full interface.

**Poor Connection Mode** is a traffic cut. It collapses the layout the same way, but it also tells the server to stop sending the live streams — so the frequency and meter readings in the banner are blanked on purpose, and network use drops to almost nothing (Section 24.1). Use it when the network itself is the problem — a weak or metered connection — and you are willing to operate on your audio alone.

In short: Mobile simplifies the screen while keeping the data live; Poor Connection mode cuts the data to survive a bad link.

# 25. Adding to Your Home Screen — The PWA Experience

Although Web Commander is a browser page, every modern phone and tablet supports adding it to the home screen as a Progressive Web App (PWA). After adding, the page launches with its own icon, runs in full-screen without the browser’s address bar visible, and feels indistinguishable from a native installed app — except there is nothing to install or update from any app store.

## 25.1 iPhone and iPad (Safari)

**Important:** Not recommended on iPhone or iPad. Apple’s WKWebView (the engine behind iOS standalone-PWA mode) aggressively suspends and resumes the JavaScript runtime during phone lifecycle transitions (screen lock, switching apps, low-power mode). Those suspensions can cause Web Commander’s WebSocket connection to appear to drop and reconnect repeatedly, even when the network is fine. The same page running in a regular Safari tab does not have this problem because Safari handles tab lifecycle differently than standalone PWAs. The recommended pattern on iPhone and iPad is to open Web Commander in Safari directly, then bookmark the page for one-tap return. If you want a home-screen icon, use Safari’s bookmark-to-home-screen feature for the bookmark itself rather than the Add-to-Home-Screen PWA flow.

If you want to try the PWA install anyway: open Web Commander in Safari. Tap the Share button at the bottom of the screen (the square-with-up-arrow icon). Scroll the share sheet and tap “Add to Home Screen”. Adjust the name if you wish, then tap Add. An icon for Web Commander appears on your home screen; tap it to launch the app in standalone mode. Be aware of the reconnect-cycle issue above.

**Important:** Safari is the only iOS browser that can install a PWA. Chrome and Firefox on iOS share Safari’s underlying rendering but their “Add to Home Screen” entry creates a bookmark rather than a standalone app.

## 25.2 Android (Chrome, Edge, Firefox, Samsung Internet, Brave)

Open Web Commander in your browser. Tap the three-dot menu in the top right and choose “Install app” or “Add to home screen” (the wording varies by browser). Confirm the install. An icon appears in your app drawer and home screen; tap it to launch in standalone mode.

## 25.3 Why Use the Home-Screen Shortcut

The standalone PWA has several advantages over a regular browser tab:

- No address bar takes up vertical space; the full screen is usable for radio controls.

- Launches with a single tap on its own icon, not via opening a browser and finding a bookmark.

- Survives closing the browser; the next launch is instantaneous.

- Behaves like a separate app for multitasking, screenshotting, and notification handling.

Your saved settings (button grid, bandscope preferences, etc.) are preserved across the transition from browser bookmark to home-screen icon, because they live in the same local-storage area that the underlying browser uses.

# 26. The Configuration Page — Buttons, Colors, and Flic

The gear icon in the bottom utility toolbar opens the configuration page. This is where you set up the button grid and the per-RX appearance details. The page is divided into sections; each section’s settings are saved together when you click Save.

## 26.1 Button Font and Height

Two numeric fields control the size of the buttons in the grid: font size (8—24 pt, default 13) and button height in pixels (24—80, default 36). Larger values give a more touch-friendly grid at the cost of vertical space; smaller values pack more controls into less area. The defaults are tuned for finger use on a phone.

## 26.2 Unkey Delay

As described in Section 22.3, this sets the number of milliseconds the MOX button waits between release and sending the un-key CAT command. Default 0.

## 26.3 Flic Hotkey Binding

If you use a Flic button (a Bluetooth single-button accessory) configured to send a keyboard combination, this section lets you bind that combination to the page’s MOX action. Pick the modifier keys (Ctrl, Alt, Shift) and the key, and the page will treat that combination as a MOX press whenever it has focus. Convenient for hands-free keying when your phone is mounted at the operating position.

## 26.4 The Default Button Look

Command buttons you have not given a color of your own render in the hardware-panel style — brushed-metal grey at rest, and lit blue when active — and their text is automatically kept light or dark so it always stays easy to read against either shade. You can still set any button’s own colors in the button library below (Section 26.6); the panel look simply applies until you do. The Mobile and Poor Cnx buttons open a full-screen overlay rather than toggling a radio state, so their color picker shows a single resting color (they have no separate “active” color).

## 26.5 The Color Picker

Every color swatch on the configuration page — the on and off color swatches for each button in the library below — opens the same color picker when you tap it. The picker is the same on every platform: Windows, Mac, Linux, Android, iPhone, and iPad all see the same controls and the same workflow.

What appears when you tap a swatch:

- A gradient area at the top for picking saturation and brightness by dragging.

- A hue slider beneath the gradient.

- A small color preview that compares the original color (left) with your new choice (right) — at a glance you can see exactly how the change differs from where you started.

- A hex text field where you can type a specific color value like “#ff8800” if you know exactly what you want.

- A row of swatches at the bottom containing your recently picked colors (most recent first), followed by a small curated set of preset colors.

- A Cancel button (gray) and a Save button (green) at the very bottom.

How to use it:

- **Pick visually.** Drag inside the gradient and move the hue slider to find a color you like. The preview updates live so you can see the result before committing.

- **Type a specific hex value.** Click the hex field and type a six-character color code (e.g. “#cc0000”). Useful when you want to match a particular brand color or carry one over from another program.

- **Tap a swatch.** One tap on any swatch in the bottom row immediately loads that color into the picker. Useful for re-using a color you’ve picked before or for jumping to a preset.

- **Save or Cancel.** Save commits the color and closes the popup. Cancel closes the popup and restores the color that was selected when you opened the picker — a safety net if you’ve experimented and don’t like the result.

Two workflow notes worth knowing:

- **Recent colors are remembered across sessions.** Every color you save is added to your personal swatches row. Close the browser, come back tomorrow, open the picker on any button — your recent colors are still there. The list holds the twelve most recently picked colors per browser.

- **Copying a color from one button to another.** Pick a color for the source button and save it. Open the picker for the destination button — the source color is the first entry in your recents row. One tap to apply, then Save. No hex numbers to write down, no trial and error.

- **Reverting after a mistake.** If you save a color and then realize you wanted the previous one back, reopen the picker on the same swatch. The color you had before is in the recents row (likely the second or third entry depending on what you have picked since). Tap it, save, you’re back where you started.

## 26.6 Button Library

The bulk of the configuration page is the button library — a long list of every available CAT-driven button. Each row contains these editable controls:

- A visibility checkbox (uncheck to hide the button from the grid).

- An editable label text field showing the button’s current display name.

- A small circular reset arrow (↺) that restores the button to its default label and color (see Section 26.7).

- Two color swatches: the button’s on (active) color and its off (inactive) color.

- Up (▲) and Down (▼) arrow buttons for changing the button’s position in the grid.

On a fresh install every checkbox is already on, so the grid in the main window shows everything. Use this page to uncheck the buttons you don’t want, recolor the ones you do (using the picker described in Section 26.5), reorder, optionally rename, and click Save.

## 26.7 Renaming a Button

Every button in the library can be given a custom display name. Click into the label text field for any row and type whatever you want, up to ten characters. Common reasons to rename:

- Shorten a long default to fit a narrow phone-screen grid (e.g. “2 Tone” into “2T”).

- Translate a CAT-protocol code into something friendlier (e.g. “PS-A” into “PureSig”).

- Use a label that matches your local convention or another program’s naming scheme.

Whatever you type in the label field becomes the visible caption on the button in the main grid. The underlying CAT command that fires when the button is pressed is unchanged — only the on-screen text differs.

To restore a button to its defaults, click the small reset arrow (↺) on its row. It returns the button to its original label and color. If you have changed only one of the two, that one is restored immediately; if you have changed both the label and the color, it first asks which you would like to reset — Label, Color, or Both. The arrow is greyed out when the button already matches its default, so an active arrow always means “you have changed this button and can undo it.”

Label changes — like every other setting on this page — only take effect when you click Save at the bottom. Cancel discards them.

## 26.8 Reset, Cancel, Save

Three buttons at the bottom: Reset (restores all configuration to factory defaults), Cancel (closes the page without saving), and Save (writes all changes and rebuilds the interface). Changes are not committed until you click Save.

# 27. Client Settings and Resetting

Your remote device keeps its own settings, held by the browser and separate from the server’s settings on the desktop (Section 11).

Everything else — button grid, bandscope and waterfall preferences, color choices, Mic Cal, Flic binding, Up/Down colors, step size — is saved by your browser. Each browser-on-each-device has its own independent set.

## 27.1 Resetting to Defaults

Three reset paths exist, scoped from narrowest to broadest:

- **Reset bandscope/waterfall defaults.** A Reset button on the bandscope settings panel restores all bandscope and waterfall controls to their shipped defaults.

- **Reset all browser configuration.** The Reset button on the configuration page restores everything else stored in this browser (buttons, colors, font sizes, Flic, Up/Down colors) to defaults.

- **Reset All Settings (server side).** The Reset tab in the server status window has a single “Reset All Settings” button. A confirmation dialog lists every category that will be erased: Thetis connection settings, web server port, main window position, the VDO.Ninja callsign and saved device choices, and the KC4VO audio server’s device choices, port, and window position. After confirming, Web Commander closes; relaunch lands in first-run state.

To completely wipe a browser’s state and start over, clear site data for the page from the browser’s settings.

# 28. Client Audio — VDO.Ninja and the KC4VO App

Audio is a separate subsystem from radio control. Whichever transport you use, its server runs on the desktop (Section 8); here we cover choosing a transport and connecting to it from your device. There are two, and you can use whichever suits the device in your hand.

Web Commander ships with two independent audio transports built in. Both carry receive audio from the radio to your remote device and your microphone audio back to the radio; they differ in how they move that audio, and each has its strengths.

**VDO.Ninja** is a browser transport — it runs in any modern browser (iPhone, iPad, Android, or laptop) with nothing to install on the remote side; you simply open a URL. VDO.Ninja is a well-regarded third-party, open-source WebRTC application, not developed by KC4VO. It uses Opus compression, so it needs noticeably less bandwidth, and it runs on devices where a native app is not available.

**The KC4VO Low Latency Server** pairs with KC4VO’s native remote-audio apps — Thetis Audio Commander for iPhone and Android, and a companion app for Windows. It carries uncompressed audio, which can sound richer than a compressed stream (how much you notice depends on your transmit bandwidth), and it connects more directly: it uses no external signaling or relay server, so the path is a consistent point-to-point link.

Either is a sound choice; pick the one that fits your device and your priorities — lowest bandwidth and nothing to install (VDO.Ninja), or uncompressed audio over a direct connection (the KC4VO server with Thetis Audio Commander). Because a purpose-built app now carries audio so well, the VDO.Ninja audio buttons are hidden from the main screen by default; turn them on with the “VDO.Ninja Audio Buttons” checkbox on the Configuration page whenever you want them.

## 28.1 VDO.Ninja in the Browser

The VDO.Ninja transport runs in your browser. It is the right choice when your remote device is an iPhone, iPad, Android phone or tablet, laptop, or any other device with a modern browser. There is nothing to install on the remote side — you simply open a URL.

Connecting your remote device to it:

- On your phone or tablet, open Web Commander in the browser as you normally would for radio control. Make sure the VDO.Ninja audio buttons are showing on the connection bar — they are hidden by default, so turn on “VDO.Ninja Audio Buttons” on the Configuration page if you do not see them. Then, on the connection bar at the top, tap 🔊 (Phone speaker) or 🎧 (Headphones) directly; each icon acts on its own with no intermediate popup.

- The browser opens a new tab on VDO.Ninja with your callsign already in the URL; the page autostarts streaming, asks for mic permission, then audio is flowing both ways. Switch tabs to come back to radio control.

- Bookmark or add-to-home-screen the VDO.Ninja tab on first visit if you want one-tap access without going through Web Commander every time.

Phone speaker mode adds acoustic echo cancellation so the radio audio coming out of the phone’s speaker does not feed back into the mic. Headphones mode omits AEC for cleaner audio.

**Important:** To switch modes (Speaker ↔ Headphones), tap ⏹ first to stop the current audio, then tap the other mode. Direct mode-switching without stopping leaves the audio in a half-set-up state on iOS (mic and/or speaker stops working) because iOS Safari/Chrome don’t cleanly hand off WebRTC between tab loads. The connection bar enforces this — while one mode is active, the other mode’s button is greyed; only ⏹ is available.

**Note:** iOS mic permission. iOS Safari and Chrome re-prompt for microphone permission on every fresh page load of the VDO.Ninja tab by default — each Stop-then-Start cycle counts as a fresh load. To stop the re-prompts, open iOS Settings → Safari → Camera & Microphone (or Websites → Microphone) and set vdo.ninja to “Allow” instead of “Ask”. Android Chrome remembers Allow persistently and does not re-prompt.

**Note:** On an iPhone, while audio is flowing you may see a “Now Playing” widget appear in iOS Control Centre. This is a harmless cosmetic iOS chrome effect — its pause button does nothing because WebRTC does not implement the iOS Media Session API. Audio continues flowing regardless.

## 28.1.1 The Speaker Volume Popup

Tapping the 🎚 button on the connection bar opens a small popup with two things:

- **Speaker volume.** Slider 0—300 (default 100) plus a numeric text box that accepts up to 500. Single value shared across every connected client because it lives on the host’s outgoing audio path, and saved on the Web Commander server so the value survives restarts. Changes take effect LIVE — the audio level shifts continuously as you drag, with no reload or audio interruption.

The Speaker volume slider is primarily useful as a workaround when AEC is on and Android Chrome routes the WebRTC stream into the OS’s voice-call audio stream — the phone’s media volume buttons then stop affecting playback level. Boosting Speaker volume (200—500) sends a louder signal that lands at a usable level regardless of which Android stream the OS chose.

- **Mic-level guidance.** A text panel below the volume slider explains how to adjust your mic level. The mic level isn’t controllable from this popup (Web Commander has no way to reach inside your phone’s VDO.Ninja tab to drive it remotely). Two paths: adjust the “Master Gain” slider on the VDO.Ninja page itself (switch to that tab in your phone browser); or adjust “TX Gain” in your remote profile directly on Thetis. The popup recommends the Thetis path. Both produce the same audible result.

**Note:** Speaker volume is single-source — two operators on two phones share the same value. If one bumps it to 250, the other immediately hears that change too.

**Note:** The mode buttons (🔊 / 🎧 / ⏹) live on the connection bar itself, not inside this popup. The active-mode gold ring and the Stop-first switching behavior described earlier apply to the bar icons directly. The popup is just the speaker volume and the mic-level tip.

## 28.2 The KC4VO App

The second built-in transport is a dedicated audio server that pairs with KC4VO’s native remote-audio apps — Thetis Audio Commander for iPhone and Android, and a companion app for Windows. It is the right choice when you prefer a dedicated app to a browser tab, want uncompressed audio, or want a direct connection with no external relay. Latency is excellent.

**Important:** The KC4VO remote audio applications are not bundled with Web Commander. You must obtain them separately.

You start this audio server on the desktop and read off its address there (Section 8.2). Then, on your device:

- On your remote device, open the KC4VO remote audio app, enter the IP and port, and connect.

Audio flows. As with the VDO.Ninja transport, you only do this setup once; the remote app remembers the IP and port for the next launch, and the audio server remembers its device choices.

# 29. The Large-Screen Layout

Web Commander offers an optional Large-Screen Layout: a full remote-control console styled like a hardware front panel, built for the room available on a computer or a large tablet. Turn it on with “Use Large Screen Layout” on the Config page. It engages whenever the screen is wide enough and steps aside for the phone layout when it is not.

Instead of scrolling between sections, the console shows everything at once:

- Both receivers across the top — RX1 and RX2 frequency and signal readouts side by side.

- A full control column down the left — receiver selector, frequency entry and tuning, and the complete slider bank (audio, pan, AGC, Drive, Mic, TX bandwidth) with live values.

- Both bandscopes in the center — RX1 and RX2 spectra and waterfalls, each independently adjustable, with the microphone meter above them.

- Your button library across the bottom, alongside a large, always-visible MOX button.

With RX2 enabled in Thetis, the console runs both receivers at once — two readouts and two live bandscopes, each with its own settings. With RX2 off, it collapses cleanly to a single receiver, and the RX1 bandscope controls remain available.

## 29.1 The Layout Fits Your Screen Automatically

The console comes in two forms and chooses the right one for the screen, so it is never cut off at the edges or needlessly shrunk:

- On a wide landscape screen — a computer, a 1080p monitor, or a large tablet held sideways — you get the full three-column console. If the window is narrower than the console needs, the whole console scales down to fit rather than running off the edge, and returns to full size when the window is widened again.

- On a tall or narrow screen — a tablet held upright, or a folding phone opened to its inner screen — the console reflows into a single tall column at full size, stacking the bandscopes, controls, slider bank, and button library one above the next. Nothing is shrunk, so text and meters stay sharp, and it switches the moment you rotate the tablet or open the fold.

- On an ordinary phone, you get the phone layout — the familiar scrolling screen.

At a glance:

- A phone held normally — the phone layout.

- A folding phone opened to its inner screen — the tall single-column console.

- A large tablet in landscape — the three-column console.

- A large tablet in portrait — the tall single-column console.

- A computer or 1080p monitor — the three-column console.

- In-between sizes — 8-inch Android tablets and smaller iPads — fall on either side of the line depending on width and how you hold them.

**Note:** scaling on Android phones and tablets is still being refined and may not be perfect yet on every device.

# 30. Data Usage

How much data Web Commander uses depends mostly on choices you control — above all, how many bandscopes you display and whether you turn on the data-saving modes. The approximate figures below, per hour, let you plan against a data allowance.

- Two bandscopes, full detail — about 245 MB per hour.

- One bandscope, full detail — about 125 MB per hour.

- Bandscope in Data Saver — about 19 MB per hour, each.

- No bandscope displayed — about 7 MB per hour.

- Poor Connection Mode — under 1 MB per hour.

The bandscope is by far the largest item, because it carries a live spectrum picture updated many times a second. Data Saver thins that stream sharply, and Poor Connection Mode goes further still, dropping the program to almost no data at all. Everything else — frequency, meters, buttons, and tuning — is small by comparison.

Audio is separate from these figures. If you use it, it runs at a steady rate the whole time it is connected and does not change with your control choices:

- VDO.Ninja WebRTC audio, which works in any browser: about 125 MB per hour.

- The KC4VO Low Latency Server, used with its companion app on Android: about 375 MB per hour.

Because audio is a fixed cost, the way to manage your total data is through the control choices above.

# Part III — Reference

# 31. Compatibility and Interoperability

Because Web Commander talks to Thetis through the standard CAT and TCI servers, it is compatible with everything that already works with Thetis.

## 31.1 CAT-Driven Programs

Logging programs (Log4OM, N1MM+, DXLab, etc.), digital mode programs (WSJT-X, JS8Call, JTDX, FLDIGI), CAT controllers (MIDI knobs via Midi2Cat, hardware tuners with CAT) — all continue to work unchanged. They communicate with Thetis directly and have no awareness of Web Commander. When they change the radio’s state, Web Commander sees the change via TCI push notifications and updates its display in real time.

## 31.2 The Thetis Main UI

The Thetis main UI continues to work normally. You can operate Web Commander from your phone in one room and the Thetis main UI on the same PC in another room; However if you change certain settings on the Thetis UI, Web Commander may not know about the change as not all settings are bidirectional to save the bandwidth that would be required to constantly poll Thetis to check on its current status. However, if you need to guarantee synchronization between Web Commander and Thetis, just press the Refresh button on the control bar at the bottom of the remote screen, and a full refresh cycle will occur and Thetis and Web Commander will be brought back into sync.

## 31.3 PureSignal, Diversity, MultiRX

All Thetis DSP and signal-processing features — PureSignal, diversity, multi-RX, noise reduction, advanced filtering — continue to operate exactly as they always have. Web Commander can drive any of them via its custom button grid; the buttons appear, click them, the underlying CAT command runs and Thetis takes the action.

# 32. Troubleshooting

## 32.1 Browser shows “Connecting...” and never connects

The most common cause is that the server is not running or is on a different IP address than the URL you’re using. Check the status window on the Thetis PC to confirm the server is alive and to see the exact URL it expects.

Second most common: a Windows firewall is blocking port 8765. Either add an exception for the Web Commander executable, or temporarily disable the firewall to confirm that’s the cause.

## 32.2 Frequency display says ----.--- and never updates

The server is connected to the browser but isn’t getting frequency notifications from Thetis. Check that Thetis’s TCI server is enabled in Setup → TCI Server, and that the configuration page in the server’s status window points at the correct TCI host and port. Restart the server after changing TCI settings.

## 32.3 Bandscope is empty / waterfall is grey

Same root cause as the frequency display issue — TCI is not connected. The bandscope is fed entirely from TCI’s IQ stream; if TCI is down, there’s nothing to display.

## 32.4 Bandscope colors look wrong on one band

The color calibration takes a moment to settle after a band change. Wait a few seconds after QSY, or press Recal on the bandscope settings panel to force an immediate recalibration. If colors remain off after that, the band’s noise floor may be unusually busy — the calibration uses the live S-meter to anchor itself, and a busy band makes that anchor less reliable.

## 32.5 Adjusting Low / High waterfall on mobile without a minus sign

The Low and High waterfall inputs have dedicated +/— step buttons next to each field that work on any platform. The on-screen keyboard is intentionally suppressed on mobile so the buttons are the only path; desktop physical keyboards work normally.

## 32.6 Page on phone home screen launches but won’t connect after sleeping

iOS in particular pauses background activity aggressively in standalone PWA mode (the mode used when you Add to Home Screen on iPhone). When you wake the page after the device has slept, the connection may have been silently dropped, and the page may go through a noisy series of failed reconnect attempts before settling down. The recommended fix is to stop using the home-screen shortcut on iPhone and use Safari directly instead — the same page running in a regular Safari tab does not have this problem because Safari handles tab lifecycle differently than standalone PWAs. See Section 25.1.

## 32.7 MOX state on the page does not match the radio

Tap Refresh in the utility toolbar. The page re-queries all state from Thetis and resyncs the displays.

# Appendix A — Bandscope Color Thresholds

The default bandscope color gradient is anchored to fixed absolute S-meter dBm values. The thresholds are:

- Green — below S7 (—85 dBm).

- Green-to-Yellow transition at S7 (—85 dBm).

- Yellow-to-Orange transition at S9+5 (—68 dBm).

- Orange-to-Red transition at S9+10 (—63 dBm).

- Red-to-Bright Red transition at S9+15 (—58 dBm).

- Solid Bright Red above S9+15.

Reading the chart: green is noise and background; yellow says “there is something there”; orange is comfortably readable; red is a strong signal; bright red is a booming local or commercial broadcast.

# Appendix B — Glossary

**Auto Ref** — The bandscope’s automatic noise-floor tracking feature. When enabled, the display baseline follows the band’s noise floor automatically; manual Recal forces an immediate re-tracking.

**Auto Zoom on TX** — Bandscope option that snaps the zoom to the TX Zoom slider value at key-down so your transmit carrier and IMD shoulders fill the spectrum width, and restores your manual zoom at unkey. Default on. See Section 19.8.

**Average IMD3 last transmission** — White text readout that appears in the top-right area of the bandscope as soon as you unkey, showing the average IMD3 estimate across the just-completed transmission. Stays until your next key-down. For very short transmissions (under ~4 seconds) displays "insufficient data" instead. See Section 19.8.

**Bandscope** — The real-time spectrum trace at the top of the bandscope panel.

**Button library** — The list of every CAT-driven button that can be added to the custom button grid via the configuration page.

**CAT** — Computer-Aided Transceiver protocol. The textual command interface Thetis uses for radio control; standard for all SDR programs and most hardware radios.

**Web Commander** — Short form of Thetis Web Commander, the program described in this guide.

**dBc** — Decibels relative to the carrier — a level expressed as how many dB it sits below (or above) a reference carrier signal. The transmit bandscope’s vertical scale is in dBc, so the carrier itself is 0 dBc and IMD shoulders sit at, e.g., -55 dBc.

**Est avg IMD3** — Numeric estimate of IMD3-region shoulder amplitude shown in the top-right area of the bandscope during transmit, in dBc relative to the carrier. Continuous voice-driven measurement (not a two-tone test): samples the loudest bin in the close-in IMD3 region (~150 Hz to 1.5 kHz from carrier on the unwanted side) every FFT frame, smooths with a 2-second rolling median, refreshes once per second. Shows "Acquiring data..." for about five seconds after key-down (FFT/PureSignal settle ~3 seconds, rolling window fills ~2 seconds). Useful for relative comparisons (PS on/off, drive levels, amp on/off). A two-tone test gives a different but similar spec-grade number. See Section 19.8.

**Flic** — A Bluetooth single-button accessory. Web Commander can bind a Flic-issued key combination to its MOX action.

**IMD** — Intermodulation Distortion. The unwanted shoulder products that appear on either side of an SSB transmit signal due to non-linearity in the transmit chain. Reducing IMD is the purpose of PureSignal adaptive predistortion.

**IQ stream** — The raw spectrum data Thetis sends to Web Commander over TCI so the bandscope can be drawn. You do not need to know anything about it; it is mentioned here only so the term is not a mystery if you see it elsewhere.

**LP100A** — A TelePost external digital wattmeter that measures forward power and SWR in the feedline. Web Commander can read one connected to the Thetis PC by USB and show its output power and SWR on the page. See Section 9.

**Mic Cal** — A per-browser slider, on the Config page, that adds a fixed dB offset to the mic-meter reading so Web Commander’s display agrees with what you see on Thetis’s own on-screen mic meter.

**Poor Cnx** — A toggle that silences every push stream from the server and replaces the page with a compact MOX-only overlay. Use it when your network is slow or unreliable, or when you want to minimize data usage on a metered connection. See Section 24.

**MOX** — Manual Operate eXternal — the CAT command that keys the transmitter. Also the name of the big transmit button on the page.

**MOX bus** — A shared notification channel used by several programs in the author’s ecosystem to coordinate transmit state. Web Commander listens and broadcasts.

**PureSignal** — Thetis’s adaptive predistortion feature. It samples the transmit signal at the antenna port and continuously corrects the drive so the resulting transmit signal stays clean (low IMD). The transmit bandscope and TX Ref line are designed around verifying that PureSignal is doing its job.

**PWA** — Progressive Web App. The standard mechanism that lets a browser-based page be installed to a phone’s home screen and launched as a standalone full-screen app.

**Recal** — The manual “recalibrate” button on the bandscope settings panel. Forces immediate noise-floor recalibration.

**Show IMD3 estimate** — Bandscope checkbox that master-enables the on-screen IMD3 measurement (both the live “Est avg IMD3” during transmit and the “Average IMD3 last transmission” after unkey). Untick to suppress those numbers without affecting any of the visual bandscope behavior. Default on. See Section 19.8.

**Smooth** — The bandscope’s temporal averaging time constant. Larger values give a calmer display; smaller values give a more responsive one.

**Step** — The amount the VFO changes when one of the Up/Down buttons is pressed once. Selected from the Step dropdown next to the buttons.

**TCI** — Transceiver Control Interface — the channel Thetis uses to share live information (spectrum data, S-meter readings, mic level, VFO, MOX state) with external programs like Web Commander.

**Thetis** — The SDR application that runs the radio. Web Commander is a remote front end for Thetis.

**TX Range** — Bandscope control that sets, in dBc, how far below the transmit carrier the visible display extends. Default 70 dBc; range 40—100 dBc. See Section 19.8.

**TX Zoom** — Bandscope slider that sets the target zoom percentage Auto Zoom snaps to at key-down. Higher values zoom tighter on the carrier and shoulders; lower values leave more spectrum visible. Default 85; range 0—95. Greyed out when Auto Zoom is off. See Section 19.8.

**TX Ref** — Bandscope control that places a horizontal amber reference line at a chosen dBc level during transmit, with a label echoing the value. Used to mark the IMD floor you expect from predistortion. Has its own on/off checkbox. Default -55 dBc, on. See Section 19.8.

**VDO.Ninja** — A free browser-based audio service used for the audio half of remote operating. Web Commander does not carry audio; VDO.Ninja handles it.

**VFO** — Variable-Frequency Oscillator. The radio’s tuned frequency.

**Waterfall** — The scrolling spectrogram below the bandscope trace. Each horizontal line is one moment in time, with the spectrum painted across it.

# Index

A

Access code, 7

custom code, 7

generating a new code, 7

guest code, 8

session cookies, 8

Acoustic echo cancellation. See Audio

AEC. See Audio

ALC, 27

Audio, 8

AirBrake Reduction, 10

callsign, 9

KC4VO Low Latency Server, 10

Opus compression, 35

speaker volume, 36

Thetis Audio Commander app, 37

VDO.Ninja bridge, 9

VDO.Ninja setup wizard, 9

WebRTC, 35

Auto Ref. See Bandscope

B

Bandscope, 21, 40

appearance (Gradient Fill, Opacity), 25

auto reference and recal, 22

Auto Zoom on TX, 40

carrier peak-lock, 22

colors, 21

controls on main screen, 26

gain, 22

palettes, 25

smoothing, 22, 42

transmit display, 22

tune lock, 26

tuning from, 25

TX Range, 42

TX Ref, 42

TX Zoom, 42

zoom, 22

Button library. See Buttons

Buttons, 20

band buttons, 20

colors and color picker, 33

copying colors between buttons, 33

filter-width buttons, 20

font size and height, 32

hex color entry, 33

library, 20

mode buttons, 20

per-receiver sets, 21

recent colors, 33

renaming, 34

reset to default, 34

transmit toggles, 20

C

CAT (Computer-Aided Transceiver), 3, 41

Client settings (per-browser), 15

Color picker. See Buttons, colors and color picker

COM port. See Wattmeter (LP100A)

Compatibility, 38

logging and digital-mode programs, 39

Configuration page, 32

Connection

automatic reconnection, 15

connecting, 15

heartbeat, 15

mobile operation, 16

multiple browsers, 5

D

Data Saver, 30

live bandwidth readout, 30

Data usage, 38

dBc (decibels relative to carrier), 22, 41

Diversity, 39

E

Echo cancellation. See Audio

F

Firewall (port 8765), 39

Flic button, 32, 41

hotkey binding, 32

Forward power. See Wattmeter (LP100A)

Frequency

direct entry, 19

display, 18

Snap, 19

step size, 19

up/down tuning, 19

G

Guest code, 8

expiration, 8

four-hour limit, 8

revoking, 8

H

Home screen (PWA), 31

Android, 32

iPhone (not recommended), 31

I

IMD (intermodulation distortion), 41

IMD3 estimate, 24

average after unkey, 40

live readout (Est avg IMD3), 41

Show IMD3 estimate toggle, 42

Interface, 16

connection bar, 16

power/connection badge, 17

title bar, 17

Intermodulation distortion. See IMD (intermodulation distortion)

IQ stream, 41

L

Large-Screen Layout, 37

fits the screen automatically, 37

Lock A / Lock B, 20

LP100A. See Wattmeter (LP100A)

M

Manual connection. See Wattmeter (LP100A)

Mic Cal. See Mic meter

Mic meter, 27

calibration, 27, 41

Mobile mode, 31

MOX. See Transmit

MOX Control Server, 13

PTT hotkeys, 13

starting from Web Commander, 13

Multi-RX. See MultiRX (multi-receiver operation)

MultiRX (multi-receiver operation), 3

N

Network

dynamic DNS, 6

HTTP (not HTTPS), 6

LAN, 6

remote access, 6

static IP / DHCP, 6

VPN, 6

Noise blanker. See Noise reduction (NR, NB, ANF)

Noise reduction (NR, NB, ANF), 20

O

Output power. See Wattmeter (LP100A)

P

Poor Connection Mode, 29, 41

Predistortion. See PureSignal

Progressive Web App. See Home screen (PWA)

PTT. See Transmit

PureSignal, 24, 41

PWA. See Home screen (PWA)

R

Recal. See Bandscope

Receivers (RX1 / RX2), 28

Reconnection. See Connection

Reset (settings), 34

Reset All Settings (server), 35

S

Server

installing and starting, 5

reducing radio-PC load, 14

status window tabs, 5

Thetis connection settings, 5

S-meter, 19

ballistics, 20

during transmit, 20

scale (S-units, dBm), 19

Smooth. See Bandscope

Snap. See Frequency

Spectrum display. See Bandscope

Step. See Frequency

SWR. See Wattmeter (LP100A)

T

TCI (Transceiver Control Interface), 3, 42

Thetis, 42

Transmit

hardware PTT not seen, 28

MOX bus, 13, 41

MOX button, 27

unkey delay, 28

Troubleshooting, 39

Tune. See Transmit

Two-tone IMD3 test, 24

TX Range. See Bandscope

TX Ref. See Bandscope

TX Zoom. See Bandscope

U

Utility toolbar, 18

Refresh, 18

Scope, Sliders, Buttons, 18

V

VAC. See Virtual audio cable (VAC)

VDO.Ninja. See Audio

VFO. See Frequency

Virtual audio cable (VAC), 8

DirectSound, 8

VAC1 and VAC2 buttons, 20

W

Waterfall, 26, 42

brightness, 27

Fade and Hide, 27

Low and High, 26

Wattmeter (LP100A), 11

automatic detection, 11

COM port (manual), 12

desktop readout window, 11

exciter vs output, 12

manual connection, 12

multiple meters, 12

port settings (115200 8N1), 12

readout buttons, 11

*— End of user guide —*

---

# Part IV — Help Reference (exact values, complete lists, and troubleshooting)

This section is written for the AI help assistant. It gives exact defaults, full option lists, and a troubleshooting guide taken directly from the program itself. **Where any detail here differs from the descriptions earlier in this guide, treat this section as authoritative** — it reflects the current program behavior precisely. Use it to give specific answers ("the default is X," "the range is Y to Z," "that control is not available") and to diagnose "why isn't this working" questions.

## R1. Ports and network

The server listens on **all network interfaces (0.0.0.0)**, so it is reachable from the LAN — and from the internet if the router forwards the port. The **access code is the only thing gating access** (see R2).

| Purpose | Port | Notes |
|---|---|---|
| Browser (web page + live data) | **8765** | Configurable as "Server Port." This is the only port a user puts in a browser URL. |
| Thetis CAT (radio control) | **13013** | Configurable. Thetis must have its CAT server enabled. |
| Thetis TCI (bandscope, S-meter, power) | **50001** | Configurable. Thetis must have its TCI server enabled. |
| MOX bus — broadcast | **5011 (UDP)** | How transmit state is shared with other programs. |
| MOX bus — query/toggle | **5010 (UDP)** | Used by the optional MOX Control Server and remote PTT apps. |
| KC4VO native audio server | **5002 (UDP)** | Default; changeable in the audio server window (needs a restart to take effect). |

The displayed connect URL is `http://<this-PC's-LAN-IP>:8765`. For LAN use, give the Thetis PC a static IP or DHCP reservation so the URL doesn't change. For remote use, forward the server port on the router (optionally with a dynamic-DNS name) or use a mesh VPN like Tailscale.

## R2. Access code and login

The browser page is password-protected.

- On **first run**, the server auto-generates a **12-character access code** (mixed-case letters and digits, with easily-confused characters removed). It is shown on the server's **Security** tab.
- Opening the page shows a **login screen asking for the access code** before the control interface loads. One code is shared by all devices; many devices can use it at the same time.
- **Sessions live in the server's memory only.** Restarting the server logs everyone out and they must re-enter the code — this is normal, not a fault.
- **Wrong code** → the login page shows "Wrong code." There is no lockout or rate limiting.
- The primary code is managed on the **Security** tab: show/hide it, set a custom code (minimum 6 characters), or generate a new one. Changing it affects new logins only; already-connected devices stay connected until the server restarts.
- A separate optional **guest code** (Guest tab) has a hard expiration — presets 15 / 30 / 60 minutes, custom up to 240 minutes. It is empty by default. Revoking it immediately disconnects guest devices.

## R3. Complete button library (and what is deliberately NOT included)

The grid is built from a fixed library. If a control is not in the list below, it cannot be added as a button.

- **Bands:** 160 M, 80 M, 60 M, 40 M, 20 M, 17 M, 15 M, 12 M, 10 M. **Not included as band buttons:** 30 M, 6 M, and any VHF/UHF band. (Typed frequency entry and scope-tuning do accept 6 m and the other ham bands; there is simply no 6 M *button*.)
- **Modes:** LSB, USB, FM, AM. **Not included:** CW, the digital modes (DIGU/DIGL), SAM, DRM, SPEC. A CW or FT8 operator sets the mode at the radio or in their digital software; Web Commander does not have those mode buttons.
- **Filter widths:** 5.0k, 4.4k, 3.8, 3.3k, 2.9, 2.7, 2.4 (kHz; the labels are abbreviated).
- **Receive toggles:** two full sets of noise tools — NR2-1, NB1-1, NB2-1, ANF-1 (first set) and NR2-2, NB1-2, NB2-2, ANF-2 (second set) — plus Mute 1, Mute 2, RX2 (on/off).
- **Transmit toggles:** Tune, VOX, 2 Tone, PS-A (PureSignal), Monitor.
- **Audio routing toggles:** VAC1, VAC2.
- **Misc toggles:** Lock A, Lock B, VFO Sync, Power.
- **Client-side action buttons** (these affect only this browser, not the radio): Mobile (shrink to the MOX-only overlay), Poor Cnx (Poor Connection Mode), DS (data-saver — cycles the bandscope frame rate between 30 and 5 fps), and four wattmeter readout buttons.

**Wattmeter readout buttons** are **hidden by default** — the user adds them from the Configuration page. There are four, told apart by letter case: lowercase **pwr / swr** show the *exciter* (the radio's own output, before any amplifier); uppercase **PWR / SWR** show the *LP100A* reading (true output after the amplifier). The uppercase pair shows "n/a" when no LP100A is connected.

The fixed tuning buttons (Down 5, Down 1, UP 1, UP 5), the RX1/RX2 selector, and the toolbar (Scope, Sliders, Buttons, Refresh, Config) are always present and not part of the configurable library.

## R4. Tuning, step sizes, and the bandscope as a tuning surface

- **Step sizes:** 10 Hz, 100 Hz, 500 Hz, **1 kHz (default)**, 100 kHz, 1 MHz, and **Snap**. Snap is an action, not a step: it rounds the current frequency to the nearest multiple of the selected step (e.g. with Step = 1 kHz it snaps to the nearest whole kHz), then returns the selector to the previous step.
- **Up/Down buttons:** one tap = one step (or five for the "5" buttons); press and hold to tune continuously.
- **Tuning on the bandscope/waterfall is ON by default.** Mouse wheel = ±1 of the current step per notch (wheel up tunes up). Click or tap = tune to that point, snapped to the current step grid. Both stay inside the ham band edges. There is a per-device padlock that disables tap/wheel tuning while leaving the buttons and keypad working — this is separate from Thetis's Lock A/Lock B.
- **Direct entry:** the keypad enters frequency in **kHz.Hz** format, e.g. `14205.010` = 14,205.010 kHz = 14.205010 MHz. Out-of-band entries are rejected with "Invalid frequency."

## R5. Bandscope, waterfall, transmit display, S-meter — exact defaults and ranges

**Receive bandscope controls** (each receiver has its own independent set):

| Control | Default | Range |
|---|---|---|
| Gain | **1.0×** | 0.5–4.0, step 0.1 |
| Zoom | **80** | 0–95 |
| Smooth | **150 ms** | 30–500 |
| Auto Ref | **On** | on/off (off pins the floor and the display can look wrong) |
| Opacity (gradient fill) | **85%** | 10–100 |
| Gradient Fill | **On** | on/off |
| Trace color (hue) | 200 (blue) | 0–360 |
| Palette | **Classic** | Classic, Thermal, Fire, Viridis, Inferno, Plasma, Grayscale, Phosphor |

FFT size and detector are fixed internally and are **not** user-adjustable controls in this interface.

**Color meaning (Classic palette only):** colors are tied to absolute signal strength. Thresholds: green below about **−85 dBm** (~S7), green→yellow at −85 dBm, yellow→orange at **−68 dBm** (S9+5), orange→red at **−63 dBm** (S9+10), red→bright red at **−58 dBm** (S9+15), solid bright red above that. The other palettes use smooth gradients without these absolute bands. The color calibration re-derives itself live and takes about **2–4 seconds to settle** after a band change or after unkeying — colors can look off briefly during that window, and on a very busy band the anchor is less reliable (press Recal to force it).

**Waterfall controls:** Low **−130 dB** (range −180 to −20), High **−80 dB** (range −180 to +20), Hide **30** (0–255), Fade **100** (0–255), Bright **100% / 1.00×** (50–200). The waterfall shares the bandscope's palette.

**Transmit display:** at key-down the carrier peak locks to a fixed spot near the top and the display becomes a dB-below-carrier (dBc) ruler.

| TX control | Default | Range |
|---|---|---|
| Show IMD3 estimate | On | on/off (master switch for the IMD3 numbers) |
| Auto Zoom on TX | On | on/off |
| TX Zoom | **85** | 0–95 |
| TX Range (dB shown below carrier) | **70 dBc** | 40–100 |
| TX Ref line | **−55 dBc** | −90 to −30 |
| TX Ref line on/off | On | on/off |

**IMD3 estimate timing:** it ignores the first **3 seconds** after key-down (warm-up), then samples the close-in shoulder region (about **150 Hz to 1.5 kHz** from the carrier), shows a ~2-second median that refreshes once per second, and after unkey shows the transmission's average. Transmissions shorter than about **4 seconds of usable data** show "insufficient data." This is a voice-driven estimate for comparing your own settings, **not** a formal two-tone IMD3 measurement.

**S-meter:** standard scale, S9 = −73 dBm, 6 dB per S-unit, above S9 shown as "S9+N." It is fed by Thetis's own meter pushed over TCI (not polled), and is blanked during transmit (the receiver is muted while you key).

**Mic meter (during transmit):** scale runs −20 to +12 dB; the scale turns **red above 0 dB** (where Thetis's ALC starts acting) and is white at/below 0. The **Mic Cal** offset defaults to **+13 dB** (range 0–25) to make this reading agree with Thetis's own peak-reading mic meter.

## R6. Audio — two independent paths, both optional, both off by default

Web Commander carries no audio by itself. It offers two separate audio transports, each on its own tab in the server window. A user can run neither, either, or both.

- **VDO.Ninja audio bridge** — browser-based audio. The Thetis PC runs a hidden browser instance that relays audio through vdo.ninja; the user listens in a second browser tab on their device. One-time setup: enter an **ID prefix** (callsign) and choose which virtual audio cable is receive and which is transmit. The user-facing host volume control runs **0–500% (default 100% = unity)**. There are two client links — one for headphones, one for no headphones (the no-headphones link adds echo cancellation so the phone speaker doesn't get retransmitted).
- **KC4VO native low-latency audio server** — a separate UDP audio server (default port **5002**) for the native "Thetis Audio Commander" app on Android/Windows/iOS. It includes the **AirBrake mic gate** (see below). Buffer defaults to 50 ms (client can request 10–500 ms).

Both are launched from their tab, with an optional "enable on Web Commander start" checkbox each (both default off). Starting/stopping from the button is one-shot and does not change the start-up setting.

**AirBrake mic gate (native audio server only):** when operating with a phone's speaker and mic, keying transmit lets a tail of receive audio bleed from the speaker into the mic and onto the air as a noise burst. The AirBrake gate mutes the phone mic for a hold time right after keying to swallow that tail. Slider label **"AirBrake Reduction,"** range **0–1000 ms in 50 ms steps, default 650 ms, 0 = off**. It arms on the keying event itself (off the MOX bus). Headphones eliminate the bleed entirely and make the gate unnecessary.

## R7. LP100A wattmeter — exact behavior

- **Serial settings: 115200 baud, 8 data bits, no parity, 1 stop bit (115200, N, 8, 1).**
- **USB meters auto-detect.** The program identifies the LP100A by its FTDI USB-serial chip and remembers it by serial number, so it keeps working even when Windows assigns a different COM number. No COM port to choose.
- **Native/non-USB serial ports need the Manual Connection path** (auto-detect only finds USB-FTDI cables). In the LP100A Setup window, enter the COM port (a bare number like `7` becomes `COM7`). **The user must also make sure Windows has that port set to 115200, 8, None, 1 in Device Manager** — a native port left at the Windows default of 9600 will connect but the readings won't decode, and the meter shows as "not found." (This is the single most common native-serial problem.)
- **"Not found" always means the meter didn't answer in LP100A format** — wrong port, wrong baud, wrong cable, the meter switched off, or another program holding the port. There is no "connected but garbled" state; a port that doesn't decode is treated as absent.
- A pinned meter that gets unplugged is reported as not found rather than silently switching to a different meter. If several LP100As are attached, the LP100A Setup window lets the user pick which one feeds the readouts.
- The desktop "LP100A Window" shows power and SWR in large type on the radio PC's own screen; it reads dashes until you transmit.

## R8. MOX / transmit indication — what lights the button and what doesn't

The on-screen MOX button (and the red wattmeter readouts) light up **only for transmit events that are broadcast on the MOX bus (UDP 5011).** Those are:

- Pressing MOX in the browser (or a bound Flic button).
- The optional MOX Control Server's own button or its Scroll Lock / Ctrl+F12 hotkeys.
- A remote PTT app sending a toggle.

**Transmit started at the radio is invisible to the MOX button:** a hardware mic PTT, a foot switch, a CW key, VOX, or Tune/2-Tone keyed at the radio will **not** turn the button red. This is by design — the MOX bus is event-driven, and nothing continuously polls the radio's transmit state, so radio-side keying never produces a broadcast. (The lowercase **exciter** wattmeter readouts *do* react to any transmit, because those come from Thetis over TCI — so power can show while the MOX button stays idle.)

To make hardware PTT visible to the browser, run a program that keys through the MOX bus. The **MOX Control Server** is the companion for this: it's optional, not required (browser PTT works without it), and it adds global Scroll Lock / Ctrl+F12 PTT hotkeys plus remote toggling. But even it only reflects keying that goes *through* it — a foot switch wired straight to the radio still won't show.

**Safety on a lost connection:** if the connection drops while keyed, Web Commander does **not** auto-unkey on an ordinary network blip — that's deliberate, to avoid audible unkey/rekey glitches on flaky cellular. **Thetis's own MOX timeout is what drops the carrier** if the operator vanishes, so that timeout should be set. Web Commander does unkey when the operator deliberately shuts the server down.

## R9. The large-screen layout

- It is **on by default** and turns on automatically at a visible window width of about **800 pixels or more** (a three-column arrangement appears at about **1100 px or more** in landscape; narrower than that is a single column). A user can opt back to the phone layout in the Configuration page. *(Ignore any wording that says "desktop only / ≥1200px" — the actual thresholds are ~800 and ~1100.)*
- It shows both receivers' frequency and S-meter at the top, the bandscope in the center, the MOX button on the right, and the button grid in a band below the scope; both RX scopes can be shown at once.
- In the three-column arrangement the Sliders toolbar button is hidden (the slider panel is always shown there), so "the Sliders button does nothing / is missing" on a wide screen is expected.

## R10. Connecting and staying connected

- **Auto-reconnect is always on.** The page reconnects on its own after any drop, roughly every 2 seconds, and never gives up. (Any older "Auto-Reconnect checkbox" described elsewhere no longer applies — there's nothing to turn off.)
- A heartbeat runs about once a second. A brief stall (~3 s) turns the status dot amber but stays connected; a real dead connection (~12 s) forces a reconnect. Transmit is not dropped during that window.
- On a phone, returning to the page after it was in the background triggers a quick check and fast reconnect if the link died while away.
- **iOS:** use **Safari directly**; do not add the page to the home screen as a standalone app. iOS suspends home-screen web apps aggressively, which causes repeated false disconnects. (This supersedes any earlier "Add to Home Screen on iPhone" suggestion — on iOS, a normal Safari tab is the recommended way.)

## R11. Troubleshooting matrix (symptom → cause → fix)

- **Page won't load / browser hangs on "Connecting…":** the server isn't running, the URL's IP or port is wrong, or a firewall is blocking port 8765 on the Thetis PC. Confirm the server's status window shows the URL it expects; add a firewall exception for the server. If you reach a login page but it rejects you, the access code is wrong (find it on the server's Security tab).
- **Frequency shows `-----.---` and never updates:** the TCI connection to Thetis is down. Enable Thetis's TCI server and check the TCI host/port on the Server tab; restart the server after changing TCI settings.
- **Bandscope is empty or grey:** same cause as above — the bandscope is fed entirely by TCI; if TCI is down there's nothing to draw. A brief blank is also normal right after a big zoom or band change while the display settles.
- **Bandscope colors look wrong after changing bands:** the color calibration is still settling (about 2–4 seconds); press Recal to force it. A very busy band makes the calibration less reliable.
- **Wattmeter shows "n/a" / not found:** the meter didn't answer. For a USB meter, check the cable and that it's powered. For a native serial port, set the port to **115200, 8, None, 1** in Windows Device Manager and confirm you used Manual Connection. A meter you pinned that's now unplugged also reports not found.
- **The MOX button won't light when I key with my mic / foot switch / CW key:** that's by design — radio-side keying isn't broadcast on the MOX bus. Use the browser MOX button, a bound Flic, or the MOX Control Server's hotkeys if you want the button to follow your keying. The exciter power readout will still react.
- **No audio:** identify which transport you set up. VDO.Ninja: make sure you completed the one-time setup (ID prefix and which virtual cable is RX/TX) — without it, audio goes to the wrong devices and never reaches the radio; also confirm Chrome or Edge is installed on the Thetis PC. Native KC4VO server: make sure it's started and the app is pointed at this PC's IP and the right port (default 5002).
- **TX noise burst on key-up over a phone speaker:** raise the AirBrake gate (native audio server) toward 650 ms, or use headphones (which removes the problem entirely).
- **Audio is choppy (native server):** increase the buffer (10–500 ms). **Audio is delayed:** a fresh connection settles to minimum latency after a few seconds of warm-up.
- **Transmit stayed up after I lost connection:** Web Commander intentionally doesn't unkey on a network blip; Thetis's own MOX timeout drops the carrier, so make sure that timeout is set in Thetis. Unkey manually if needed.
- **iPhone keeps disconnecting:** use a normal Safari tab, not a home-screen shortcut.
