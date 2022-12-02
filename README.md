
# v-quantity-box
<a href="https://www.npmjs.com/package/v-quantity-box"><img src="https://img.shields.io/npm/dt/v-quantity-box.svg" alt="Downloads"></a>
<a href="https://www.npmjs.com/package/v-quantity-box"><img src="https://img.shields.io/npm/v/v-quantity-box.svg" alt="Version"></a>
<a href="https://www.npmjs.com/package/v-quantity-box"><img src="https://img.shields.io/npm/l/v-quantity-box.svg" alt="License"></a>

Vuetify combo input box for adding **Quantity** values with **Description**

*(README file is still under writing process..)*

## Features
- This package contains two text-fields - 1) Quantity 2) Description
- Add each Quantity values with Description
- Better use in inventory software where additional description needs while adding/updating each quantity
- Shows sum total of quantity as hint under quantity text-field
- Update & Delete quantity via v-chip component

### Installation

Use npm: ```npm install v-quantity-box```

### Prepare
At first make sure your project is Vue project, and has ```Vuetify``` as UI framework:
1. Install Vuetify:
```
npm install vuetify --save-dev
```
2. Add Vuetify to ```app.js``` or ```main.js```:
```js
import Vuetify from 'vuetify';
import 'vuetify/dist/vuetify.min.css';

Vue.use(Vuetify);
```

You also can use Vue plugin to install ```Vuetify``` by only one line command:
```
vue add vuetify
```

> Note: Make sure you are using the default Vuetify iconfont (mdi)

### Demo Images
![alt text](https://i.ibb.co/4sZr7jX/VQuantity-Box-01.png)
![alt text](https://i.ibb.co/cJLvFZd/VQuantity-Box-02.png)


Below code in the ```<template>```:
```html
<VQuantityBox dense dark outlined color="info" :quantity-array="quantity" @on-add="addToQuantity" @on-update="updateQuantity" @on-input="onQuantityInput" @on-chip-close="removeQuantity"></VQuantityBox>
```

### Usage
Add below code into your ```<script>```:
```js
import VQuantityBox from 'v-quantity-box'

export default {
    components: {
        VQuantityBox,
    },
    data: () => ({
        quantity: []
    }),
    methods: {
        addToQuantity(item) {
            this.quantity.push(item)
        },
        updateQuantity(obj) {
            const { index, item } = obj
            this.quantity[index] = Object.assign(this.quantity[index], item)
        },
        onQuantityInput(obj) {
            const { index, item } = obj
            this.quantity[index] = Object.assign(this.quantity[index], item)
        },
        removeQuantity(index) {
            this.quantity.splice(index, 1)
        }
    }
}
```

### Attributes

 - **dense** (boolean): Reduces the input height (Default: false)
 - **dark** (boolean): Applies the dark theme (Default: false)
 - **outlined** (boolean): Applies the outlined style to the input (Default: false)
 - **color** (String): Applies specified color (Default: teal)
 - **quantity-array** (Array): Array property to store values.
 - **quantity-label** (String): Label for Quantity field. (Default: Quantity)
 - **description-label** (String): Label for Description field. (Default: Description)

### Methods

(See Above script example to get better idea of methods)
- (README file is still under writing process..)

### License

MIT