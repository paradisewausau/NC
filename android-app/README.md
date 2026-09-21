# Northwoods Cab - Android App Package

This folder is a Progressive Web App (PWA): the same dispatch console, packaged
so it can be installed on an Android phone as a real app icon with a splash
screen and its own full-screen window (no browser bar).

Files:
- `index.html` - the app itself
- `manifest.json` - tells Android the app's name, icon, and colors
- `service-worker.js` - caches the app shell so it still opens if the signal drops
- `icon-192.png`, `icon-512.png`, `icon-180-apple.png` - app icons

## Step 1: Host it somewhere with HTTPS

Android will only offer to install a PWA if it's served over HTTPS (not
opened directly as a `file://` path). The free, no-account way to do this:

1. Create a new repository on GitHub and upload all the files in this folder
   to it (keep them all in the same top-level folder, don't put `index.html`
   in a subfolder).
2. In the repo's Settings -> Pages, enable GitHub Pages for the `main`
   branch, root folder.
3. GitHub gives you a URL like `https://yourname.github.io/your-repo/`.

Any static host works the same way (Netlify drag-and-drop, Cloudflare Pages,
Vercel, your own web server, etc.) - GitHub Pages is just free and simple.

## Step 2: Install it on Android

1. Open that HTTPS URL in Chrome on the Android phone.
2. Chrome shows an "Install app" prompt automatically, or: tap the
   three-dot menu -> **Install app** (or **Add to Home screen**).
3. It now behaves like any other installed app: home-screen icon, its own
   task-switcher entry, full-screen window, works offline once opened once.

At this point you have a real installable app on the phone with no app
store, no APK, and no developer account needed.

## Step 3 (optional): Turn it into an actual signed .apk

If you specifically need an installable `.apk` file (e.g., to sideload
without visiting a URL, or to eventually publish to the Play Store), use
**PWABuilder** - a free Microsoft tool built for exactly this:

1. Finish Step 1 so the app is hosted at an HTTPS URL.
2. Go to https://www.pwabuilder.com and paste that URL in.
3. It scores your manifest/service worker (this one is already set up
   correctly) and offers an **Android** package download.
4. Download the generated `.apk` (unsigned, for testing) or the Play Store
   bundle (it walks you through signing).

This uses Google's own [Trusted Web Activity](https://developer.chrome.com/docs/android/trusted-web-activity)
approach - it's a real Android app, not a hack, and it's the same technique
Google, Twitter/X, and many other companies use to ship their PWAs to the
Play Store.

## Notes

- This app stores all data in the browser's local storage on that one
  device - there's still no shared backend, so the desktop, mobile web, and
  installed-app versions on different devices won't sync with each other.
- The service worker caches the app's own files for offline use, but
  Tailwind, Font Awesome, and the PDF export library load from a CDN and
  need a connection the first time (and to re-fetch updates).
