# `Image`

Image processing library.

This library support encode/decode these formats:

| Format    | Input                                     | Output                                  |
| --------- | ----------------------------------------- | --------------------------------------- |
| RawPixels | RGBA 8 bits pixels                        |                                         |
| JPEG      | Baseline and progressive                  | Baseline JPEG                           |
| PNG       | All supported color types                 | Same as decoding                        |
| BMP       | ✅                                        | Rgb8, Rgba8, Gray8, GrayA8              |
| ICO       | ✅                                        | ✅                                      |
| TIFF      | Baseline(no fax support) + LZW + PackBits | Rgb8, Rgba8, Gray8                      |
| WebP      | No                                        | ✅                                      |
| AVIF      | No                                        | ✅                                      |
| PNM       | PBM, PGM, PPM, standard PAM               | ✅                                      |
| DDS       | DXT1, DXT3, DXT5                          | No                                      |
| TGA       | ✅                                        | Rgb8, Rgba8, Bgr8, Bgra8, Gray8, GrayA8 |
| OpenEXR   | Rgb32F, Rgba32F (no dwa compression)      | Rgb32F, Rgba32F (no dwa compression)    |
| farbfeld  | ✅                                        | ✅                                      |
| SVG       | ✅                                        |                                       |

See [index.d.ts](./packages/binding/index.d.ts) for API reference.

