# Making an API: PokeGIFs


##### Building a simple API in Rails

This API will allow its consumers to lookup a Pokemon based on its name or Pokedex ID number, get some basic info about that Pokemon, and also get a random GIF associated with it.

Pokemon is a huge franchise, and there are currently about 800 different Pokemon. Rather than trying to maintain a list of all these different Pokemon, and also trying to find and store a bunch of GIFs for each one, our API is just going to get all its data from two other APIs, and smash the results together.

Those two APIs are:
- The Pokemon API
- The GIPHY API

## Goals
We want to build an API with a single endpoint: `/pokemon/:id`
This endpoint has a dynamic element, `:id`. We should support `:id` being both a number, like `5`, or the name of a Pokemon, like `charmander`
Our endpoint should return some JSON including a little bit of data about our Pokemon, and a random GIF of that Pokemon. For example, our JSON might look like this:
``` json
{
  "id":25,
  "name":"pikachu",
  "types":["electric"],
  "gif":"https://media0.giphy.com/media/mQpZtX0gKDESA/giphy.gif"
}
```

## Pokemon API
The Pokemon API is pretty easy to get started with, so we'll start there. Spend a few minutes looking at the documentation.

We're really only going to use one of the endpoints for this assignment: `/pokemon/:id`

The API doesn't require authentication to use, so we can open up Postman and try running a couple queries.

Try looking up a Pokemon based on an` id` number, like `93` for example. You can also lookup a Pokemon by name instead of `id` by just putting a name instead of a number into the `:id` part of the URL (ie. `/pokemon/pikachu`). Notice how the endpoint on the Pokemon API mimics the one we'll be creating on our API.

We'll be making use of the `name`, the `id`, and the `types` from the response.

## GIPHY API
The GIPHY API is a little more complicated, because we need a client key to use it. You'll need to first signup for an account, and then register an app from the developer dashboard. There's no need to request a production key; a beta key is sufficient for the assignment.

Take a minute to look over the API documentation.

Again, there's only one endpoint we need for this assignment: `/v1/gifs/search`

The GIPHY endpoint is a little more complicated than the Pokemon endpoint though, because we need to include several query string parameters:

- `api_key`: This is where we'll attach our API client key for GIPHY
- `q`: Our search term. Should be the name of the Pokemon
- `rating`: You can optionally include an MPAA rating if you're worried about seeing Not Save For Work GIFs. Its unlikely you'll see anything particularly shocking, but if you want to be sure you don't get any inappropriate GIFs, you can set the rating to `g` or `pg`

So including the query string parameters, your call to the GIPHY API will probably look something like this:
`
https://api.giphy.com/v1/gifs/search?api_key=YOUR_KEY_HERE&q=pikachu&rating=g
`
Give this a try in Postman. Try searching for a few things in the `q` query string parameter, and see what you get in return.

## Setting Up Our API Endpoint
Now that we've experimented a bit with the APIs we'll be using, we need to start building our API.

Remember that we need one endpoint for our API, `/pokemon/:id`. You'll need a controller for this endpoint. It's probably reasonable to call this the `PokemonController` (the plural of Pokemon is Pokemon rather than Pokemons).

You'll also need a route in your routes file, and an action in your controller. What do you think is an appropriate route and action for what we're trying to do? Think about where you might've seen a similar URL in the past, and what controller action that URL routed to. For example, in Photogur, what did `/pictures/:id` route to?

Let's start by just having it return a really simple JSON response to make sure its working. Something like this:
``` json
{
  "message" : "ok"
}
```

You can render this JSON response by calling render in your controller action like this:

``` ruby
render json: { "message": "ok" }
```

## Adding the Pokemon Info
Once you've decided what your controller, route, and action should look like, we'll need to actually query the Pokemon API in our controller action. We can use a gem to help us with this. HTTParty is a good choice, because its simple and widely used. Typhoeus is a bit more robust of a choice that works similarly. They're both already in your `Gemfile`. Here's what this might look like with HTTParty:
``` ruby
res = HTTParty.get("http://pokeapi.co/api/v2/pokemon/pikachu/")
body = JSON.parse(res.body)
puts body["name"] # should be "pikachu"
```
How can we use this within the controller action we defined in the previous step? How can we have it work for any value that's in our `:id` parameter, not just "pikachu"?

