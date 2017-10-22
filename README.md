# vue-drag-verify [![NPM version](https://img.shields.io/npm/v/vue-drag-verify.svg)](https://www.npmjs.com/package/vue-drag-verify)

> This is a vue component, which is sliding to unlock for login or sign up. This is used to protect your web app from bot attack.
[demo](https://ashleylv.github.io/vue-drag-verify/)

## Installation

``` bash
 npm install vue-drag-verify --save
```

## Usage
``` xml
<drag-verify :width="width" 
			 :height="height" 
			 :text="text" 
			 :success-text="successText" 
			 :background="background" 
			 :progress-bar-bg="progressBarBg" 
			 :completed-bg="completedBg" 
			 :handler-bg="handlerBg" 
			 :handler-icon="handlerIcon" 
			 :text-size="textSize" 
			 :success-icon="successIcon" 
			 :circle="getShape"></drag-verify>
```

``` javascript
import Vue from 'vue'
import dragVerify from 'vue-drag-verify'

export default {
  name: 'app',
  components:{
    dragVerify
  }
}
```
## Props

Property|Type|Default|Description
---|---|---|---
width|Number|200|The width of the component
height|Number|60|The height of the component
text|String|swiping to the right side|The text shows on the component
successText|String|success|The text shows when it’s successful
background|String|#ccc|The background color of the component
color|String|#ffffff|The color of the text
progressBarBg|String|#FFFF99|The backgound color of the progress bar
completedBg|String|#66cc66|The backgound color of the component when the button dragged to the right side
circle|Boolean|true|If true, the shape of component is round
handlerIcon|String|-|The icon of handler
successIcon|String|-|The icon of handler when the button dragged to the right side
handlerBg|String|#fff|The background color of the handler
textSize|String|20px|Font size of prompt message
