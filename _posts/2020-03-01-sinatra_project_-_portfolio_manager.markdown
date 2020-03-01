---
layout: post
title:      "Sinatra Project - Portfolio Manager"
date:       2020-03-01 08:47:30 +0000
permalink:  sinatra_project_-_portfolio_manager
---

To demonstrate my understanding of MVC, CRUD, and user management, I created this a simple portfolio manager application. A User `has_many` portfolios and `has_many` stocks through portfolios. Portfolios `belongs_to` a user and stocks `belongs_to` a portfolio. A User `has_secure_password` and `validates_uniqueness_of` a username. A User can sign up, login, and logout as well as complete CRUD actions on portfolios and stocks.

### User Authentication
ActiveRecord's `has_secure_password` macro allows us to leverage the `authenticate` method to compare a user's password input with the `password_digest` stored in the database. This digest is salted and encrypted, so we aren't storing user passwords in plaintext. ActiveRecord also prevents us from saving multiple users with the same username to the database via the `validates_uniqueness_of` macro. I still had to validate user input in the controller logic, but ActiveRecord fails silently in the background in case we forget.

### CRUD Actions
Once a user is authenticated, they can create portfolios with just a portfolio name. We can create new stocks on the portfolio show and edit pages. On the portfolio edit page, we can also rename the portfolio and delete stocks. Stocks can be deleted and updated from the stocks edit page as well. Portfolio owners must be logged in to complete the CRUD actions, and they can only act on stocks and portfolios that belong to them. No peeking at other users' portfolios or stocks either..

### Takeaways
* Whenever I see `has_many` and `belongs_to` relationships, I immediately know that the belonging object database table will need a column for the id of the owning object. The shovel operator manages this relationship in the database automatically.
* Whenever I see the `has_secure_password` macro, I immediately know that the  object's database will need a `password_digest` column.
* When managing users, we need to enable sessions and set a session secret in the `application_controller` to access the `session` hash.
* When using multiple controllers, we must remember to mount them in `config.ru`.
* The `patch` and `delete` methods require `Rack::MethodOverride` to function properly.
* `Sinatra::Flash` is slightly easier to use than `Rack::Flash`
* Defining `helpers` in `application_controller.rb` helps us adhere to the DRY principle
