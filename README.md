### Vue Svg Render

Library component convert svg to png image and this component build for Vue3

#### Install

`$ npm install vue-svg-to-png`

#### How To Use
##### Use at Single Component
```javascript
import VueSvgToImg from 'vue-svg-to-png'

export default {
	components: {
		VueSvgToImg
	}
}
```
##### Use at Global Component
```javascript
import { createApp } from 'vue'
import App from './App.vue'
import VueSvgToImg from 'vue-svg-to-png'

createApp(App).use(VueSvgToImg).mount('#app')
```

```html
<template>
	<VueSvgToImg
		src="/icons/bubble.svg"
		color="#800080"
		:width="320"
		:height="320"
	/>
</template>

<!-- Dynamic Color -->
<template>
	<VueSvgToImg
    src="/icons/bubble.svg"
    color="#800080"
    color-hover="red"
    :static="false"
    :width="320"
    :height="320"
	/>
</template>
```
### Data
                    
Property  | Description
------------- | -------------
src  | source of svg file
width  | number or string
height  | number or string
alt | string
color | string example '#000000' or 'red'
color-hover | string, this property use for dynamic color
static | boolean, this property as options for static image or not
custom-class | string, for customize default component
