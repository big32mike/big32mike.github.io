---
layout: post
title:      "Macaroni and Cheese with a side of CLI Gem"
date:       2019-04-16 05:33:27 -0400
permalink:  macaroni_and_cheese_with_a_side_of_cli_gem
---

Even if you don't find my [code](https://github.com/big32mike/cheezme) useful, you're at least thinking about mac and cheese. For that, you're welcome. For this project I chose to scrape the [Macaroni and Cheese category](https://allrecipes.com/recipes/509/main-dish/pasta/macaroni-and-cheese/) of [Allrecipes.com](http://allrecipes.com). The code wasn't as challenging as the project planning and time management. This post will help me avoid future pitfalls.
### Planning...
...is not the word I should use for what happened with this project.  Traveling and underestimating the amount of work put me behind. I designed a complex object model before having a working interface. I overestimated the importance of the scraping mechanics, and front loaded that work. Next time I'll build the house before furnishing it. I'll use this post as my blueprint.

### Getting started
When I finally got around to the interface, I realized [Avi's](https://www.youtube.com/watch?v=_lDExWIhYKI) [videos](https://www.youtube.com/watch?v=Y5X6NRQi0bU) were a gold mind. I wasn't exactly sure what Bundler was doing, but I got my interface stubbed out enough to keep up progress.

### [Scraper.rb](https://github.com/big32mike/cheezme/blob/master/lib/cheezme/scraper.rb)
We don't need to create any scraper objects, but it made sense to separate our scraping from our object factory. We learned how to turn hashes into objects programatically, so that seemed like a good delimiter.  There is a recipe missing the cook and prep time data on its page, so I had to account for those edge cases here. There are also a few recipes whose url doesn't scrape fully the first time or two. Retries are eventually successful.

### [Recipe.rb](https://github.com/big32mike/cheezme/blob/master/lib/cheezme/recipe.rb)
The Object Relation labs prepared me for quickly creating objects from collections. It was trivial to extend those objects with more attributes after the Mass Assignment lab. The color assignment would be cleaner if refactored into its own method, called from the constructor. I tried using the rainbow refinements, but bundler wasn't finding it. I could have used colorized, but I saw others using it so I wanted the challenge of figuring out a different gem.

### [CLI.rb](https://github.com/big32mike/cheezme/blob/master/lib/cheezme/cli.rb)
I missed the subtle influence of the directory structure of the project on the namespace until the end. Avi touched on it briefly in the video, but it was easy to gloss over.  I used @@all because I got tired of typing `Cheezme::Recipe`. Lesson learned. @@url made more sense than hard coding the url in the scraping method. I wanted to access it directly without passing an argument, hence the class variable.

### Cheezme::CLI.menu
I literally refactored this method in the middle of writing this paragraph. I wanted to avoid multiple `elsif` by using a `case/when`. I was thwarted when I needed to test for inclusion in the range of integers, but also act on the string values. The retry logic was added to coerce the finickey recipe pages. 

### Bundler, RVM, and resolving dependencies
I've been using local dev the whole course with no issues, but I did run into [this bundler issue](https://github.com/bundler/bundler/issues/6937) on this lab. I'm running Ruby 2.6.0, and the fix isn't coming until 2.6.2, so I used `rvm` to install an older version. To utilize a gem we need its dependencies (.gemspec file), retrieve and install them (`bundle install`), and make them available to our run time environment (`require`).

### Refactoring, Features, and drawn conclusions
I'm still scraping the cook profile url  because I toyed with the ideas of different object classes relating to each other, but realized that's outside the scope of this project. We can easily extend to cook and review objects.  I took this refactoring opportunity to properly learn hwo to use feature branches in git. My team all develops off master, and every once in a while one of us gets our checked out repo into such a bad place that we'd delete and re-clone it. I read up on [feature branch workflows](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) and the [difference between merging and rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) when combining changes. Finally, I refactored this blog post in an attempt to make it more succinct. Brevity being the soul of wit and all. While I'm more comfortable writing code than prose, I'm equally enthused to hone this skill.
