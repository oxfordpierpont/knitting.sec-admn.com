# Loop & Loom

Interactive knitting education and pattern companion (the cashmere bear-ear hat project). A static single-page app served by Nginx in a small Docker container, deployed with Dokploy.

Live: https://knitting.sec-admn.com

## Layout

- `site/index.html` contains the whole app (markup, styles and script).
- `site/assets/` holds the three colorway photos. Filenames include a content hash, so Nginx caches them for a year.
- `nginx.conf` sets gzip, cache headers, security headers and a `/healthz` endpoint.
- `Dockerfile` uses `nginx:stable-alpine` and listens on port 80. Dokploy's Traefik handles HTTPS.

## Run locally

```
docker build -t loop-and-loom .
docker run --rm -p 8080:80 loop-and-loom
```

Then open http://localhost:8080.

## Deploy

Dokploy project **Loop & Loom**, application `knitting`. Build type is Dockerfile, the source is this repo's `main` branch, and the domain is `knitting.sec-admn.com` on port 80 with a Let's Encrypt certificate. To redeploy, push to `main` and trigger a deploy in Dokploy.

## Compatibility changes from the original file

- The base64 images, which were embedded twice each, are now separate files. The HTML dropped from 2.5 MB to about 80 KB.
- Text inputs are 16px on phones, so iOS Safari no longer zooms in when a field is tapped.
- Safe-area padding was added for the iPhone notch and home indicator (`viewport-fit=cover`).
- Page changes use `scrollTo(0,0)`. Older Safari threw an error on `behavior:'instant'`.
- Backup restore works on older Safari: `Object.hasOwn` was replaced, and there is a `FileReader` fallback for `File.text()`.
- Export works on iOS and Firefox: the download link is attached to the page and the file stays available for 60s.
- Small fixes: `100dvh` for the iOS viewport, no sticky hover on touch screens, no text inflation in landscape.

Visual output is pixel-identical to the original in WebKit (iPhone 13 and desktop).
