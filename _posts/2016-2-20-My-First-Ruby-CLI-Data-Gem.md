---
layout: post
title: My First Ruby CLI Data Gem
---

Having been enrolled in the online full stack development course from Flatiron School for a while,  it is time for me to build my first [Ruby CLI Data Gem] (http://github/marcotsui2003). This locate-urgent-care Ruby Gem is a command line application that scraps online information such as phone no., address, directions and opening hours of open urgent care clinics that are close to the zip codes provided by the users. 

The file structure of gem follows the guidelines in [RubyGems](http://guides.rubygems.org/make-your-own-gem/), and the objects models are based on what I learned in the OO Ruby classes from Learn :


{% highlight bash %}
.
├── bin
│   ├── console
│   ├── locate-urgent-care
│   └── setup
├── CODE_OF_CONDUCT.md
├── config
│   └── environment.rb
├── Gemfile
├── Gemfile.lock
├── how_to_install_phantomjs.txt
├── lib
│   ├── locate_urgent_care
│   │   ├── clinic.rb
│   │   ├── command_line_interface.rb
│   │   ├── command_line_interface.rb~
│   │   ├── scraper.rb
│   │   ├── scraper.rb~
│   │   └── version.rb
│   ├── locate_urgent_care.rb
│   └── locate_urgent_care.rb~
├── LICENSE.txt
├── locate_urgent_care-0.1.0.gem
├── locate_urgent_care.gemspec
├── Rakefile
├── README.md
└── test.txt

{% endhighlight%} 
First I tried to use Nokogiri to scrap the webite but failed to get any search results. It turns out this is a Javascript-rendered website so I used a headless driver poltegeist/phantomjs with capybara. To do this you need to install {% ihighlight html %}  phantomjs version  {% endihighlight %} and install the gem {% ihighlight html %}  capybara {% endihighlight %} and gem {% ihighlight html %}  poltergeist {% endihighlight %}.

To use Cabypara add this line to the Scraper class:
{% highlight ruby %}
class LocateUrgentCare::Scraper
  extend Capybara::DSL
....
{% endhighlight%} 

and these to the config/environment.rb:
{% highlight ruby %}
Capybara.default_driver = :poltergeist
Capybara.run_server = false

Capybara.register_driver :poltergeist do |app|
  Capybara::Poltergeist::Driver.new(app, 
  js_error: false,
  debug: false, :default_wait_time => 30, :timeout => 90 )
end
....
{% endhighlight%} 

Now the scraper works as what it is supposed to! Below is a demonstration of how to use this scraper as a commmand line application:

{% youtube vdsKtQbYPkc %}




This is more or less how I finished my first CLI project. It was tough, but also a lot of fun and I learned a ton from it! 




