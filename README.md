# Presentations for Conference Talks

This repo contains my [reveal-md](https://github.com/webpro/reveal-md) slides for various conference talks.

## Prerequisites

- [nvm](https://github.com/creationix/nvm)

## Usage

```sh
nvm install
npm install
npm run watch // or "serve" instead of "watch", opens browser at http://localhost:1948
```

## Generating PDF

```sh
`npm bin`/decktape --size 1920x1080 http://localhost:1948/$PRESENTATION/slides.md $PRESENTATION.pdf
```
