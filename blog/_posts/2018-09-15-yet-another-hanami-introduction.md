---
title: How to Build a Web Application
tags: beginner ruby hanami web
toc: true
published: false
---


This is the first installment of a repeating segment on basic web design and popular frameworks. I say 'popular' frameworks because that's what I happen to know. I haven't take the time to learn web development for Haskell or Scala because it doesn't match the languages I use for web applications.

### What is a web application?

{::nomarkdown}
<p>This begs an important question: '<a href="#" data-toggle="popover" title="HelloInternet.jpg" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/spongebob_internet.jpg" height="100%" width="100%" />'>what</a> <a href="#" data-toggle="popover" title="AlicornCat.jpg" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/cat_let_me_introduce_you_to_the_internet.jpg" height="100%" width="100%" />'><em>is</em></a> <a href="#" data-toggle="popover" title="IllBeYourGuide.jpg" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/cat_further_into_the_internet.jpg" height="100%" width="100%" />'>the</a> <a href="#" data-toggle="popover" title="A E S T H E T I C | 90s vision of tech" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/aesthetic_internet.jpg" height="100%" width="100%" />'>internet</a>?' I'll describe some of the structure behind basic websites to you. The internet consists of two things: static content (pages/text, <a href="https://en.wikipedia.org/wiki/File_format" >media files,</a> etc.) and messages (<a href="https://en.wikipedia.org/wiki/Representational_state_transfer">REST services/APIs</a> return <a href="https://en.wikipedia.org/wiki/JSON">JSON</a> format, <a href="https://en.wikipedia.org/wiki/Ajax_(programming)">AJAX</a> allows asynetc.). Possibly the best example of AJAX requests is the continuous news feed on Facebook, which incrementally fetches data for the user without redirection. It occurs in the 'background' of the user-interface.
</p>


<p>
Essentially, the <a href="#" data-toggle="popover" title="BeneathTheSurface.jpg" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/frontend_vs_backend.jpg" height="100%" width="100%" />'>surface</a> of the web is static/dynamic pages or HTML based mobile applications (like Twitter). The initial request provides a document skeleton and references (<code>link</code>/<code>script</code>) to other resources that style the document, provide app or UI logic, or enhance UI interactivity in some way. Some of the 'dynamics' we observe just involve reorganization of material the user already has (e.g. buttons, menus, sorting wiki tables, etc). On the other hand, modern single-page applications like facebook allows users to 'post' in the background onto their feed, and load the new post, without users ever refreshing the skeleton. A pratically infinite 'feed' can provide an expensive waste of memory (<a href="http://www.schools.com/imagesvr_ce/8525/social-media-waste-of-time.jpg">and time</a>). But the '<a href="#" data-toggle="popover" title="REST services are the backbone of persistence and retrieval" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/rest-api.jpg" height="100%" width="100%" />'>magic</a>' behind dynamic Facebook/Twitter feeds (i.e. no reloading or redirection) is the AJAX/REST calls referred to previously.
</p>
{:/}


### Web application terminology

Some of this terminology may seem like a foreign language or just abstract concepts without structure or connections to those unfamiliar with 2010s web application design. I'll describe some of these terms briefly. The prevailing conventions for 2010s (server-oriented) web app design is described as the Model-View-Controller (MVC) paradigm that describes data flow from the user's browser (the 'view', or what the user sees) through the logical code and into a persistence layer (such as a database). I'll describe the framework from the database on up. We'll omit more complex MVVM UI-oriented patterns since we'll mostly focus on the server-app/database essentials with the Hanami framework rather than focusing extensively on the UI.

![https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller](img/mvc_diagram.png "Simple MVC overview")

#### Database

