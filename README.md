# Cropme

Cropme is a customizable and easy to use javascript image cropper plugin.

[See the demo](https://shpontex.github.io/cropme)

## Features

Support:
 - Two-dimensional translation
 - Scaling
 - Free rotation
 - Multi-touch support (pinch-zoom, two finger rotation, ...)
 - Base64 and blob exportation
 - Multiple croppers

## Architecture

```
dist/
├── cropme.min.css   (compressed)
└── cropme.min.js    (UMD, compressed)
```

## Installation

**npm**

```
npm install cropme
```

**Download**

 [Download the project](https://github.com/shpontex/cropme/archive/master.zip) and extract it in your project folder, then import it.

```html
<link rel="stylesheet" href="path-to/cropme.css">
<script src="path-to/cropme.js"></script>
```

## Usage

### Syntax

```js
new Cropme(element, options);
```

- **element** (`HTMLElement`, required): *the cropper wrapping HTML element.
Must be a [flow content element](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories#Flow_content) or an image.*


- **options** (`Object`, optional): *The configuration options, see [**Options**](#options).*

### Example

**Vanilla javascript**

```html
<div id="container"></div>

<script>
  var element = document.getElementById('container');
  var cropme = new Cropme(element);
  cropme.bind({
    url: 'images/naruto.jpg'
  });
</script>

<!-- or use image tag -->
<img src="images/naruto.jpg" id="myImage" />
<script>
  var element = document.getElementById('myImage');
  new Cropme(element);
</script>
```

**JQuery**

```html
<div id="container"></div>

<script>
  var example = $('#container').cropme();
  example.cropme('bind', {
    url: 'images/naruto.jpg'
  });
</script>

<!-- or use image tag -->
<img src="images/naruto.jpg" id="myImage" />
<script>
  $('#myImage').cropme();
</script>
```

## Options

The values in the examples below are the default values.

### Container

- Target: the container of the cropper.
- Key: `container`
- Parameters:
  - **width** (`int`, unit: `px`, default: `300`): *the outer container width*
  - **height** (`int`, unit: `px`, default: `300`): *the outer container height*

#### Example

```js
container: {
  width: 500,
  height: 400
}
```
### Viewport

- Target: the part that will be cropped.
- Key: `viewport`
- Parameters:
  - **width** (`int`, default: `100`): *the viewport width*
  - **height** (`int`, default: `100`): *the viewport height*
  - **border** (`object`): *the viewport frame border*
    - **enable** (`bool`, default: `true`): *toggle the border*
    - **width** (`int`, unit: `px`, default: `2`): *the border width*
    - **color** (`string`, unit: `hex, rgba, hsl`, default: `#fff`): *the border color*

#### Example

```js
viewport: {
  width: 100,
  height: 100,
  border: {
    enable: true,
    width: 2,
    color: '#fff'
  }
}
```

### Zoom

- Target: the image zoom options
- Key: `zoom`,
- Parameters:
  - **min** (`number`, default: `0.01`): *minimum zoom*
  - **max** (`number`, default: `3`): *maximum zoom*
  - **enable** (`bool`, default: `true`): *enable or disable the zoom feature*
  - **mouseWheel** (`bool`, default: `true`): *enable or disable mouse wheel zoom*
  - **slider** (`bool`, default: `false`): *toggle the slider input*

#### Example

```js
zoom: {
  min: 0.01,
  max: 3,
  enable: true,
  mouseWheel: true,
  slider: false
}
```

### Rotation

- Target: the image rotation
- Key: `rotation`
- Parameters:
  - **enable** (`bool`, default: `true`): *enable or disable the rotation*
  - **slider** (`bool`, default: `false`): *toggle the slider input*
  - **position** (`string`, default: `right`, available: `right, left`): *the slider input position*

#### Example

```js
rotation: {
  enable: true,
  slider: false,
  position: 'right'
}
```

### Custom class

- Target: the container class
- Parameter:
  - **customClass** (`string`, default: `null`): *the class of the container*

#### Example
```js
{
  customClass: 'my-custom-class'
}
```

## Methods

### bind()

*Binds an image and return a promise after the image is loaded.*

#### Arguments

The `bind()` method expects an `Object` containing:
- **url** (required)
  - **type**: `String`
  - **description**: The url of the image to bind.
- **position**
  - **type**: `Object` with `x` and `y` keys.
  - **description**: An object that contains the x and y coordinates.\
  If not specified, the image is centered horizontaly and verticaly.
- **scale**
  - **type**: `number`
  - **description**: The scale of the image, 1 is the original image size.\
  If not specified, the image will takes the container's height and scale \
  automaticaly.
- **angle**
  - **description**: The rotation of the image by an angle in degree
  - **type**: `number`
  - **unit**: `degree`
  - **default**: 0

#### Example

```js
$('#myImage').cropme('bind', {
  url: 'images/naruto.jpg',
  position: {
    x: 230,
    y: 30
  },
  scale: 1.3,
  angle: 90
});
```

### rotate()

*Rotate the image to the given angle.*

#### Arguments

- **angle**
  - **description**: The angle the image will be rotated to.\
  The rotation is not relative to the current image rotation.
  - **type**: `number`
  - **unit**: `degree`

#### Example

```js
$('#myImage').cropme('rotate', 90);
```

### export()

*Returns a promise with the cropped image.*

#### Arguments

As a parameter, the `export()` function can receive:
1. An `Object` containing:
  - **type**
    - **type**: `String`
    - **default**: `base64`
    - **possible value**: `base64`, `blob`
    - **description**: The image exportation format
  - **width**
    - **type**: `int`
    - **description**: The width of the output images, the height will be \
    proportional.
  - **scale**
    - **type**: `number`
    - **description**: The size of the ouput, relative to the original image size.\
    If `scale` has a value, the `width` get ignored.

  2. A `String` specifying the exportation format (`base64` or `blob`)

:warning: Calling `export()` without parameters returns a **base64** image with \
the original viewport size.

#### Example

```js
// string
$('#myImage').cropme('export', 'blob');

// object
$('#myImage').cropme('export', {
    type: 'base64',
    width: 800
});

// no parameter
$('#myImage').cropme('export');
```

### position()

*Returns an object specifying the image configuration*

#### Example

```js
$('#myImage').cropme('position');
```

**Output**

```js
Object {
  x: 20,
  y: 40,
  scale: 1.4,
  deg: 45
}
```

### destroy()

*Destroy the cropme instance*

#### Example

```js
$('#myImage').cropme('destroy');
```
