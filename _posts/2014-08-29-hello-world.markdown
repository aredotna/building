---
layout: post
title:  "Middleware"
date:   2014-08-29 19:56:25
---

###TLDR;
- We have already started development on our new isomorphic javascript client, Ervell.
- ([More][isomorphic-links] on isomorphic javascript)
- Goals for Ervell
  - lightweight
  - responsive
  - push "collective file system" concept (channels nested under user, user lands on their own content initially)
  - be seo friendly
  - easier and more straightforward way to connect blocks to different channels
  - encourage exploration

![Ervell][ervell-image]

- Ervell uses Artsy's [Ezel][ezel] boilerplate, which is really just a set of conventions to hook various Express.js applications together in a way that allows an application to share client-side and "server"-side code.
- This allows you to reap the benefits of server rendered templates (speed, SEO friendly, etc) while retaining the ability to write rich client side applications without rewriting your templates.
- [Ezel][ezel] is easier to learn after looking at Artsy's [force-public][force-public] repo.
- Middleware is a powerful way to process requests and responses at different stages of an Express.js application
- An example of middleware we use is a lightweight authentication middleware, which adds a user's token to the header of a particular API request.

{% highlight coffeescript %}
module.exports = (p_req, res, next) ->
  # hook into backbone-super-sync
  Backbone.sync.editRequest = (req) ->
    # if passport has added a user to the request,
    # add the users token to the header
    if p_req.user?
      req.set('X-AUTH-TOKEN': p_req.user.get('authentication_token'))

  next()
{% endhighlight %}

{% highlight coffeescript %}
express = require "express"
routes = require "./routes"
auth = require '../../lib/middleware/auth'

app = module.exports = express()
app.set "views", __dirname + "/templates"
app.set "view engine", "jade"
app.get "/:username", auth, routes.user
{% endhighlight %}

- Middleware (according to wikipedia) is a concept that originated out of the need to link older legacy applications with newer ones.
- Where does middleware end? `The distinction between operating system and middleware functionality is, to some extent, arbitrary`
- The aesthetic of middleware: functional and ideally elegant, capable of translating and mutating data from one point to another
- Arena wants to be middleware between a single person their future ideas





[ezel]:        http://ezeljs.com/
[force-public]:   https://github.com/artsy/force-public
[isomorphic-links]: http://x.are.na/UdKZhKH
[ervell-image]: https://trello-attachments.s3.amazonaws.com/53825c12f5cc26118b4ed172/53dbf1f5d9405191696ddff8/1978x1656/7e058a12fd3c95b23fd912683a9a9ac6/Arena-Channel%26Connect3.jpg
