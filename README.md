# switch vue
An on/off switch component for Vue.js with theme support.

## Installation

```bash
	npm install --save ofcold-switch
```

## Usage

```html
	<!-- Html  -->
	<OfcoldSwitch v-model="on_ready"></OfcoldSwitch>
```

```javascript
	//	Javascript
	import OfcoldSwitch from 'ofcold-switch';

	export default {
		data:() => ({
			on_ready: false
		}),
		components: {
			OfcoldSwitch
		}
	}
```