The [database](https://en.wikipedia.org/wiki/Database) is a persistence layer for web applications allowing programmatic access, deduplication, backup facilities, networking, and sometimes maximum data compression. 1990s and 2000s web applications often stored data as files or directories, relying on CGI scripts to parse and/or retranslate information. Databases are faster and have query optimization for faster data retrieval than file-system based solutions for web app data persistence. Conventional databases rely on the SQL-family languages for data retrieval and focus internally on deduplication, minimal data models, indexing, and transactional integrity. Modern databases rely on fast networking for perceived persistence but are arguably less fault tolerant. I prefer SQL databases for deduplication and minimal hardware requirements and the abundance of documentation. I'll leave basic database concepts such as the [data model](https://en.wikipedia.org/wiki/Database_model), [normalization](https://en.wikipedia.org/wiki/Database_normalization), [relation](https://en.wikipedia.org/wiki/Relational_model), and [query optimization](https://en.wikipedia.org/wiki/Query_optimization) as an excercise to the readers.

#### Model

The data model is a much more abstract concept than the database alone. The database is a persistance layer for data focused on organization, deduplication, and querying. It contains a model for a record and returns a subset of the data from queries as quick as possible. But the database must transfer the data to application code for calculation, processing and display to the user or to send back to persistence. However, the basic data model is often defined completely by the database table/model, in terms of record attributes and relations to other records.

Inside of application level code, data is retrieved through a connection to the database and the data is translated into language-specific types (integers, floats, etc.). List/arrays and hashmaps are ditched in favor of *bona-fide* [OOP objects](https://en.wikipedia.org/wiki/Object-oriented_programming). The [Object Relational Mapping](https://en.wikipedia.org/wiki/Object-relational_mapping) (ORM) layer is an abstraction that allows programmers to interact with the data by providing access to querying, joining, bulk data insert, and other convenience methods. Data coming out of the database is typically valid, but constraints for incoming data can be applied at the application level (not just attribute type, but more complex constraints [i.e. even numbered ints only] that are more easily expressed in the high-level application code than at the database layer. There are a few instances when you would want to bypass the ORM and many ORM-database combinations allow for direct querying and JSON output.

#### Controller

If the model/ORM/db is the guts of the app then the controller is the brains. At the most basic level, it is a class that contains methods called '[actions](#action)' that perform model-level changes, calculations, or view rendering. While the 'controller' and the 'model' are both Ruby code in this case, the controller action invoked by the URI => routing may or may not contain model objects for database interaction; users cannot interact with the database without passing through first the controller and next the ORM model. Controllers invoke framework-specific methods (render, redirect, etc.) that trigger additional content-specific actions for the framework to modfiy the users view. Note that controller code is also used to construct [REST APIs](https://en.wikipedia.org/wiki/Representational_state_transfer) which is nowadays the most common method for all database interaction.

#### View

Views and view-partials are web components (HTML, JS, CSS) that are loaded into the users browser after the server receives a request. In a simple web application, the user receives a single HTML page (a view) from hitting a certain URI. In other cases, a complex front-end may AJAX-request a partial for a panel or tab, without refreshing or redirecting the user's browser. Facebook is a good example of the latter, where continuous post loading occurs via AJAX.

At first glance, view partials might not seem more like a convenience function for users that simply improve speed an application quality. However, they address separation of concerns for developers (sidebars, queues, feeds, and other components can be viewed, debugged, tested, and developed separately. Second, they dramatically affect app usability, as the user has more power to affect server-side activity through single page apps without refreshing. It simplifies the workflow of user actions in sometimes complicated workflows.

#### Action

While a View manifests as true client asssets (HTML, JS, CSS) an action represents the response of the server to certain HTTP verbs from the view. If our controller was called `home` and the primary action for the homepage called `index`, the combination would be referred to in the routing framework as `home#index`.

#### Partial

A partial is a portion of a full HTML page that is loaded separately or assembled on the server piece by piece to generate a full HTML page. Proceeding with the example of Facebook, there are 4 partial views that the user can see on the Facebook home page. The news feed in the center of the page is a partial that displays data from a number of posts, n (10? 20? I don't know). On the right side you see a partial for stories and advertising. On the left is a partial for apps, events, groups, etc. Messenger mini-windows also behave like partials. As mentioned before, these partial views may be fully assembled into a single HTML page through a process called 'templating' that happens on the server before the result is sent to the user. Alternatively, a view may be loaded that has an AJAX or aother request for information (HTML, XML, JSON) from the server that, when loaded, assembles into a partial on the client. The former process makes use of what is called a 'templating' engine and is discussed later.
#### D.O.M.

The Document Object Model (DOM) is essentially contents of the HTML layout, specifying UI objects that can have data or actions bound to them or serve web functionality (forms, buttons, inputs etc.). HTML examples include `div`, `li`, and `span`. SVG/XML objects like `circle`, `path`, etc. can have actions bound to them as well (as popularized by the D3JS framework. The D.O.M. is static browser data until reactivity/response is encoded with the first-gen browser programming language `javascript`.




## Our First Lightweight Application 'Clone', a Ruby Web App

{::nomarkdown}
<p>Let's build a lightweight social media app, like Twitter. We'll call it <a href="#" data-toggle="popover" title="WittyAppName.jpg" data-placement="auto" data-trigger="hover" data-html=true data-content='<img src="img/storm_trooper.png" height="100%" width="100%" />'><code>clone</code></a>. Essentially we'll want to let users log in, read their feed, and post short text posts that others can read.
</p>

{:/}


#### App Bootstrap 

```bash
# Use rvm if you don't have an appropriate ruby install and/or gemset
# Were gonna go through the whole postgres setup cause this is hard mode
hanami new clone --database=postgres --test=rspec 
bundle install
# bundle exec hanami server
mkdir logs # We'll put out app and db log files here.
```

##### Postgres database

This requires a PostgreSQL installation (we're using 9.6.5). Use your [operating system's package manager](https://en.wikipedia.org/wiki/List_of_software_package_management_systems) to install postgres, or install [from source](https://www.postgresql.org/). We'll set up a server in a non-standard location with your user account, such that elevated privileges are not required. On a related note, [best practices would suggest we create a separate application user for the database](https://www.postgresql.org/docs/9.3/static/app-createuser.html), to avoid using the superuser.

```bash
echo '.env.test' >> .gitignore # We're gonna make sure you don't commit your postgres credentials to git
# Feel free to replace the whoami subshell with your username
initdb -D db/clone -U $(whoami) -W 
# Note the final message:
#    pg_ctl -D db/clone -l logfile start
pg_ctl -D db/clone -l logs/postgres_db.log start

# Now our database server is alive but empty.
# Edit .env.test and set the DATABASE_URL to "postgresql://username:password@localhost/clone_test
export HANAMI_ENV=test
bundle exec hanami db prepare
```

#### Testing and BDD

The prevailing paradigm for software development is to write specs and tests around what you expect functions to produce, for models to behave, or how UIs render/react. Two competing opinions about testing include test-first (TDD/BDD) or protoype-first(test last). The conventional wisdom about test first is that if you truly understand the design of your app, then you can write tests for edge cases, functionality, and interactivity around/between functions/classes etc. before you write the code, in a way that prevents you from writing test that only partially address each function and don't test every input.

For example, in a previous blog post, we wrote a recursive fibonacci function. The function is simple enough, but it didn't include type checking, and could be subject to cryptic TypeErrors from incorrect input. We could write a test that 'covers' the whole function and demonstrates that under certain conditions the correct output is produced, but usually tests are needed to ensure your functions can handle incorrect input as well. Errors aren't intrinsically bad, but you want your code to raise exceptions in the most germane part of your code and with the most descriptive error message possible.

To begin testing, we'll start by testing that the home page include the word 'clone' and the phrase 'a simple blog with embedded images/memes'. This example will be very similar to the Hanami.rb [Getting Started](http://hanamirb.org/guides/1.0/getting-started/) guide. The first test run should fail and you should fully understand the reason *why*. As you inspect the output, you'll notice that it's a `404` page, because the 'root' route `/` isn't set by default. So next we'll tell Hanami we want the root URI to reference/route towards the index method of the home controller. I'll explain controllers and methods in a second.

```ruby
#spec/web/features/visit_home_spec.rb
require 'spec_helper'
require 'features_helper'

describe 'Visiting the home page' do
  it 'contains the the title and subtitle' do
    visit '/' # The test framework actually navigates to the '/' URI
    expect(page.body).to include('Clone')
  end
end
```

```bash
# Run first to make sure it fails, and that you understand the reason *why* it fails.
bundle exec rake spec
```

Now set the URI=>action mapping in `routes.rb`. You'll note the error will change to reflect the absence of a controller named `home`. Run the next hanami command with bundler to create a controller and an action. Afterwards, you'll see the list of files generated and need to make your test pass by completing the `index.html.erb`. Any additional content you expect to see on the fully rendered home page (header, icons, footer, description etc.) should *also* be in your test/spec.

```ruby
#apps/web/config/routes.rb
root to: 'home#index'
```

```bash
bundle exec hanami generate action web home#index # Create templates and controller/action
emacs apps/web/templates/home/index.html.erb # Add some text to the html that makes your app pass!
```


Now we have a basic app that can display static content. Static content servers are actually a cornerstone of the modern internet, even though you would typically use Apache, Nginx, or other lightweight servers to serve simple html views. The only reason we're using a HTML document with embedded ruby (`.html.erb`) instead of plain HTML, is because Ruby is our templating language. Templating languages allow larger views to be constructed from partial views...and here we muddy the water of what a 'partial' view actually is. If you need a refresher on the concept of partials, check out the [terminology](#web-application-terminology) section.

#### Authentication

We will be setting up Omniauth with a 3rd party library. We *could* roll our own authentication, by creating a User model, controller, and view, including a login action.


#### Extra stuff

Clone should be released into the world as 'spacebook', a space-styled meme sharing site.
