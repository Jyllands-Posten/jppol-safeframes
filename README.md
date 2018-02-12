# sharedsafeframes
JPPol shared safeframes implementation

Based on https://github.com/InteractiveAdvertisingBureau/safeframe

# Table of Contents
1. [Safeframe side setup](#safeframe-side)
2. [Host side setup](#host-side)
3. [Calling banner](#calling-banner)
4. [Special formats - Callbacks](#callbacks)

# Safeframe side
Upload the safeframe folder of this project to an external address
eg. //ebimg.dk/ux/data/safeframes/

# Host side
Include jppolhost.min.js in page
Creates an object called jppolAdOps, with the following exposed elements

* [safeframeInit](#safeframeInit)
* [setupFinalPos](#setupFinalPos)


# Initialize JPPol Safeframes from publisher site
## safeframeInit
* function

The initialize function takes an object as argument, with [safeframeURL](#safeframeURL) and [adtechNetworkId](#adtechNetworkId) as mandatory settings

Initialize function
``` js
  var sfOptions = {
    safeframeURL: '//ebimg.dk/ux/data/safeframes/index.html', // [Path from setup](#safeframe-side)
    adtechNetworkId: '123'
  }
  jppolAdOps.safeframeInit(sfOptions)
```

### debug info from safeframe

[Se debug](#debug)

## Options

### safeframeURL
* Mandatory [string]

Set the URL of were the safeframe HTML file is placed

setup example
```js
  var sfOptions = {
    safeframeURL: '//ebimg.dk/ux/data/safeframes/index.html' // [Path from setup](#safeframe-side)
  }
```

### adtechNetworkId
* Mandatory [string / object]

Set the ADTECH network id, can be passed as an object to differentiate between network ids based on [device type](#device)

String setup example
```js
  var sfOptions = {
    adtechNetworkId: '123'
  }
```

Device Object setup example
```js
  var sfOptions = {
    adtechNetworkId: {
      desktop: '123',
      smartphone: '123.0',
      tablet: '123'
    }
  }
```

### baseBannerSrc
* Optional [string / object]
* default: '//adserver.adtech.de/addyn/3.0/'

Set the base banner url, from which the final banner script src will be build, can be passed as an object to differentiate between network ids based on [device type](#device)

String setup example
```js
  var sfOptions = {
    baseBannerSrc: '//adserver.adtech.de/addyn/3.0/'
  }
```

Device Object setup example
```js
  var sfOptions = {
    adtechNetworkId: {
      desktop: '123',
      smartphone: '123.0',
      tablet: '123'
    }
  }
```

### device
* Optional [string]
* default: 'desktop'

Set current device type, used to differentiate adtechNetworkId and baseBannerSrc

Setup example
```js
  var sfOptions = {
    device: 'desktop'
  }
```

### messageCallback
* Optional [function]

See also: [Callbacks](#callbacks)

Set the function which should be called when the safeframe is triggering onPosMsg
An object[example shown below] is passed as argument to the callback function containing 4

Argument example
``` js
var messageObject = {
  'placement': 'string', // string
  'type': 'string', // ['msg'/'error']
  'content': 'string', // safeframes doesnt support other types at the moment
  'nuked': nuked
}
```

Init setup example
```js
  var sfOptions = {
    onPosMsg: handleCallBack
  }
```

### wallpaper

#### wallpaperHandler
* Optional [boolean]
* default: false

Should be set to true if wallpaper is an option on the page, usually only for desktop devices

example
```js
  var sfOptions = {
    wallpaperHandler: true
  }
```

#### wallpaperSelector
* Optional [string]

Can be body or regular CSS selector / ID

example
```js
  var sfOptions = {
    wallpaperSelector: 'adtechWallpaper'
  }
```

#### wallpaperPositions
* Optional [Array / string]

Name of positions of which to check for wallpaper.

example
```js
  var sfOptions = {
    wallpaperPositions: ['wallpaper','monster']
  }
```

### allowNuke
* Optional [boolean]
* default: true

Mainly for debugging purposes

example
```js
  var sfOptions = {
    allowNuke: true
  }
```

### debug
* Optional [boolean]
* default: false (will be set to true, if fulldebug is true)

For debugging purposes
Log statements from jppol-safeframes is prefixed with jppol-safeframes, followed by either filename or "inside safeframe"

setup example
```js
  var sfOptions = {
    debug: false
  }
```

output example
```js
  jppol-safeframes: jppolhost.js safeframe posMsg followleft msg safeframeloaded|w160|h604
```


### fulldebug
* Optional [boolean]
* default: false

For debugging purposes
BEWARE: This will add debug information directly into the banner container

example
```js
  var sfOptions = {
    fulldebug: false
  }
```



# Calling a banner

To call a specific banner call [setupFinalPos](#setupFinalPos) with position data

## setupFinalPos
* function

Takes an object with all the data needed to create the correct banner

positionData

```js
var positionData = {
  placement: 'monster',
  bannerID: '4040617',
  type: '277',
  sfWidth: 930,
  sfHeight: 180,
  sfZIndex: 1,
  shared_data: {
    elementPos: document.getElementById(destID).getBoundingClientRect()
  }
}
```

### placement
* Mandatory [string]

placement id / name for banner


```js
var positionData = {
  placement: 'monster'
}
```

### bannerID
* Mandatory [string]

ADTECH placement ID or alias for banner

```js
var positionData = {
  bannerID: '4040617'
}
```

### type
* Mandatory [string]

ADTECH placement type / sizeid

```js
var positionData = {
  type: '277'
}
```

### destID
* Mandatory [string]

ID of element to serve banner in

```js
var positionData = {
  destID: 'monster_trgt'
}
```

### Dimensions
* Mandatory
* sfWidth [number]
* sfHeight [number]
* sfZIndex [number]

Set the width, height and z-index of the safeframe

```js
var positionData = {
  sfWidth: 930,
  sfHeight: 180,
  sfZIndex: 1
}
```

### keyValues
* optional [array]

Key values to be added to banner called besides criteo, adform and rubicon key values
Could be prebid key values

```js
var positionData = {
  keyValues: ['prebidwhatever=1']
}
```

### shared_data
* Optional [object]

Pass shared data to the safeframe, this data is open to every banner to use

```js
var positionData = {
  shared_data: {
    elementPos_top: document.getElementById(destID).getBoundingClientRect().top
  }
}
```

## Passing private data to banner

### privateDataOptions
* Optional [object]

```js
var privateDataOptions = {
  key: 'privateKeyForSF',
  private_data1: 'Private data',
  private_data2: {
    more_data: 'More private data'
  }
}
```

# Callbacks

Handling special banner formats, like wallpaper, skyskraper(follow) etc.

At the moment safeframes only support strings to be sent from inside the safeframe

## load

When the banner is loaded the safeframe sends the a message with the content "safeframeloaded"

### data

Since the safeframe API doesnt support objects in messaging, additional data is appended with a pipe [|]
If a prebid placeholder is present the prebid type will be appended, otherwise the width and height dimensions will be appended.
See example implementation from safeframe below

```js
if (sf_align.querySelector('.prebidPlaceholder') !== null) {
  loadedMessage += '|prebid'
} else if (sf_align.querySelector('.prebidPlaceholder_xhb') !== null) {
  loadedMessage += '|prebid_xhb'
} else {
  loadedMessage += '|w' + sf_align.clientWidth + '|h' + sf_align.clientHeight
}
```
