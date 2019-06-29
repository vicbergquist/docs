# 📋 Chapter 3: Connect your Project to an API

| **Project&nbsp;Goal** | Learn how API calls work and how to implement them in your web app                                                                                                                                  |
| --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **What&nbsp;you’ll&nbsp;learn**       | Using the [DogCEO API](https://dog.ceo/dog-api/) you will load dog images dynamically, instead of using dummy data                                                                                             |
| **Tools&nbsp;you’ll&nbsp;need**       | A modern browser like Chrome. If using Chrome, download Chrome DevTools for Vue.js. An account in CodeSandbox.io. If you get lost, import the starting point for this chapter [here](https://github.com/VueVixens/projects/tree/master/chapter-2-end). Instructions on how to do this are in [Appendix 1](appendix_1.md) |
| **Time needed to complete** | 1 hour

## Instructions

If you need to restart your project, clone [this repo](https://github.com/VueVixens/projects/tree/master/chapter-1-end) into Code Sandbox by clicking on the **Import from GitHub** link on the bottom left of the main page, and then pasting the repo's url into the fields.

So far, we have placed images of dogs onto our screens via some static JSON data that we imported into a component. That's great for demo purposes, but in real life, you're almost always going to build web apps that consume real data that's coming from either your own data sources, or externally, from somewhere on the internet. Let's learn how to consume third-party data.

To perform API calls we will need a library called [axios](https://github.com/axios/axios). It's a promise-based HTTP client that works both in the browser and in a node.js environment.

::: tip Note 💡
Originally, Vue supported its own way of making API calls using .ajax; but this resource was deprecated as Axios's standalone library worked very well for this purpose, removing the need for an integrated solution. Read more about this decision [here](https://medium.com/the-vue-point/retiring-vue-resource-871a82880af4).
:::

## Add Axios

First, add Axios's library to your project dependencies. To do so in Code Sandbox, click on `File Editor` tab -> `Dependencies` -> `Add Dependency` and search for `axios`. Import axios into the component where we will perform our API call - `views/Pets.vue`. 

Import axios right after the start of the script block (line 3):

```js {3}
// ./views/Pets.vue
<script>
  import axios from "axios";
  import {Dogs} from '../data/dogs';
  import Dog from '../components/Dog';
  export default {
    components: {
      appDog: Dog,
    },
    data() {
      return {
        dogs: Dogs,
      };
    },
  };
</script>
```

All our calls will use the same base URL with different endpoints and because we don't want to write this base URL every time, axios lets us set a base URL that all axios calls will use.

Right under the import for axios, add the base URL to Axios' options in `views/Pets.vue` (line 4):

```js {4}
// ./views/Pets.vue
<script>
  import axios from "axios";
  axios.defaults.baseURL = "https://dog.ceo/api";
  ...
</script>
```

Now we are ready to make our first API call.

## Call the API

Let's replace the first static image with the random Husky picture from the Dog CEO API. First we have to check which endpoint we have to use. Looking at the API's [documentation](https://dog.ceo/dog-api/) we can find out that we need to append `/breed/husky/images/random` to the base API call (the `api` part is already in our base URL).

We want a new image to replace the old one right when the component is created, so let's add a `created()` hook after the `data()` property (line 15):

```js {15}
// ./views/Pets.vue
<script>
  import axios from "axios";
  import {Dogs} from '../data/dogs';
  import Dog from '../components/Dog';
  export default {
    components: {
      appDog: Dog,
    },
    data() {
      return {
        dogs: Dogs,
      };
    },
    created() {}
  };
</script>
```

::: warning Note💡
  Make sure to add a comma after the data object and then add the `created()` hook! If you don't do this, the component will throw an error and your app will stop working.
:::

Inside the created hook we will add our first query to the API. To perform a GET request Axios uses the `axios.get` method. The result will be a [JavaScript promise](https://developers.google.com/web/fundamentals/primers/promises), so we have to provide success and failure callbacks to it. 

::: tip Note💡
This is our app's first lifecycle hook! [Lifecycle hooks](https://vuejs.org/v2/guide/instance.html#Instance-Lifecycle-Hooks) are very useful when you want fine control over when to run blocks of code.
:::

For now, let's simply print the query result to the console. Inside `created()` place this highlighted axios snippet (3-10):

```js {3-10}
// ./views/Pets.vue
created() {
  axios
    .get("/breed/husky/images/random")
    .then(response => {
      console.log(response);
    })
    .catch(error => {
      console.log(error);
    });
  }
```

In the browser view in Code Sandbox, switch to the Pets tab. You should see an object in your console. Drill into it by clicking its left-hand arrow. We are interested in its `data` field. You can see we have a status `success` and a message with an image URL (you can copy/paste it to your browser and discover a cute Husky).

## Use the API 1 - Replace Some of the Static Data

Let's replace our Husky image with this new one. First, we should find a Husky in our dogs array with an `Array.find` method. It will check the `dogs` array items that we are already loading into the component from the `data/dogs.js` data file one by one to find the first item matching provided criteria. In our case the criteria is a `breed` equal to `husky`. 

Replace the `console.log()` inside the `then` callback in the Axios call we just implemented with the highlighted lines (6-7):

```js {6-7}
// ./views/Pets.vue
created() {
  axios
    .get("/breed/husky/images/random")
    .then(response => {
      const husky = this.dogs.find(dog => dog.breed === 'husky');
      console.log(husky);
    })
    .catch(error => {
      console.log(error);
    });
  }
```

Ok, we have found a husky, which you can see in the `console.log()`. You can also see him in your app's Pets page - look for 'Max', listed as a husky. Now let's provide him with a new image by reassigning the image url from the static data to data coming from the API. 

Add the highlighted line (8) under the lines you just added above.

```js {8}
// ./views/Pets.vue
created() {
  axios
    .get("/breed/husky/images/random")
    .then(response => {
      const husky = this.dogs.find(dog => dog.breed === 'husky');
      console.log(husky);
      husky.img = response.data.message;
    })
    .catch(error => {
      console.log(error);
    });
  }
```

You should see the image change to a random husky image pulled from the Dog CEO API.

## Use the API 2 - Randomize the Images

Let's try to load a random image for each dog in our `dogs` array. The first thing we need is a proper endpoint for each breed we have. We will create an array of links using the `.map` method.

::: tip Note 💡
The `map()` method returns a new array with the results of calling a provided function on every element in the calling array.
:::

To create an array of links, we create a `linksArray` constant and add it inside the `created()` hook, like this (lines 3-5):

```js {3-5}
// ./views/Pets.vue
created() {
  const linksArray = this.dogs.map(
        dog => "/breed/" + dog.breed + "/images/random"
      );
  axios
    .get("/breed/husky/images/random")
    .then(response => {
      const husky = this.dogs.find(dog => dog.breed === 'husky');
      console.log(husky);
      husky.img = response.data.message;
    })
    .catch(error => {
      console.log(error);
    });
  }
```

We're taking the breed of each dog in the array and inserting it inside the endpoint string (we used the same one previously for husky, but `breed` was hard-coded to a static value there).

At this point, we have to perform multiple API calls using all the links we've just created in our `linksArray` constant - because many API calls exist in our static data. To help us do this, axios has some helper functions called `axios.all` and `axios.spread` that we wil use.

We will provide an array of our requests to the first one, `axios.all`; it will return an array of responses. Then, we should use `axios.spread` to spread this array into multiple arguments. To create an array of queries we will use a `.map` method on our `linksArray`, performing `axios.get` for each link. 

Add this snippet (lines 6-13) right under the `linksArray` snippet you added just before.

```js {6-13}
// ./views/Pets.vue
created() {
  const linksArray = this.dogs.map(
        dog => "/breed/" + dog.breed + "/images/random"
      );
  axios.all(linksArray.map(link => axios.get(link)))
   .then(
     axios.spread((...res) => {
       this.dogs.forEach((dog, index) => {
         dog.img = res[index].data.message;
       });
     })
   );
  }
```

::: tip Note 💡
  The forEach() method executes a provided function once for each array element in `linksArray`. It's looping through the static data and adding a random image to `this.dogs`. So, after we've got an array of images in response, we are iterating through our `dogs` array again, replacing each dog image with a corresponding new one from the API (`index` is the index of the current element being processed in the array; it is the same for both arrays because response objects are placed in the same order they were sent).
:::

Now we have new images each time our `Pets` component is created (you can see the images change on page refresh or simply by switching the tabs from `pets` to `home` and back). The dogs' names and breeds are still being drawn from static data, but the images are coming from the API, matched with the static dog's breed.

The only remaining problem is that we can still see old images for a short moment when we enter the pets tab. Let's clear the dogs images before we perform a query. Add this code at the start of the `created()` hook (lines 3-5):

```js {3-5}
// ./views/Pets.vue
created() {
  this.dogs.forEach(dog => {
     dog.img = "";
  });
  const linksArray = this.dogs.map(
        dog => "/breed/" + dog.breed + "/images/random"
      );
  axios.all(linksArray.map(link => axios.get(link)))
   .then(
     axios.spread((...res) => {
       this.dogs.forEach((dog, index) => {
         dog.img = res[index].data.message;
       });
     })
   );
}
```

**Now we initially see empty dog portraits and then images are loaded from the API. Progress!**

# Final result
![chapter 3 result](./images/petshop_chapter3.jpg)
