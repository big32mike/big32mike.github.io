---
layout: post
title:      "Macaroni and Cheese with a side of CLI Gem"
date:       2019-04-16 05:33:27 -0400
permalink:  macaroni_and_cheese_with_a_side_of_cli_gem
---

### Introduction
Macaroni and Cheese is a staple of family gatherings, and a go-to comfort food for me. As my homage to fromage, I built a CLI scraper of the [Macaroni and Cheese category](https://allrecipes.com/recipes/509/main-dish/pasta/macaroni-and-cheese/) at [Allrecipes.com](http://allrecipes.com). The planning and time management aspects of the project were more challenging than writing the code. This is my restrospective.

### Planning...
...was not a significant part of my process.  While traveling, I fell behind by underestimating the project scope. I then overestimated the object complexity requirements before I had a working interface. I also overestimated the portion of the project focused on scraping mechanics, and I prioritized that effort. Next time build the house before furnishing it, using this post as a blueprint.

### Getting started
When I finally got around to the interface, I realized [Avi's](https://www.youtube.com/watch?v=_lDExWIhYKI) [videos](https://www.youtube.com/watch?v=Y5X6NRQi0bU) were a gold mind. I was unsure how Bundler worked, but I got my interface stubbed out enough to keep up the progress. As a Linux admin, I take for granted how second nature file permissions and shell interpreters are to me, and how unwelcoming they can be to beginners. The ability to break down daunting tasks to discrete manageable pieces is as important as writing an algorithm. It's its own algorithm.

### [CLI.rb](https://github.com/big32mike/cheezme/blob/master/lib/cheezme/cli.rb)
I missed the subtle influence of the directory structure of the project on the namespace until the end of the project. Avi touched on it briefly in the video, but I managed to gloss it over.  I made a class variable `@@all` because I got tired of typing `Cheezme::Recipe`. Lesson learned. `@@url` made more sense than hard coding the site in the scraping method. It's a class variable to allow methods direct access.

### [Scraper.rb](https://github.com/big32mike/cheezme/blob/master/lib/cheezme/scraper.rb)
We don't need to create any scraper objects, but it made sense to separate our scraping from our object factory. We learned how to turn hashes into objects programatically, so that seemed like a good delimiter.  There is a recipe missing some metadata on its page, so I had to account for those edge cases here. There are also a few recipes whose url doesn't scrape fully the first time or two. Retries are eventually successful.

### [Recipe.rb](https://github.com/big32mike/cheezme/blob/master/lib/cheezme/recipe.rb)
The Object Relation labs prepared me for quickly creating objects from collections. It was trivial to extend those objects with additional attributes after the Mass Assignment lab. The color assignment would be cleaner if refactored into its own method, called from the constructor. I tried using the rainbow refinements, but bundler wasn't able to install it. I could have used colorized, but I wanted the challenge of figuring out a new gem.

### Cheezme::CLI.menu
I literally refactored this method in the middle of writing this paragraph. I wanted to avoid multiple `elsif` by using a `case/when`. I was thwarted when I needed to test for inclusion in the range of integers, but also act on the string values. The retry logic was added to coerce scraping of the wonky recipe pages. The final product is much more readable.

### Bundler, RVM, and resolving dependencies
I've been using a local development environment for this course without issue, but I did run into [a problem with my version of bundler](https://github.com/bundler/bundler/issues/6937) . I'm running Ruby 2.6.0 locally, and the fix isn't coming until 2.6.2. I ran `rvm install 2.4.5` to install an older version. To utilize a gem we need its dependencies (.gemspec file), retrieve and install them (`bundle install`), and make them available to our run time environment (`require`).

### Refactoring, Features, and drawn conclusions
I'm still scraping the cook profile url  because I toyed with the ideas of different object classes relating to each other, but realized that's outside the scope of this project. We can easily extend to cook and review objects.  I took this refactoring opportunity to properly learn hwo to use feature branches in git. My team all develops off master, and every once in a while one of us gets our checked out repo into such a bad place that we'd delete and re-clone it. I read up on [feature branch workflows](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow) and the [difference between merging and rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing) when combining changes. Finally, I refactored this blog post in an attempt to make it more succinct. Brevity being the soul of wit and all. While I'm more comfortable writing code than prose, I'm equally enthused to hone this skill.
