---
layout: post
title:      "NFL CLI Build "
date:       2020-04-17 13:09:03 -0400
permalink:  why_did_you_decide_to_study_software_engineering
---

## Purpose

My cli build  was designed to allow users to effieciently pull up nfl roster and schedules quickly .  The app works by utilizing a series of questions to determine what the user is interestad in.  The app then scrapes the sports site for the information that we are seeking and returns that information to the user.   

This app finds its utility in that it allows users to quickly access information without having to sift through a website to do so.  Many nfl fans want access to the information about there favorite teams and when they will play.  This app should allow users to retrieve that information faster.


### 1.  Installed gem

In order to build our gem I used the bundle gem tool.  

*bundle gem nfl_lookup_tool*     

**Dont forget the _.  If you name your gem with - instead your folders will be nested **

1. builds gem
2.  builds git repository


### 2. Add to github
1. go to git hub
2. create repsository and push existing repository from command line using 

*git remote add origin https://github.com/enyia21
/nfl_lookup_tool.git*

*git push -u origin master*

### 3. Added required files to the environment file.  
In order to ensure that all of the files are aware of eachother require_relative commands must be added to the environment file (nfl_lookup_tool.rb)

![]https://drive.google.com/file/d/18Y7CH4l8kwtAkWC4VmVqD9HJOeZGD0xP/view?usp=sharing

### 4. Added code to the executible file

By adding the code:

*NflLookupTool::CLI.new.call*


the code is not executable for the command line.  

###  5.  CLI 


After I inserted code into the executile file to all on the CLI, I transitioned to building out the CLI.   I utiiized pseudocode to mock up the behavior of my application.  After I figureed out the behavior i wanted to create I began to build out an gem with mutiple classes to execute it.   

    *1. CLI - Interacts with users.  It takes in user input and returns Nfl rosters, schedules, or teams*
		
		
### Classes
I utilized class to store scraped information so that it'd be available to users 

		*2.  Teams - stores every team instance it interacts directly with the Cli
		3.  Schedule - returns a list of team gamses 
		4.  Scrapper  - scrapes web address twice for information regarding the team  *
		5.  

