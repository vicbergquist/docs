# 📋 Chapter 1: Introducing the My Pet Shop Web App

| **Project Goal**            | Get started with Vue.js by creating a static Pet Shop web app                                                     |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| **What you’ll learn**       | Setting up your Vue app, CSS Grid, Styling in Vue, code structure in preparation for moving forward.              |
| **Tools you’ll need**       | A modern browser like Chrome. If using Chrome, download Chrome DevTools for Vue.js. An account in CodeSandbox.io. |
| **Time needed to complete** | 1/2 hour                                                                                                          |

## Instructions

Since this is the very first Vue.js web project we're going to make, we'll start from scratch in [Code Sandbox](http://codesandbox.io). Create a Code Sandbox account and scaffold a starter Vue.js template by clicking [here](https://codesandbox.io/s/vue).

We're going to build a storefront for a fictional Pet Shop that will look like this:

![pet store](./images/petshop_chapter1_1.jpg)

In addition, we're going to create a switch that will change the look of the shop to resemble this:

![pet store](./images/petshop_chapter1_2.jpg)

Take a look at the code that was scaffolded by Code Sandbox for a basic Vue.js app. The first file you'll see is open by default: `main.js`. This is the main starting point of a Vue.js app. Note that in this file you import Vue from its npm package: `import Vue from "vue";`. Code Sandbox imports all the needed dependencies from npm to build the app; you can always check out the root `package.json` to find out which dependencies are needed.

`main.js` also initializes the app as a new Vue.js app and sets the div into which the app code will be injected. It also names the main component(`App`, line 3) and sets the template's name:

```js {3}
// main.js
new Vue({
  render: h => h(App)
}).$mount("#app");
```

Open up `App.vue`. In this file, the 'home' component is built. It contains the three main parts of a Vue.js Single File Component (SFC): a template, a script block, and a style block.

::: tip Note 💡
[Single File Components](https://vuejs.org/v2/guide/single-file-components.html) are the building blocks of a Vue app. They allow us to write our HTML, JavaScript and CSS all within a single `.vue` file. By using Single File Components, we can split our application (and components) into smaller (and reuseable) pieces to make it easier to build, read, understand and debug our app. This is made possible by [vue-loader](https://vue-loader.vuejs.org), which is a loader for [webpack](https://webpack.js.org).
:::

Note the first div in the template block has the id of `app` (line 3) - this is the div where the app code will be injected. There's also a `<HelloWorld>` component included underneath the logo image (line 5). This is an example of a SFC being included into `App.vue`.

```html {3,5}
<!-- App.vue -->
<template>
  <div id="app">
    <img width="25%" src="./assets/logo.png">
    <HelloWorld msg="Hello Vue in CodeSandbox!" />
  </div>
</template>
```

Open `components/HelloWorld.vue` and you'll find the source of the list of links that appears embedded in `App.vue`. This file also includes a script block with a `msg` variable and some more styles in a `<style>` block.

We're going to rip this sample app apart and recreate it! Let's get started.

## Build the Styles

Let's start in `App.vue`, since we don't have to make any changes to `main.js`. This workshop will teach you how to build a Vue app, meaning that CSS is outside the scope of this workshop, which is why we are simply copy-pasting styles into our app, rather than writing our own styles.

::: warning Note
Do not remove the template or script block  in `App.vue`. This will break your app. In this step we are only concerned with the styles of our app.
:::

Add the following style block at the bottom of the file, **replacing** the current `<style>` block:

```scss {4,87}
<style lang="scss">
	@import url("https://fonts.googleapis.com/css?family=Roboto");

	/*brown and mint*/
	/*dark brown 32292F
	light mint 99E1D9
	bisque F0F7F4
	dark mint 70ABAF
	light brown 705D56*/

	*,
	*:before,
	*:after {
		box-sizing: border-box;
	}

	body {
		margin: 0;
		padding: 0;
	}

	main {
		padding: 40px;
		font-family: "Roboto", "sans-serif";
		background: #fff top center repeat;
		color: #444;
		background-image: url("https://raw.githubusercontent.com/VueVixens/projects/master/petshop/images/bg.jpg");
	}

	h1,
	p {
		margin: 0 0 1em 0;
	}

	img {
		max-width: 100%;
		display: block;
		margin: 0 auto;
	}

	.app-container {
		max-width: 940px;
		margin: 0 auto;
		background-color: #fff;
	}

	.app-container > * {
		border-radius: 5px;
		font-size: 150%;
		margin-bottom: 10px;
	}

	.wrapper {
		display: grid;
		grid-gap: 10px;
		grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
		grid-auto-rows: minmax(150px, auto);
	}

	.wrapper > * {
		padding: 15px;
		border-radius: 5px;
	}

	.light-mint {
		background-color: #99e1d9;
	}

	.dark-mint {
		background-color: #70abaf;
	}

	.light-brown {
		background-color: #705d56;
		color: #f0f7f4;
	}

	.dark-brown {
		background-color: #32292f;
		color: #f0f7f4;
	}

	.bisque {
		background-color: #f0f7f4;
	}

	/*orange and green*/
	/*
	dark orange 771100
	orange CC6633
	light orange FF9900
	dark green 689980
	light green 86a193
	*/

	.orange-green {
		background-image: url("https://raw.githubusercontent.com/VueVixens/projects/master/petshop/images/bg2.jpg");
		.light-mint {
			background-color: #86a193;
		}

		.dark-mint {
			background-color: #689980;
		}

		.light-brown {
			background-color: #cc6633;
		}

		.dark-brown {
			background-color: #771100;
		}

		.bisque {
			background-color: #ff9900;
		}
	}

	.panel {
		/* needed for the flex layout*/
		margin-left: 5px;
		margin-right: 5px;
		flex: 1 1 200px;
	}

	.tall-panel {
		grid-row-end: span 2;
	}

	.app-header,
	.app-footer {
		flex: 0 1 100%;
		padding: 15px;
		text-align: center;
	}

	/* We need to set the margin used on flex items to 0 as we have gaps in grid.  */
	@supports (display: grid) {
		.wrapper > * {
			margin: 0;
		}
	}
</style>
```

This style block includes a few surprising things:

- **It uses paths to external images hosted on Github, rather than relative paths:** 
		This is because CodeSandbox doesn't host images; normally you'll just add an image on a relative path such as `/images/myImage.png`.
- **There is some funny 'grid' stuff going on:** 
		This style sheet and the template we will build make use of CSS Grid, a new way of making flexible, responsive 'masonry' layouts like this one with stacked 'blocks' of content. Learn more about CSS Grid [here](https://css-tricks.com/snippets/css/complete-guide-grid/).
- **There are two style sets:** Or at least two style patterns. One has a brown-mint theme (line 4), the other is orange-green (line 87) We'll make use of this soon.

::: tip Note 💡
You may have noticed that the `style` block that we replaced used a `scoped` keyword and our new one doesn't. This `scoped` keyword ensures that your styles will remain valid only for the current SFC, meaning that they are scoped to that specific component and won't affect any other HTML in other components. However, we want our new styles to be applied globally, and removed the `scoped` keyword. This means that our whole app will be affected by these styles.

Also note that we specified that we are using [Sass](https://sass-lang.com) to write our CSS in the new `<style>` block we just added. We do this because we think it's easier to manage our CSS with Sass. We can write Sass in our Vue component thanks to [vue-loader](https://vue-loader.vuejs.org).
:::

Adding this `<style>` block didn't do much to how our app template looks like except make the `<li>` group look strange. Let's fix this!

## Install Vuetify

Before we edit the template, we're going to install Vuetify. Vuetify is a User Interface (UI) component library that gives Material Design styles to your Vue apps. This means that you don't have to be doing any styling of your app if you don't want to, because Vuetify comes with ready-to-use UI components. 

In this chapter, we're only going to use it to create a toggle switch for our theme, but we'll use Vuetify and its UI components more in future chapters.

::: tip Note 💡
[Vuetify](https://vuetifyjs.com/en/getting-started/quick-start) is a semantic component framework for Vue. It aims to provide clean, semantic and reusable components for building your application.
:::

Install Vuetify by clicking the 'Add Dependency' button in the Dependency dropdown area on the left in CodeSandbox (you may have to scroll!). Search for 'Vuetify' and install it by clicking it. CodeSandbox will take care of almost all the rest.

Next, check whether Vuetify was installed as a dependency by opening `package.json` and checking the "dependencies" object. It should look like this (version numbers might vary):

```json
// package.json
"dependencies": {
  "vue": "^2.5.2",
  "vuetify": "1.2.9"
},
```

CodeSandbox only installs Vuetify as a dependency. To be able to actually use Vuetify in our app, we have to initialize it. 

Next, initialize Vuetify by opening `main.js` and adding these lines (4, 5 and 7) under the second `import`:

```js {4,5,7}
// main.js
import Vue from "vue";
import App from "./App.vue";
import Vuetify from 'vuetify';
import 'vuetify/dist/vuetify.min.css';

Vue.use(Vuetify);

Vue.config.productionTip = false;

new Vue({
  render: h => h(App)
}).$mount("#app");
```

This ensures that Vuetify's themes and components will be available throughout the Vue app and Vuetify CSS styles are included as well.

In order to have nice icons in our application, we also need to add Material icons to our `index.html` file. Please open `public/index.html` and add this string inside your `<head></head>` tag:

```html
<link href='https://fonts.googleapis.com/css?family=Roboto:300,400,500,700|Material+Icons' rel="stylesheet">
```
Next, we want to add the structure of our Pet shop app to our `App.vue` component, by overwriting the `<template>` block and the HTML inside it. 

::: warning Note
Do not delete the `<script>` or `<style>` block from your `App.vue` component. We only want to overwrite the `<template>` block.
:::

Overwrite the current `<template>` in `App.vue` with this markup:

```html
<template>
  <v-app>
    <main>
      <div class="app-container">
        <header class="app-header dark-brown">
          <h1>My Pet Store</h1>
        </header>
        <div class="wrapper">
          <div class="panel tall-panel light-mint">
              <h2>Pet Products</h2>
              <p>Premium Puppy Chow</p>
              <p>Kibble, sale in bulk, $20/lb</p>
              <img src="https://raw.githubusercontent.com/VueVixens/projects/master/petshop/images/food.png"/>
          </div>
          <div class="panel bisque">
              <h2>Donate</h2>
          </div>
          <div class="panel tall-panel light-brown">
              <h2>Adoptable Pets</h2>
              <p>Fisher, Chihuahua, age 3</p>
              <img src="https://raw.githubusercontent.com/VueVixens/projects/master/petshop/images/chihuahua.jpg"/>
          </div>

          <div class="panel bisque">
              <h2>Contact Us</h2>
          </div>
          <div class="panel tall-panel dark-mint">
              <h2>Pet of the Month</h2>
              <p>Meet Stanley, A young French Bulldog</p>
              <img src="https://raw.githubusercontent.com/VueVixens/projects/master/petshop/images/bulldog.jpg"/>
          </div>
          <div class="panel tall-panel light-mint">
              <h2>Success Stories</h2>
              <p>Bennie found his forever home!</p>
              <img src="https://raw.githubusercontent.com/VueVixens/projects/master/petshop/images/collie.jpg"/>
          </div>

          <div class="panel bisque">
              <h2>Special Events</h2>
          </div>

          <div class="panel bisque">
              <h2>Learn About Pet Ownership</h2>
          </div>
        </div>
        <footer class="app-footer dark-brown">
          <p>123 Main Street | Smithfield, RI 90987 | 345-456-5678</p>
        </footer>
      </div>
    </main>
  </v-app>
</template>
```

Wow, that made a big change! Suddenly, you have a storefront!

::: tip Note 💡
Note the use of `<v-app>` - this is a requirement of Vuetify and is a sure sign you'll have a Vuetify-themed app. If we want to use other Vuetify components they have to be inside `<v-app>`. If they are not, Vuetify will not know that they exist and they won't show up in our app.
:::

## Create a Theme Switch

Now we're going to actually use that Vuetify theme by creating a toggle switch. Pressing this switch will trigger a theme switch, so you'll use the 'orange' theme you may have seen in the `<style>` block we added earlier.

### Add the `.orange-green` class

You might see the `orange-green` class in our `App.vue`'s `<style>` block. Let's add this class to the `<main>` element inside our `<template>` and observe how all the colors and the background are change:

```html {4}
<!-- App.vue -->
<template>
	<v-app>
			<main class="orange-green">
				...
			</main>
	</v-app>
</template>
```

### Class Binding with `v-bind`

In order to make a toggle switch that changes the class of the `<main>` tag, we need to do something called [class binding](https://vuejs.org/v2/guide/class-and-style.htm ), which makes it possible to manipulate an element's class list.

::: tip Note
`v-bind` is one of [Vue's directives](https://vuejs.org/v2/guide/syntax.html#Directives). A directive is a special keyword that tells Vue to do *something* to the element. In Vue, directives appear as a prefixed HTML attribute: `v`, followed by a directive name, in this case: `bind`. Continue reading to learn how to use `v-bind`.
:::

To bind a class we need to use `v-bind` with the class attribute, `v-bind:class`, and passing an object to it `v-bind:class="{}"`. 

Replace the simple class on the `<main>` element with a class binding and add an object with a key of `'orange-green'` and give it `false` as a value (line 3):

```html {3}
<!-- App.vue -->
<template>
	<main v-bind:class="{'orange-green': false}">
		...
	</main>
</template>
```

Vue will evaluate the object and give the element the appropriate class, defined by the object key, depending on the truthiness of the value. Above, the `orange-green` class will not be applied, because its value is set to `false`.

Try to change `false` to `true` and vice versa. You can see how the class is applied by inspecting the element in your dev tools and how theme of you app is changing.

### Binding the Class to a Variable

Get excited! It's time to create your first Vue variable. This variable will be used to determine whether our `orange-green` class should be applied or not.

First, you have to add `data()` to your Vue component. This function should return an object of our Vue variables. Let's create one in the `<script>` block. 

Remove the current `components` block and add this `data()` function block (lines 5-9):

```js {5-9}
// App.vue
<script>
	export default {
		name: "App",
		data() {
			return {
				themeSwitched: false
			};
		}
	};
</script>
```

::: tip Note 💡
At this point you can remove the HelloWorld.vue component from the `components` folder as we won't need it. You can also remove the line that imports the HelloWorld.vue component.
:::

So, now you have a variable called `themeSwitched` and its default value is `false`.

In the `<main>` tag, replace `false` in the class binding with our newly created variable (line 3):

```html {3}
<!-- App.vue -->
<template>
	<main v-bind:class="{'orange-green': themeSwitched}">
		...
	</main>
</template>
```

Change `themeSwitched` value inside `data` from `false` to `true`. Again, you can see the color change effect.

Now we only need a toggle switch to change a theme. First we will create a button (we're using Vuetify so it will be a Vuetify button component). Let's place it in the `header` right after the `h1` tag:

```html {6}
<!-- App.vue -->
<template>
	...
	<header class="app-header dark-brown">
		<h1>My Pet Store</h1>
		<v-btn>Switch theme</v-btn>
	</header>
	...
</template>
```

Next, we want to add a click event handler to our button. We can use the `v-on` directive or its shortcut `@`. This handler will change `themeSwitched` value to its opposite value, toggling the color-changing class.

```html {6}
<!-- App.vue -->
<template>
	...
	<header class="app-header dark-brown">
		<h1>My Pet Store</h1>
		<v-btn @click="themeSwitched = !themeSwitched">Switch theme</v-btn>
	</header>
	...
</template>
```

Test your application by clicking the button. Looks nice, right?

**Congratulations! You've just finished Chapter 1!**

# Final result

![final result chapter 1](./images/petshop_chapter1_1.jpg)
