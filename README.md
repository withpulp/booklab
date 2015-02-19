# Booklab.me <small>Book recommendations that make sense</small>

## Needs

* I'm tired of reading books that aren't relevant
* I'd like to easily organize books I want to read
* I want mentorship but I don't even know how to start

<!-- toc -->

* [MVP](#MVP)
  * [Subscriptions](#subscriptions)
  * [Book Wall](#book-wall)
  * [Reading List](#reading-list)
  * [Notifications](#notifications)
  * [Sharing](#sharing)
* [Technical Outline](#technical-outline)
  * [Collections](#collections)
* [How to use](#how-to-use)
  * [Requirements](#requirements)
  * [Installation](#installation)
  * [Deployments](#deployments)
  * [SEO and other concerns](#seo-and-other-concerns)
  * [Adding allow rules for external URLs](#adding-allow-rules-for-external-urls)
* [Structure](#structure)
  * [Packages used](#packages-used)
  * [Folder structure](#folder-structure)
* [License](#license)

<!-- toc stop -->

Demo here: [booklab.meteor.com](http://booklab.meteor.com).

## MVP

**Note:** This is still a work in progress.

### Subscriptions

```
When I'm looking for a new book  
I want to find books from people I look up to  
So that I can be sure the book is relevant to my interests
```

### Book Wall

```
When a recommendation is shared  
I would like it to include a title and author  
So I can do my own research on the book
```

```
When a recommendation is shared  
I want to see a short description from the person who recommended it  
So I can see whether it is something I'd like
```

```
When a recommendation is shared  
I would like to see the genre of the book and other books within that genre  
So I can see similar books that I may like
```

```
When a recommendation is shared  
I would like a link to Amazon or other retailers  
So I can gauge other reviews and easily make a purchasing decision
```

```
When a recommendation is shared  
I would like to see if others said anything about it  
So I can get a better picture of what it's like
```

### Reading List

```
When there are book recommendations I like  
I want to be able to add them a reading list  
So that I can have a list of books to read in the future
```

```
When there are books in my reading list  
I want to be able to sort them based on my interest  
So my reading list becomes meaningful
```

```
When there are books in my reading list  
I want to be able to sort them by rating, genre, recommender  
So that I can make more sense of the books
```

### Notifications

```
When a person I admire has posted a new book  
I want to be notified through email
So that I can review their recommendations easily
```

### Sharing

```
When I've read a book that's had tremendous impact on me  
I would like to share it with others  
So they can benefit as I did
```

## Technical Outline

### Collections

This is still a work in progress.

* Books - users can post their books
    * title
    * author
    * genre
    * description
    * amazon/barnes noble link
    * userId(recommender)
    * username(recommender)
    * date(now)
    * userRating
    * commentsCount(0)
* Relationships - users can subscribe to each other
    * userId(follower)
    * userId(leader/recommender)
* Bookmarks - reading list for liked books
    * pull data from Books
    * date(now)
* Comments - comments from users on books
* Notifications - notify when leader/recommender posted a new book (email and via app)

## How to use

### Requirements

Make sure [Meteor is installed and up to date](https://www.meteor.com/install) or run:

```
curl https://install.meteor.com/ | sh
```

### Installation

```
git clone git@github.com:amazingBastard/booklab.git
cd mtr-booklab
meteor
```

### Deployments

It is highly recommended to use [Meteor Up](https://github.com/arunoda/meteor-up) for easy deployments.
Have a look at the repository for more information.

### SEO and other concerns

> Meteor cannot do SEO

This statement is only partially true, since there is a package called [ms-seo](https://github.com/DerMambo/ms-seo), which
has a lot of neat little tricks to help web crawlers notice your app the way you want them to. Use constants under
__client/lib/constants.js__ for the app. Change SEO settings inside the routes like that.

```javascript
Router.route('/about', function () {
  this.render('about');
  // Using the app constants
  SEO.set({ title: 'About -' + Meteor.App.NAME, og: {...} });
});
```

### Adding allow rules for external URLs

The [browser-policy](https://atmospherejs.com/meteor/browser-policy) adds rules to deny all operations from external URLs.
This helps dealing with clickjacking and other XSS methods used to attack the client. To whitelist a url, add following to
__server/config/security.js__

```javascript
BrowserPolicy.content.allowOriginForAll(YOUR_URL);
```

Other security enforcing packages like [audit-argument-checks](https://docs.meteor.com/#/full/auditargumentchecks) and
[matteodem:easy-security](https://github.com/matteodem/meteor-easy-security) have also been added.

## Structure

### Packages used

* Meteor Core
  * [meteor-platform](https://github.com/meteor/meteor/tree/devel/packages/meteor-platform)
* Routing
  * [iron:router](https://github.com/EventedMind/iron-router)
  * [zimme:iron-router-active](https://github.com/zimme/meteor-iron-router-active)
* Collections
  * [aldeed:collection2](https://github.com/aldeed/meteor-collection2)
  * [dburles:collection-helpers](https://github.com/dburles/meteor-collection-helpers)
* Accounts
  * [accounts-password](https://github.com/meteor/meteor/tree/devel/packages/accounts-password)
  * [useraccounts:semantic-ui](https://github.com/meteor-useraccounts/semantic-ui)
* UI and UX
  * [fastclick](https://github.com/meteor/meteor/tree/devel/packages/fastclick)
  * [meteorhacks:fast-render](https://github.com/meteorhacks/fast-render)
  * [natestrauser:animate-css](https://github.com/nate-strauser/meteor-animate-css/)
  * [nooitaf:semantic-ui](https://github.com/nooitaf/meteor-semantic-ui)
* Security
  * [browser-policy](https://github.com/meteor/meteor/tree/devel/packages/browser-policy)
  * [audit-argument-checks](https://github.com/meteor/meteor/tree/devel/packages/audit-argument-checks)
  * [matteodem:easy-security](https://github.com/matteodem/meteor-easy-security)
* SEO
  * [manuelschoebel:ms-seo](https://github.com/DerMambo/ms-seo)
* Development
  * [less](https://github.com/meteor/meteor/tree/devel/packages/less)
  * [jquery](https://github.com/meteor/meteor/tree/devel/packages/jquery)
  * [underscore](https://github.com/meteor/meteor/tree/devel/packages/underscore)
  * [raix:handlebar-helpers](https://github.com/raix/Meteor-handlebar-helpers)

The "insecure" and "autopublish" packages are removed by default (they make your app vulnerable).

### Folder structure

```
client/ 				# Client folder
    compatibility/      # Libraries which create a global variable
    config/             # Configuration files (on the client)
	lib/                # Library files that get executed first
    startup/            # Javascript files on Meteor.startup()
    stylesheets         # LESS files
    modules/            # Meant for components, such as form and more(*)
	views/			    # Contains all views(*)
	    common/         # General purpose html templates
model/  				# Model files, for each Meteor.Collection(*)
private/                # Private files
public/                 # Public files
routes/                 # All routes(*)
server/					# Server folder
    fixtures/           # Meteor.Collection fixtures defined
    lib/                # Server side library folder
    publications/       # Collection publications(*)
    startup/            # On server startup
meteor-boilerplate		# Command line tool
```

(*) = the command line tool creates files in these folders

## License
This project has a limited MIT License, see the LICENSE.txt for more information.
