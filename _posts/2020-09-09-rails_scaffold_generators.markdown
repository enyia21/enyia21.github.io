---
layout: post
title:      "Rails Scaffold Generators"
date:       2020-09-10 00:03:42 +0000
permalink:  rails_scaffold_generators
---

Rails generators allow for the quick building of websites by abstracting away development specifics to allow users to focus on the creation of websites.  Each generator allows a user with a variety of tools and testing procedures that allow for a user to efficiently build a site.  The most comprehensive generator is the scaffold::

Scaffold generates an entire resource which includes: 
Models
Migration
Controller 
Views



1.  `rails generate scaffold NAME [field[:type][:index] field[:type][:index]] [options]` 

NAME  - Refers to the model name
FIELD 
    :TYPE - Refers to the type of attribute that your passing in
		:INDEX - can be unique or non-unique if based on your needs 
		
An example would be:
```
$ rails generate scaffold HighScore game:string score:integer
    invoke  active_record
    create    db/migrate/20190416145729_create_high_scores.rb
    create    app/models/high_score.rb
    invoke    test_unit
    create      test/models/high_score_test.rb
    create      test/fixtures/high_scores.yml
    invoke  resource_route
     route    resources :high_scores
    invoke  scaffold_controller
    create    app/controllers/high_scores_controller.rb
    invoke    erb
    create      app/views/high_scores
    create      app/views/high_scores/index.html.erb
    create      app/views/high_scores/edit.html.erb
    create      app/views/high_scores/show.html.erb
    create      app/views/high_scores/new.html.erb
    create      app/views/high_scores/_form.html.erb
    invoke    test_unit
    create      test/controllers/high_scores_controller_test.rb
    create      test/system/high_scores_test.rb
    invoke    helper
    create      app/helpers/high_scores_helper.rb
    invoke      test_unit
    invoke    jbuilder
    create      app/views/high_scores/index.json.jbuilder
    create      app/views/high_scores/show.json.jbuilder
    create      app/views/high_scores/_high_score.json.jbuilder
    invoke  assets
    invoke    scss
    create      app/assets/stylesheets/high_scores.scss
    invoke  scss
    create    app/assets/stylesheets/scaffolds.scss
```

In this example the Model - HighScore was generated with two attributes.   One of the attributes was` game:string` which is an attribute GAME of type STRING.  These attributes are used to generate a migration table for active record.  Along with that migration table will be a mode that will have those attributes with it.   A controller will be created allow with views and forms will be generated following with restful principles.   This means there will be ReStful routes generated including: 
1. new
2. create
3. update
4. update
4. destroy
5. show 
6. edit 
7. index
These routes will be wiritten with their corresponding views created: 
1. index.html.erb
2. new.html.erb
3. show.html.erb
4. edit.html.erb
Routes are created by the scaffold generator.  The routes are written by the generator for each model that is scaffolded.  Finally json builders are added via scaffolding.  Allong with the generated infrastructure, there are test suites built to aid users in verifying that their models are fuctioning properly.  Scaffolding allows with the quick generation of restful rails apps.  



