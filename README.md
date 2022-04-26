# Smooth Gradient Sass Function

<https://smooth-gradient-sass-function.dskd.jp/>

Inspired by [Naoki Matsuda's Pen](https://codepen.io/readymadegogo/pen/pPLJgR) & [Lukas Hermann's Pen](https://codepen.io/lhermann/pen/qmpMGQ).

[![Netlify Status](https://api.netlify.com/api/v1/badges/1e62fa99-883c-46bc-8bb2-948a81953597/deploy-status)](https://app.netlify.com/sites/smooth-gradient-sass-function/deploys)

- **v3.x is compatible to dart-sass.**
- [v2.x](https://github.com/oti/smooth-gradient-sass-function/tree/v2.1.0) is compatible to libsass(node-sass).

## You can get smooth gradient sass function

Gradients are scrim, easeOutSine, and clothoid curve.

```scss
// `scrim` smoothing
@function scrim($from: [#000000, 0], $to: [transparent, 100%]) {
  // validate arguments
  // ...

  $scrim: (
    1: 0,
    0.738: 0.19,
    0.541: 0.34,
    0.382: 0.47,
    0.278: 0.565,
    0.194: 0.65,
    0.126: 0.73,
    0.075: 0.802,
    0.042: 0.861,
    0.021: 0.91,
    0.008: 0.952,
    0.002: 0.982,
    0: 1,
  );
  @return _make-gradient-list($scrim, $from, $to, $start, $end);
}

@function _make-gradient-list($map, $from, $to, $start, $end) {
  $color-stops: ();
  @each $key, $mod in $map {
    $position: $mod * ($end - $start) + $start;
    $new-stop: color.mix($from, $to, $key * 100%);
    $color-stops: list.append($color-stops, $new-stop $position, "comma");
  }
  @return $color-stops;
}
```

```scss
// `easeOutSine` smoothing
@function easeOutSine($from: [#000000, 0], $to: [transparent, 100%]) {
  // validate arguments
  // ...

  $easeOutSine: (
    1: 0,
    0.917: 0.053,
    0.834: 0.106,
    0.753: 0.159,
    0.672: 0.213,
    0.591: 0.268,
    0.511: 0.325,
    0.433: 0.384,
    0.357: 0.445,
    0.283: 0.509,
    0.213: 0.577,
    0.147: 0.65,
    0.089: 0.729,
    0.042: 0.814,
    0.011: 0.906,
    0: 1,
  );
  @return _make-gradient-list($scrim, $from, $to, $start, $end);
}
```

```scss
// `clothoid` smoothing
@function clothoid($from: [#000000, 0], $to: [transparent, 100%]) {
  // validate arguments
  // ...

  $clothoid: (
    1: 0,
    0.3: 0.5,
    0.15: 0.65,
    0.075: 0.755,
    0.037: 0.8285,
    0.019: 0.88,
    0: 1,
  );
  @return _make-gradient-list($scrim, $from, $to, $start, $end);
}
```

## Usage

### from git

1. git clone and move to your project.

```bash
git clone git@github.com:oti/smooth-gradient-sass-function.git
mv smooth-gradient-sass-function/src/_smooth-gradient.scss your/project/scss/path
```

2. `@use` in your .scss file.

```scss
@use "your/project/scss/path/smooth-gradient" as gradients;
```

### from npm

1. npm install.

```shell
npm i smooth-gradient-sass-function
```

2. `@use` in your .scss file.

```scss
@use "../(to project root)/node_modules/smooth-gradient-sass-function" as
  gradients;
```

### write function

3. write `gradients.scrim()` sass function and argument in argument of native `linear-gradient()` function.

```scss
.elem {
  // default blend is `#000` to `transparent`
  background-image: linear-gradient(to bottom, gradients.scrim());
}
```

## Any angel, Any color, Any opacity

```scss
.box {
  // start left
  background-image: linear-gradient(to right, gradients.scrim());
}
```

```scss
.box {
  // start left bottom
  background-image: linear-gradient(45deg, gradients.scrim());
}
```

```scss
.box {
  // 1st arg is a list, containing start color code and position (default: #000000 0)
  background-image: linear-gradient(
    to bottom,
    gradients.scrim($from: #ff0000 20%)
  );
}
```

```scss
.box {
  // 2nd arg is a list, containing end color code and position (default: transparent 100%)
  background-image: linear-gradient(
    to bottom,
    gradients.scrim($from: #ffff00 20%, $to: rgba(#ffff00, 0.2) 60%)
  );
}
```

```scss
.box {
  // args are determined by type, and can be mixed
  // this is equivalent to $from: #ffff00 0, $to: rgba(255, 255, 0, 0) 50%
  background-image: linear-gradient(
    to bottom,
    gradients.scrim($from: #ffff00, $to: 50%)
  );
}
```

```scss
.box {
  // positions can be absolute values, as long as they are compatible
  background-image: linear-gradient(
    to bottom,
    gradients.scrim($from: 3em, $to: 16em)
  );
}
```

## LICENSE

[MIT license Copyright (c) 2018 oti](LICENSE.txt)
