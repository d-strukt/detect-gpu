# Detect GPU

![](http://img.badgesize.io/TimvanScherpenzeel/detect-gpu/master/dist/detect-gpu.min.js.svg?compression=gzip&maxAge=60)
[![npm version](https://badge.fury.io/js/detect-gpu.svg)](https://badge.fury.io/js/detect-gpu)
[![dependencies](https://david-dm.org/timvanscherpenzeel/detect-gpu.svg)](https://david-dm.org/timvanscherpenzeel/detect-gpu)
[![devDependencies](https://david-dm.org/timvanscherpenzeel/detect-gpu/dev-status.svg)](https://david-dm.org/timvanscherpenzeel/detect-gpu#info=devDependencies)

Classify GPU's based on their benchmark score in order to provide an adaptive experience.

## Stability and reporting issues

In the current state `detect-gpu` should be considered to be experimental and should not yet be used in production.

If you are interested in helping me out collecting renderer names please add the following script tag to your webpage:

```html
<script defer src="https://unpkg.com/detect-gpu/scripts/analytics_embed.js"></script>
```

The script simply checks the name of the users unmasked renderer and sends it as a Google Analytics event to my account. The contents of the script can be found [here](https://github.com/TimvanScherpenzeel/detect-gpu/blob/master/scripts/analytics_embed.js).

## Demo

[Live demo](https://timvanscherpenzeel.github.io/detect-gpu/)

## Installation

Make sure you have [Node.js](http://nodejs.org/) installed.

```sh
 $ npm install detect-gpu
```

## Usage

`detect-gpu` uses benchmarking scores in order to determine what tier should be assigned to the user's GPU. If no `WebGLContext` can be created or the GPU is blacklisted `TIER_0` is assigned. One should provide a HTML fallback page that a user should be redirected to.

By default are all GPU's that have met these preconditions classified as `TIER_1`. Using user agent detection a distinction is made between mobile (mobile and tablet) prefixed using `GPU_MOBILE_` and desktop devices prefixed with `GPU_DESKTOP_`. Both are then followed by `TIER_N` where `N` is the tier number.

In order to keep up to date with new GPU's coming out `detect-gpu` splits the benchmarking scores in `4 tiers` based on rough estimates of the market share.

By default `detect-gpu` assumes `10%` of the lowest scores to be insufficient to run the experience and is assigned `TIER_0`. `40%` of the GPU's are considered good enough to run the experience and are assigned `TIER_1`. `30%` of the GPU's are considered powerful and are classified as `TIER_2`. The last `20%` of the GPU's are considered to be very powerful, are assigned `TIER_3`, and can run the experience with all bells and whistles.

You can tweak these percentages when registering the application as shown below:

```js
import { getGPUTier } from "detect-gpu";

const GPUTier = getGPUTier({
  mobileBenchmarkPercentages: [10, 40, 30, 20], // (Default) [TIER_0, TIER_1, TIER_2, TIER_3]
  desktopBenchmarkPercentages: [10, 40, 30, 20], // (Default) [TIER_0, TIER_1, TIER_2, TIER_3]
  forceRendererString: "Apple A11 GPU", // (Development) Force a certain renderer string
  forceMobile: true // (Development) Force the use of mobile benchmarking scores
});
```

## Development

```sh
$ npm start

$ npm run serve

$ npm run lint

$ npm run dist

$ npm run deploy

$ npm run parse-analytics

$ npm run update-benchmarks
```

## Licence

My work is released under the [MIT license](https://raw.githubusercontent.com/TimvanScherpenzeel/detect-gpu/master/LICENSE).

`detect-gpu` uses both mobile and desktop benchmarking scores from [https://www.notebookcheck.net/](https://www.notebookcheck.net/).

The unmasked renderers have been gathered using the analytics script that one can find in `scripts/analytics_embed.js`.
