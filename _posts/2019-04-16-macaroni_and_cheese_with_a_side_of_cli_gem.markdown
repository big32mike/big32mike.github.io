---
layout: post
title:      "Macaroni and Cheese with a side of CLI Gem"
date:       2019-04-16 05:33:27 -0400
permalink:  macaroni_and_cheese_with_a_side_of_cli_gem
---

Even if you don't find my [code](https://github.com/big32mike/cheezme) useful, you're at least thinking about mac and cheese. For that, you're welcome. For my CLI project I chose to scrape the [Macaroni and Cheese category](https://allrecipes.com/recipes/509/main-dish/pasta/macaroni-and-cheese/) of [Allrecipes.com](http://allrecipes.com). The code is pretty straight-forward. I'd like to get better at allocating my resources, so I'm going to focus on improving my approach to projects going forward. This post will read like a restrospective so that future me spends his time more wisely.
## Planning...
...is not the word I should use for what happened with this project. I traveled for much of the two week project window, and I slightly underestimated work requirements and simultaneously overestimated the complexity. I built a mental model of objects and relations before I'd even begun coding. I'm working on making this habit more of a bug, and less of a feature of my personality. I naively assumed that the mechanics of scraping would be the hardest part of the project, so when I wasn't procrastinating or trying to rub my pollen infested eyes right out of my skull, I focused on isolating way too much data for more objects and attributes than I ultimately needed.

## Getting started
I have a terrible habit of jumping into things without reading the instructions. Worse, most of the time when I do read the instructions it's more of a skimming. I've caught myself just jumping into fixing the test output on several labs, and over-complicating them. So much time wasted by impatience. I'm sure there's a proverb somewhere that embodies the profound irony of such haste.

I wish I'd watched [Avi's](https://www.youtube.com/watch?v=_lDExWIhYKI) [videos](https://www.youtube.com/watch?v=Y5X6NRQi0bU) all the way through before I started anything with this project. His idea to start "close to the user" and get the CLI working was the better approach. Instead, I had mac and cheese on the brain, so I immediately jumped into scraping the recipe index page, and individual recipe pages for attributes I could turn into objects. 

### Scraper.rb
I followed the OO-student-scraper lab paradigm and decided to do my scraping in its own class. It made sense to me to separate my scraping concern from my eventual objects. This class is just a hash factory--URLs come in, hashes go out. Objects are data and behavior, so my class methods here return hashes of data. This allows my object class(es) to be more generic.

There are a few places where I had to massage the scraped data to play nicely. The precision of the math that calculates the star ratings would make my high school Physics teacher proud. Two decimal places is plenty for us mortals. I originally replaced the preceding "By " characters from the cook's names, since that would make for a cleaner `Cook.name` attribute. In the end I only used Recipe objects, so the having the "By " in there was better for printing. When scraping the directions I would periodically get some unwanted text appended to each step for particular recipes. Once I noticed it during testing, I realized I needed to account for it. There was also a phantom item in the CSS element array that I had to pop off. One recipe was even missing the `li.prepTime__item` element from its recipe page, so I had to account for that.

### Recipe.rb
With the heavy lifting done in the Scraper class, my focus was on building actual objects. Knowing I had to go a level deep, I didn't want to duplicate a lot of effort here. I knew I'd need to scrape an index page to create objects, but also extend those objects with more attributes from a subsequent page. Once I could create an object from a hash, I knew adding attributes from an array of hashes would be trivial. The Music CLI and Student Scraping labs really set me up for success here. Mass assignment with the send method is one of my favorite bits of Ruby syntactic sugar. I made `create_from_collection` a class method since it's essentially a custom constructor. I didn't add `print_ingredients_and_directions` until I'd gotten the CLI close to complete. One more thing I didn't realize I needed since I dove right into objects.

### CLI.rb
As much as I wish I'd gotten the interface working first, it was good for me to struggle with it a bit at the end. I heard my parents utter something about always learning the hard way. This was a good real-world introduction to controller concept of the MVC pattern. It's a good foundation to build on as we get to more complex apps. It felt a little hacky to assign `Cheezme::Recipe.all` to `@@all`, but I wanted short hand. It didn't seem like a best practice. I weighed making it an instance variable for a sec, which would have required an `attr_accessor`. That felt like overkill. I also could have namespaced this better, to use `Recipe.all` or `Scraper.scrape_recipe_index`.

### Cheezme::CLI.menu
I took a break in the middle of writing this section to refactor this method. I started with a `case/when` statement, but needed to check for both string and integer values of input. I couldn't easily do that with a `case/when` so I decided to nest one in an else instead of using multiple `elsif` statements. Tradeoffs. I think the refactored code is more readable. I did install the [indent-guide-improved](https://atom.io/packages/indent-guide-improved) package to Atom to help with the less readable code, so it wasn't all bad.

### Bundler, RVM, and resolving dependencies
My mac has Ruby 2.6.0 and Bundler 2.0.1 installed. I ran into [this bug](https://github.com/bundler/bundler/issues/6937) when trying to run my CLI and get the console IRB session working. I have RVM installed, so I installed Ruby 2.4.5 and I was able to `bundle install` my way into bliss. I probably should have played around in the sandbox to avoid these issues, but the flexibility of developing locally trumps the browser or IDE experience. I appreciate having the learn IDE available, especially for beginners, but I'm glad I was able to work through this issue without too much pain.  I'm still not 100% sure about my gemspec, but my program works after moving some things around. I look forward to writing more gems.

### Refactoring, Features, and drawn conclusions
I could easily add a cook class, in fact I did start a `cook.rb`. A recipe has one cook, a cook has many recipes. I even considered writing a reviews class, where a recipe has many reviews. Reviews would get instantiated through recipes. Relating cooks to reviews is a rabbit hole that I don't want to start down. It would be nice to have some color in the output, and with a better eye for design, I'd arrange the data in a more aesthetically pleasing way. I also took this opportunity to sharpen my git foo. In my current role, we "develop" off the master branch. I used a feature branch workflow once my project was in a working state.

