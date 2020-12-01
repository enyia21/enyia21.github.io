---
layout: post
title:      "Seeding Data from a CSV file to Rails API"
date:       2020-12-01 01:29:48 +0000
permalink:  seeding_data_from_a_csv_file_to_rails_api
---


Seeding data from a CSV file requires multiple steps to multiple steps to ensure that information is properly captured.  

1.  The first step is to create an ActiveRecord Model that will store CSV data.  After generating an active record model utilizing a `rails g scaffold <model name> <atribute_name: attribute:type>...` Make sure the model attributes are found within the csv file and note the title used to identify each attribute.   

2.  The second step involves adding `gem 'csv'` to the the gem file or your rails application.  Afterward you can run `bundle` or `bundle install` to add the gem.  Once the gem has been added to to you Gemfile you will be able to create the rake task that utilizes the gem.  
3.  After the gem installation, a folder should be created to store the csv that will be referenced.  A folder should be created in `lib`  directory called `csvs` that will house the csv file. 
4.  Next you want to create  a rake task using the command
                `rails generate task <command name> <pluralized model name>`
							
 This will create a rake file `\lib\tasks\<command name>.rake`  Open this file and edit between lines 4 and 5.  
             
               namespace :slurp do
                   desc "TODO"
                   task transactions: :environment do
                 end
								 
                end
5.  In the next task the user must require "csv" and read in the csv into text format using `File.read(Rails.root.join("lib", "csvs", <csv file>)`.  We can store the value in a <csv_variable> that we utilize later.  
6.  We can insure the csv is properly parsed by calling CSV.parse(<csv_variable>, :headers => true, :encoding => "ISO-8859-1" and assigning it to a <parsed_variable>.
7. We can loop arron the parsed variable utiling the code  
                     <parsed_variable>.each do |row|
										      v = <model>.new
													v.<attribute1> = row["<corresponding attribute1>"]
													v.<attribute1> = row["<corresponding attribute1>"]
													v.<attribute1> = row["<corresponding attribute1>"]
											                            .
										   														.
												 													.
													v.<attribute_final> = row["<corresponding attribute_final>"]
													v.save
												end

8.   Finally, we need this rake task to run when we seed data for the application.  To do this we run `Rake::Task["<command_name>:<pluralized_model_name>"].invoke`

Using this technique I was able to seed a large database of information into my rails application quickly using rails db:seed and a csv file.  I hope this helps.
				

