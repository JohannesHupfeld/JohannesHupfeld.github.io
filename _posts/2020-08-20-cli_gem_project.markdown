---
layout: post
title:      "CLI Gem Project"
date:       2020-08-20 20:45:45 +0000
permalink:  cli_gem_project
---



Where to start?
	It surprisingly took me quite a while to decide what I wanted my project to be about. I searched the Internet and picked my brain for days trying to come up with something I would like and others would find interesting. In the end I decided to combine my love for horror movies and and bingeing on Netflix and made an app that would scrape the top 50 horror movies on Netflix according to Rotten Tomatoes. The app scrapes the most important information about each movie as well as the top critic reviews. I actually tried a few different websites before landing on this one because once I made it to actually scraping the website I would run into numerous issues and realize the site wasn’t exactly a friendly scraping site. So note to anyone in the beginning stages of this project is to definitely make sure the site is easy to scrape.
	
The Process
	The first step I did was add the required gems and require_relatives of each class to the environment folder inside the project folder inside the lib. I then created a file called “run” in the bin folder. In the run file I first copied and pasted the shebang line (#!/usr/bin/env ruby) from the console file within the bin folder. I then added a require relative to the environment file (require_relative ‘../lib/environment.rb’). The last thing in the run file is a line of code (NetflixHorror::CLI.new.start) that calls the start method in the CLI class which intern runs the program.
	I created four different classes in separate files. The first class I created was the CLI class. In naming the class I choose to name space it as NetflixHorror::CLI as to not create confusion if someone were to use my gem. I then created the other classes and also name spaced, (class NetflixHorror::Scraper, class NetflixHorror::Movie, class NetflixHorror::Review). Within the the CLI class I made a method called “start”. This is where the program goes through the scraper class and all other methods to actually run the program. The start method is the main flow of the CLI that calls on different actions (methods) to execute its command at certain times.
	My next focus was the scraper class. Here I created two scape methods, one for each level deep. The first one I called (def self.scrape_movies) and the second I called (def self.scrape_reviews(movie_object)). The first thing in the scrape movies method Nokogiri link to the site im scraping. We then have an array of movies equal to my html code that contains all the .divs of my movie attributes. We then find each of the specific html code per each attribute while inspecting the code of the site we are scraping and set the hash equal to attributes. To finish off this method we call the movie class and pass in the attributes and set it equal to the variable called movie (movie = NetflixHorror::Movie.new(attributes)). The same is pretty much done with the scrape review method only using a different html code for the information found on the review pages of each movie. 
	


	In the movie class we set all the attributes we are scraping to an attr_accessor and below that a class constant all (@@all = [ ]) I then made an initialize method (def initialize(att_hash)) to receive the hash created in the scrape movies method by using the send method 
			(att_hash.each do |key, value|
				self.send(“#{key}=”, value)).
 This Iterates through the hash and then for each key and value it calls the method self to whatever our key is and then assigns it to a value. The next method in the movie class is the save method which saves self to all and by doing that we add a self.save to the initialize method. I also return self in the save method so we don’t accidentally return everything later on. Next I created a method called self.all to retrieve the data. The last method in this class is the add_review method. 
	For the review class I decided to implement object relationship in which we make a movie have many reviews. To do this we have to add an empty array to the movie class that I put in the initialize method as (@reviews = [ ]). Also required is an attr_reader set to :reviews. The next step is to go to the review class and create an attr_accessor with attributes about each review. The last thing is to create an empty initialize method. 
 
