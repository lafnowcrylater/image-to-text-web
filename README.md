# Image to Text

A minimal Svelte app that extracts text from images in the browser using [Tesseract.js](https://github.com/naptha/tesseract.js).

No server required — OCR runs entirely client-side.

## Features

- Upload PNG, JPEG, or WebP images
- Live image preview before extraction
- Extracts text via Tesseract.js (English)
- Error handling and processing state feedback

## Getting Started

```bash
npm install
npm run dev
```

## Stack

- [Svelte](https://svelte.dev) + TypeScript
- [Tesseract.js](https://github.com/naptha/tesseract.js) — loaded via CDN