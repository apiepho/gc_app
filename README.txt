
January 2017

The gc_app is ruby based web crawler of the GameChanger website. (This does require a
valid subscription witch I highly encourage an recommend).

I was looking for a project to learn more about web crawling and combine that with my
interest in following youth/prep baseball scores and stats.  The GameChanger web based
service is a great tool to following your teams with more and more teams using the this
to track their games.  The data is very useful for coaches and the interested parent.

Unfortunately, GC some limitations that I hope the gc_app will let me address.  The gc_app
has two main goals:
   1) list individual games in oldest-first (rather than newest-first) order
   2) Calculate specific player stats, and consolidate


Background
----------

As a moderately experienced Ruby programer, I started with the goal of writing a Ruby
application to gather the GC data.  This lead to some interesting investigations:

Git - Obviously, git is used for source code control, and specifically the GitHub service.

Homebrew - 
I seems like most Ruby development is done on Linux systems, but I work on a
Mac.  The Homebrew tool is a great package manager for installing Linux tools on a Mac.
The first use was install rvm, Ruby Version Manager.

Meld - 
The first tool installed for me is always meld.  This is an open source tool for 
visual comparison of files and directories.  This is my goto tool for inspecting code
changes, and it directly integrates with git (use 'meld .' to see/edit pending changes.

Ruby Revision Manager - 
The previous work with Ruby I have done was in a closed environment, 
with a single version of Ruby.  It was always difficult when the version of Ruby changed.  
The Ruby Version manager allows changing versions easily. With operations like:
   rvm geset list
   rvm gemset use ...

Ruby on Rails - 
While looking for how to install Ruby and gems, I stumbled upon several
references to Ruby on Rails.  I read thru these references, installed it and played 
around a bit.  A future project will use Ruby on Rails for building a website.

Mechanize Ruby Gem -
One of the first online tutorials I found suggested using this Ruby gem to navigate
web pages automatically.  This gem does a good job of setting up and navigation of html
pages of a website.  While it is nice because it is fast and doesn't require a browser
to open, it lacks support for client-side javascript generated content.  Ultimately, I 
did not use this gem.

Nokogiri Ruby Gem -
This gem seems to be the Ruby standard for parsing html data.  It
does NOT do navigation, but provides methods to parse the html text from the page.

Selenium -
This is an open source framework for automated web testing.  A Selenium system typically
consists of a web browser plugin, an API to that plugin, and a software driver that
user applications can use to control/inspect the contents of the page on the browser.
A key aspect of testing this way, is the test sees what an end user would see (ie. after
any client-side javascript manipulates the web page).

Watir Ruby Gem -
This gem is really an extension of the open source Selenium framework.  This gem was
used for the project since it can access the final html the user sees.

Xpath -
This is way to specify html search strings similar to regex (regular expressions)
for string searching.

Scrapy - 
This is actually a perl based tool and infrastructure for web crawling.  It's
main advantage is speed...much faster than Watir.  It is also differs in a couple
structural ways:
	- It is really a web crawling tool that uses config files describing 'spiders' and
	how those spiders should navigate pages, typically in parallel.
	- Unlike Watir, a library used to navigate, Scrapy requires building code that
	runs within the scrapy framework

Splash -
Similar to the problem with the Mechanize gem, scrapy by itself cannot parse html changed
by local client-side javascript.  Splash is the solution to that.  I plan on followup
project to further understand Scrapy/Splash.


Application Structure
---------------------
(started some notes...need to finish)

There are comments in the main app files, gc_app.rb, describing the structure on how 
the parsed data is constructed.  That structure follows the GC site.  The following
describes the various files:

gc_app.rb                - the main application and command line parsing
gc_common.rb             - constants, classes, and global variables common to the app
gc_browser.rb            - a wrapper around Watir and page caching

gc_teams.rb              - gets and starts parsing "manage my teams" page (takes advantage of JSON on page)
gc_team.rb               - gets given team link and starts parsing of roster and games

gc_roster.rb             - gets and parses roster page and builds players
gc_player.rb             - from player data, builds stats  (takes advantage of JSON on roster page)
gc_stats_base.rb         - a class that can parse and display xml for several GC pages
gc_stats_spray_chart.rb  - a class that parses and displays any spray data (table differs from other stats pages)

gc_games.rb              - top level class for all games of a given team
gc_game.rb               - builds lots of information about the team from the uri, the uri page html,
                           and JSON from the page. Builds the boxscore data and starts parsing innings.
                           This also pre-parses innings to reverse the order to true chronological.
gc_boxscore.rb           - parses the boxscore page and gathers game roster and lineups from JSON
                           on that page.
gc_innings.rb            - parses all innings, doing some more re-ordering, and builds top/bottom
                           half innings
gc_inning_half.rb        - parses all the plate appearances, dealing with first table row being the half
                           inning description.
gc_appearences.rb        - builds all the appearances
gc_appearence.rb         - builds data about the plate appearance, like balls and strikes.

(As noted above, there are a cases where embbedded JSON data is used instead of true 
web crawling/parsing.  This would not be a good solution if the goal was to build an automated
test structure, but it makes things more efficient for the purposes of the app).

Other files:
ruby_prepend.rb          - a simple tool to add copyrights to the front of all files.
ruby_strip.rb            - a simple tool to strip all comments (to see actual code w/o comments)
ruby_tabs.rb             - a tool to replace any tabs with spaces and remove trailing spaces


Status as of 1-24-2017
----------------------
This is version 0.1.  The first pass of parsing all data is in place and can generate a single large
xml file with all data parsed.  It also supports page caching for faster operation.

The next version will start the generation of custom stats.