Well, when users call our API, they'll add either an ID number or a name to the `:id` portion of the URL. So let's just pass along whatever we were given in `params[:id]` to the Pokemon API.

Let's update our controller action from early so that it makes this request. It should look something like this now:

``` ruby
res = HTTParty.get("http://pokeapi.co/api/v2/pokemon/#{params[:id]}/")
body = JSON.parse(res.body)
name  = body["name"]

render json: { "message": "ok" }
```
We're still just returning { "message" : "ok" } though. So we'll need to expand our JSON response to include the info we get from this API. Remember we want to pull out the name, id, and types from the response. I've shown you how to extract the name, so examine the response data and figure out how to extract the id and types (In case you aren't familiar with Pokemon, types will be things like grass, fire, and electric).

After this stage, our response should look like:

``` json
{
  "id":25,
  "name":"pikachu",
  "types":["electric"]
}
```
**HINT** Don't be afraid to use your debugging skills to examine the response data. Putting in a `byebug` or `binding.pry` statement after `body = JSON.parse(res.body`) will give you a chance to examine the data in more detail.

## Adding the GIFs
So far we have our API endpoint. It makes a request to the Pokemon API, and then returns info about whatever Pokemon we asked for. This is cool, but the real secret sauce of our API is adding a GIF to this information, so let's tackle that now.

Again, we're gonna use HTTParty to help us here, but our URL will be a little different:

``` ruby
res = HTTParty.get("https://api.giphy.com/v1/gifs/search?api_key=YOUR_KEY_HERE&q=pikachu&rating=g")
body = JSON.parse(res.body)
```

You probably want to stick this code into that same controller action from before, between where you're querying the Pokemon API, and where you're rendering a JSON response.

Take a look at the response you got from GIPHY (again, you can use `byebug` or `binding.pry` to help you here). You should see something like this:
``` json
{
  "data" : [
    {
      ...
      "url" : "some/url"
      ...
    },
    {
      ...
      "url" : "some/other/url"
      ...
    },
    ...
  ],
  ...
}
```
What we're seeing is an array called `data` which contains the info about several gifs matching what we searched for. There's some other stuff in there too that doesn't really matter as much to us (`pagination` and `meta`).

`data` contains a bunch of objects, each containing several URLs and other information about the GIF. The field we're going to use is `url`.

So to access the first GIF from the response GIPHY gave us, we could do something like this:
``` json
res = HTTParty.get("https://api.giphy.com/v1/gifs/search?api_key=YOUR_KEY_HERE&q=pikachu&rating=g")
body = JSON.parse(res.body)
gif_url = body["data"][0]["url"]
```
There are some questions we need to figure out the answers to before we go too much further though.

Similar to our previous query to the Pokemon API, we need to make our `q` query string param work for any Pokemon, not just pikachu. In this case though, our search term **must be the name**, we can't lookup the GIF based on an `id` number. So you might need to get the name from somewhere other than the `:id` parameter in our API's URL (**Hint**: Don't forget that our previous call to the Pokemon API gave us some info about the Pokemon we're looking for, like for example the name).

One more thing to think about, we probably don't want to include our API key in our source code. If anyone takes a look at our source code, they'll find our key, and they can steal our identity! Instead, we should store it in an environment variable, and reference the environment variable in our code. Something like:
``` json
HTTParty.get("https://api.giphy.com/v1/gifs/search?api_key=#{ENV["GIPHY_KEY"]}&q=pikachu&rating=g")
```
And in our `.bash_profile` or `.bashrc` file:

``` bash
export GIPHY_KEY="YOUR_KEY_HERE"
```

Once we've got a response from GIPHY, we'll need to add our GIF url into our JSON response. Our final response should look something like this:
``` json
{
  "id":25,
  "name":"pikachu",
  "types":["electric"],
  "gif" : "https://giphy.com/gifs/pokemon-pikachu-SHyuhBtRr8Zeo"
}
```
