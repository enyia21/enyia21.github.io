---
layout: post
title:      "Voting App "
date:       2020-07-15 15:11:30 +0000
permalink:  voting_app
---


## Concept

### Goal
**The goal for the Voting App is to give citizens in every city access to voting.  They should be able to login and access their voting privelages from the internet.**

### Voters App Access
Every voter will have access to their information with the website.  They will be able to vote, create proposals and fill out ballots regarding the proposals made in their city.  When they submit a ballot with at least one proposal, every proposal will be tallied and a precentage approval will be available to the voter to view.  Voters will be able to modify their user information and their votes.  Voters will not be able to modify the ballots, information or proposals belongs to other voters. 

### Administration

Sinatra is the framework that will be utilized to run the application.  The MVC structure for the program will be include models for Voters, Ballots and Proposals.  The story will be each voter has one ballot and can make multilple proposals on that ballot.

In interacting with the information the essential relationships were revealed.  Voters has a only **one** ballot but **has many** proposal to vote on through the **one** ballot.  Ballots **belong to** one voter a piece and have many proposals on them.  Proposals appear on **many** ballots and **have many users** vote for them.  The many to many relationship between Ballots and Proposals forces the join table Ballot_Proposals to be created.

1. Voter <br>
    ```class Voter < ActiveRecord::Base
    has_one :ballot
    has_many :proposals, through: :ballot ... end```
2. Proposal<br>
    ```... has_many :ballot_proposals
      has_many :ballots, through: :ballot_proposals
      has_many :voters, through: :ballots ... ```
			
3. Ballot<br>
   ``` ... belongs_to :voter
     has_many :ballot_proposals
     has_many :proposals, through: :ballot_proposals  ...```
4. Ballot_Proposals<br>
    ```class BallotProposal < ActiveRecord::Base
    belongs_to :ballot
    belongs_to :proposal
    end```

The Databases were built utilizing the `rake db:create_migration "..."` code.  Mutilple migrations were utilized to create the tables for Voters, Ballots, Ballot_Proposals, and Proposals.  The final schema being:
```
ActiveRecord::Schema.define(version: 2020_07_12_200426) do

  create_table "ballot_proposals", force: :cascade do |t|
    t.integer "ballot_id"
    t.integer "proposal_id"
  end

  create_table "ballots", force: :cascade do |t|
    t.string "city"
    t.string "state"
    t.integer "voter_id"
  end

  create_table "proposals", force: :cascade do |t|
    t.string "name"
    t.string "details"
    t.boolean "approve?"
  end

  create_table "voters", force: :cascade do |t|
    t.string "user_name"
    t.string "password_digest"
    t.string "first_name"
    t.string "last_name"
    t.string "email"
    t.string "drivers_license"
    t.string "street_address"
    t.string "city"
    t.string "state"
    t.string "zipcode"
  end
end
```

Controllers and views and models interact with each other to allow voters to interact with the database.  In order to allow for individual control and clearer development each route was created individually.  In order to achieve this we modified the config.ru file to add multiple controllers.  

```
require './config/environment'

begin 
    # check_migration

    use Rack::MethodOverride
    use BallotController
    use ProposalController
    use VoterController
    run ApplicationController

# rescue ActiveRecord::PendingMigrationError => err
#     STDERR.puts err
#     exit 1
end
```

One one application controller is run.  Each of the other controllers requires use instead. `use Rack::MethodOverride` allows for middleware to be used.   The patch and delete routes require Sinatra middleware to work.  

Validations are vital to ensuring a database has the correct information in it.  There are two primary types of validations that I'm utilizing are database and controller level.  Database contraints and Controller level validations are typically used to verify if information can be added to active record.  There are pro and cons to the utiliztion of each tool.  

I use database level validations to insure that unique voters are created based on their drivers licences, user names and email.

```
    validates :email, uniqueness: true
    validates :user_name, uniqueness: true
    validates :drivers_license, uniqueness: true
```

Future iterations will send conformations to users that their ballots have been received.  
Utilizing database contraints involves creating validations that checks contraints before information is commited to active record. 

For a more robust experience database validations can be combined with controller level validations.  Controller level validation are not adequate when utilized on their own, however they are great to pair with other types of validations.  In order to ensure users properly logged I created routes to a failure page when users entered invalid information.  
Ex.  
```
           # verify drivers_license is unique
           if Voter.all.find{|voter| voter.drivers_license == params["drivers_license"]}
                session[:error_msg] = "Your drivers license numbers is taken!"
                redirect '/voters/failure'
           end

           # verify user_name is unique

           if Voter.find_by(user_name: params["user_name"])
                session[:error_msg] = "The username you requested is taken"
                redirect '/voters/failure'
           end
        end

        #check whether all fields were input correctly
        @new_voter = Voter.new(params)
        if !@new_voter.is_valid?
            session[:error_msg] = "Your signup is incomplete"
            redirect '/voters/failure'
        end

```

### Conclusion

**The creation of the voting app allowed me to create an app that had multiple active record associations.   I created an app that was responsive to users and secure on muliple levels.**