![CI](https://github.com/Brooooooklyn/image/workflows/CI/badge.svg)

## Support matrix

|                       | node10 | node12 | node14 | node16 | node18 |
| --------------------- | ------ | ------ | ------ | ------ | ------ |
| Windows x64           | ✓      | ✓      | ✓      | ✓      | ✓      |
| Windows x32           | ✓      | ✓      | ✓      | ✓      | ✓      |
| macOS x64             | ✓      | ✓      | ✓      | ✓      | ✓      |
| macOS arm64 (m chips) | ✓      | ✓      | ✓      | ✓      | ✓      |
| Linux x64 gnu         | ✓      | ✓      | ✓      | ✓      | ✓      |
| Linux x64 musl        | ✓      | ✓      | ✓      | ✓      | ✓      |
| Linux arm gnu         | ✓      | ✓      | ✓      | ✓      | ✓      |
| Linux arm64 gnu       | ✓      | ✓      | ✓      | ✓      | ✓      |
| Linux arm64 musl      | ✓      | ✓      | ✓      | ✓      | ✓      |
| Android arm64         | ✓      | ✓      | ✓      | ✓      | ✓      |
| Android armv7         | ✓      | ✓      | ✓      | ✓      | ✓      |
| FreeBSD x64           | ✓      | ✓      | ✓      | ✓      | ✓      |

## Performance

System info

```
OS: macOS 12.3.1 21E258 arm64
Host: MacBookPro18,2
Kernel: 21.4.0
Shell: zsh 5.8
CPU: Apple M1 Max
GPU: Apple M1 Max
Memory: 9539MiB / 65536MiB
```

```
node bench/bench.mjs

@napi-rs/image 202 ops/s
sharp 169 ops/s
In webp suite, fastest is @napi-rs/image
@napi-rs/image 26 ops/s
sharp 24 ops/s
In avif suite, fastest is @napi-rs/image
```

```
UV_THREADPOOL_SIZE=10 node bench/bench.mjs

@napi-rs/image 431 ops/s
sharp 238 ops/s
In webp suite, fastest is @napi-rs/image
@napi-rs/image 36 ops/s
sharp 32 ops/s
In avif suite, fastest is @napi-rs/image
```

## `@napi-rs/image`

See [Full documentation for `@napi-rs/image`](./packages/binding/README.md)

### Example

You can clone this repo and run the following command to taste the example below:

- `yarn install`
- `node example.mjs`

| Optimization                                                                                            | Raw                                                                                                                                                                 | Raw Size | Optimized Size |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------------- |
| `losslessCompressPng()` <br/>**Lossless**                                                               | <img src="./un-optimized.png" width="400" />[CC-BY-SA 3.0](https://creativecommons.org/licenses/by/3.0) by [Niabot](https://commons.wikimedia.org/wiki/User:Niabot) | `888K`   | `736K`         |
| `pngQuantize({ maxQuality: 75 })` <br/>**Lossy**                                                        | <img src="./un-optimized.png" width="400" />[CC-BY-SA 3.0](https://creativecommons.org/licenses/by/3.0) by [Niabot](https://commons.wikimedia.org/wiki/User:Niabot) | `888K`   | `248K`         |
| `compressJpeg()` <br/>**Lossless**                                                                      | <img src="./un-optimized.jpg" width="400" />                                                                                                                        | `192K`   | `184K`         |
| `compressJpeg(75)` <br/>**Lossy**                                                                       | <img src="./un-optimized.jpg" width="400" />                                                                                                                        | `192K`   | `104K`         |
| `new Transformer(PNG).webpLossless()`<br/>**Lossless**                                                  | <img src="./un-optimized.png" width="400" />[CC-BY-SA 3.0](https://creativecommons.org/licenses/by/3.0) by [Niabot](https://commons.wikimedia.org/wiki/User:Niabot) | `888K`   | `588K`         |
| `new Transformer(PNG).webp(75)`<br/>**Lossy**                                                           | <img src="./un-optimized.png" width="400" />[CC-BY-SA 3.0](https://creativecommons.org/licenses/by/3.0) by [Niabot](https://commons.wikimedia.org/wiki/User:Niabot) | `888K`   | `64K`          |
| `Transformer(PNG).avif({ quality: 100 })`<br/>**Lossless**                                              | <img src="./un-optimized.png" width="400" />[CC-BY-SA 3.0](https://creativecommons.org/licenses/by/3.0) by [Niabot](https://commons.wikimedia.org/wiki/User:Niabot) | `888K`   | `536K`         |
| `new Transformer(PNG).avif({ quality: 75, chromaSubsampling: ChromaSubsampling.Yuv420 })`<br/>**Lossy** | <img src="./un-optimized.png" width="400" />[CC-BY-SA 3.0](https://creativecommons.org/licenses/by/3.0) by [Niabot](https://commons.wikimedia.org/wiki/User:Niabot) | `888K`   | `64K`          |

```js
import { readFileSync, writeFileSync } from 'fs'

import {
  losslessCompressPng,
  compressJpeg,
  pngQuantize,
  Transformer,
  ResizeFilterType,
  ChromaSubsampling,
} from '@napi-rs/image'
import chalk from 'chalk'

const PNG = readFileSync('./un-optimized.png')
const JPEG = readFileSync('./un-optimized.jpg')
// https://github.com/ianare/exif-samples/blob/master/jpg/orientation/portrait_5.jpg
const WITH_EXIF = readFileSync('./with-exif.jpg')
const SVG = readFileSync('./input-debian.svg')

writeFileSync('optimized-lossless.png', await losslessCompressPng(PNG))

console.info(chalk.green('Lossless compression png done'))

writeFileSync(
  'optimized-lossy.png',
  await pngQuantize(PNG, {
    maxQuality: 75,
  }),
)

console.info(chalk.green('Lossy compression png done'))

writeFileSync('optimized-lossless.jpg', await compressJpeg(readFileSync('./un-optimized.jpg')))

console.info(chalk.green('Lossless compression jpeg done'))

writeFileSync('optimized-lossy.jpg', await compressJpeg(readFileSync('./un-optimized.jpg'), { quality: 75 }))

console.info(chalk.green('Lossy compression jpeg done'))

writeFileSync('optimized-lossless.webp', await new Transformer(PNG).webpLossless())

console.info(chalk.green('Lossless encoding webp from PNG done'))

writeFileSync('optimized-lossy-png.webp', await new Transformer(PNG).webp(75))

console.info(chalk.green('Encoding webp from PNG done'))

writeFileSync('optimized-lossless-png.avif', await new Transformer(PNG).avif({ quality: 100 }))

console.info(chalk.green('Lossless encoding avif from PNG done'))

writeFileSync(
  'optimized-lossy-png.avif',
  await new Transformer(PNG).avif({ quality: 75, chromaSubsampling: ChromaSubsampling.Yuv420 }),
)

console.info(chalk.green('Lossy encoding avif from PNG done'))

writeFileSync(
  'output-exif.webp',
  await new Transformer(WITH_EXIF)
    .rotate()
    .resize(450 / 2, null, ResizeFilterType.Lanczos3)
    .webp(75),
)

console.info(chalk.green('Encoding webp from JPEG with EXIF done'))

writeFileSync(
  'output-overlay-png.png',
  await new Transformer(PNG).overlay(PNG, 200, 200).png()
)

console.info(chalk.green('Overlay an image done'))

writeFileSync("output-debian.jpeg", await Transformer.fromSvg(SVG, 'rgba(238, 235, 230, .9)').jpeg())

console.info(chalk.green('Encoding jpeg from SVG done'))
```
