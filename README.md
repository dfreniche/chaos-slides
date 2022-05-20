# Chaos Slides Generator
A Chaos slides generator build for the community cafe at MongoDB World.

This slide generator will build a slide deck of however many slides you want.

The images from the slides can be either from a MongoDB Collection, or from Giphy.

## MongoDB Setup

Create a [MongoDB Atlas](https://www.mongodb.com/cloud/atlas/register?utm_campaign=joellord&utm_source=github&utm_medium=referral) account, and start up a new free cluster. 

Create a new database `chaos` with a collection `slides`. You can add new documents to the database. The documents should have an `image` field.

{
  _id: "...",
  image: "https://example.com/full/path/to/image.png"
}

Go to the Application Services, and create a new serverless [Function](https://www.mongodb.com/docs/atlas/app-services/functions/). Name this function `getRandom`, and put in the following code.

```js
exports = async function(arg){
  const NUMBER_OF_SLIDES = parseInt(arg.query.num) || 4;
  const collection = context.services.get("mongodb-atlas").db("chaos").collection("slides");
  const slides = await collection.aggregate([{$sample:{size: NUMBER_OF_SLIDES}}]);
  return slides;
}
```

Create an [HTTPS endpoint](https://www.mongodb.com/docs/atlas/app-services/endpoints/), and make sure that it runs the function you just create on GET requests.

## Setup Giphy

Go to the [Giphy Developers](https://developers.giphy.com/) page and create an account. From there, create a new API key.

## Setup the app

In the `index.js` file, fill in the values for the first two variables.

```js
const GIPHY_KEY = "Your Giphy API Key";
const SLIDES_ENDPOINT = "Your Atlas HTTPS Endpoint";
```

Use a static file web server such as [serve](https://github.com/vercel/serve), or use MongoDB's Atlas hosting service to host your files.

## Customize

You can change the `assets/intro.png` file to customize the intro slide as you see fit.