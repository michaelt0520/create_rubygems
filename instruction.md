[Presentation slides](http://slid.es/jendiamond/createa_gem)

# Create a Gem - Make it a CLI - Add Rspec Tests 

Create a Gem - Make it a Command Line Interface - Add Rspec Tests Using [Bundler](http://bundler.io/v1.3/rubygems.html) & [Thor](http://whatisthor.com/)

#Creating your own Gem

1. Run this command in your Terminal. This creates and names all the files you need for your gem. We are going to create a Lorem Ipsum Generator; you can call it whatever you want but seeing as we are creating a [Lorem Ipsum](http://en.wikipedia.org/wiki/Lorem_ipsum) generator we'll call it lorem. [Read about gem naming conventions.](http://guides.rubygems.org/patterns/) 
    
        $ bundle gem lorem
    
    Notice that this command also initialized a git repository for you on your local machine and gives you the path name for it.  
        
2. cd into lorem. Use `$ ls` to list your files that were just generated. 
        
        Gemfile
        Rakefile
        LICENSE.txt
        README.md
        .gitignore
        lorem.gemspec
        lib/lorem.rb
        lib/lorem/version.rb


3. Make your `lorem.gemspec` file look like this. Add your name and your email. 

>>"In order to create a gem, you need to define a gem specification, commonly
called a `gemspec.`  
A gemspec consists of several attributes. Some of these are required;
most of them are optional. The main body of this document is an alphabetical
list of gemspec attributes, each with a description, example usage, notes, and
more." [Read more about the gemspec file.](http://docs.rubygems.org/read/chapter/20) 

        \# coding: utf-8
        lib = File.expand_path('../lib', __FILE__)
        $LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
        require 'lorem/version'
    
        Gem::Specification.new do |spec|
            spec.name          = "lorem"
            spec.version       = Lorem::VERSION
            spec.platform      = Gem::Platform::RUBY
            spec.authors       = ["your name"]
            spec.email         = ["your@email.com"]
            spec.homepage      = ""
            spec.summary       = %q{Jenipsum generator}
            spec.description   = %q{Generates jenipsum text}
            spec.license       = "MIT"

            spec.add_development_dependency "bundler", "~> 1.3"
            spec.add_development_dependency "rake"

            spec.files         = `git ls-files`.split($/)
            spec.test_files    = spec.files.grep(%r{^(test|spec|features)/})
            spec.executables   = spec.files.grep(%r{^bin/}) { |f| File.basename(f) }
            spec.require_paths = ["lib"]

        end

  If you neglect to change these areas you will get this error when you go to run $ gem build lipsum.gemspec 

        ERROR:  While executing gem ... (Gem::InvalidSpecificationException)  
        "FIXME" or "TODO" is not a description


3. Open your file `lib/lorem.rb` Make it look like this. This is the file that is loaded when people require your gem. You can customize it with the behavior you want it to have.

        require "lorem/version"
        
        module Lorem
            def self.ipsum
                "Fantastic, yes, but gory. Help. It is a juniper. Pikku and Friendly ate all the kibbles. Formaldehyde, an exacto, a comb, a napkin, and a typewriter combined with vitamins, a bicycle, whiskey, batteries, flippers and a bike messenger twisted around the bend one curve at a time during the entire tour through Texas with Madi and my bass. Veragogo had a plan for fame and flicker of flash bulbs but all the costumes turned into jerseys and centuries with views of flowers, yucca, jelly, blueberries and guava paste. My teeth broke in an accident with the turbo trainwreck I call myself in my twenties. Floppy hair, green eyes and a slick bike are the formula in geometry for a Pickaxe turned Park n' Ride. The stripped cats, Lars, pompoms, sneakers, bubbler and litter boxes all make California seem like home, Massachusetts. Finland is stress-free where you can relax, swim and bike. Do a triathlon in Palm Springs or Oxnard in November or early October while listening to Vanity Press and Go Away Evil. Dudley is a great place to visit in the winter at Christmas."
            end
        end

4. Run this command.

        $ gem build lorem.gemspec
        
### Congratulations, you have created a gem. 

#### [Now you can add Thor.](http://rgsocbundler.github.io/cheatsheets/adding-cli-to-a-gem-using-thor.html)   

**Here are some good resources if you have questions.**  
[Video tutorial that will explain all the files and file structure.](http://railscasts.com/episodes/245-new-gem-with-bundler)  
[This is Bundler's Tutorial](http://bundler.io/v1.3/rubygems.html)  
[RubyGems Guides](http://guides.rubygems.org/make-your-own-gem/)

--------------------------------------------------------------

# Adding CLI to Your Gem using Thor
Our example gem is called lorem. Replace lorem with your gem directory name accordingly.

1. Go into your Gem directory

        $ cd lorem

2. Create a cli.rb file in `lib/lorem` In this file create a class that inherits from Thor and your lorem file. Add all of this code.

        require "thor"
        require "lorem"

        module Lorem
          class CLI < Thor

            desc "ipsum", "Lorem Ipsum text generator"
              def ipsum
                puts Lorem.ipsum
            end
          end
         end

    Read more about this on the Thor website [http://whatisthor.com/](http://whatisthor.com/)

3. Create bin folder

        $ mkdir bin

4. Create executable file naming it what you want your gem command to be. Do not add a .extention to it. Add the following code into your `bin/lorem` file

        #!/usr/bin/env ruby
        require "lorem/cli"

        Lorem::CLI.start

5. In lorem.gemspec, add this before `end`

        spec.add_runtime_dependency "thor"

### Run Your Unpublished Gem

There are 2 possible ways to run it.

+ `$ ruby -I lib bin/lorem ipsum`

+ `$ bundle exec bin/lorem ipsum`

    If you get an ` $ ERROR bundler: not executable: bin/gem_command_name`  
    
    Run ` $ chmod +x bin/gem_command_name`


### Install your gem locally. 

Once it is installed locally you can run your gem in your command line by it's name. `$ lorem`
    
    gem install --local lorem

### Push to Ruby Gems

    gem push zipper-0.0.1.gem


### Installing Your Gem Once It's Published

Add this line to your application's Gemfile:

    gem 'lorem'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install lorem

# Add an Rspec Test

We are trying to test that Lorem Ipsum is actually generated. This creates a spec Directory and a spec_helper.rb

1. Run this in your command line. 

    $ rspec --init

   Checkout the files that were just generated.

        $ ls
        
        Gemfile
        lib
         L lorem.rb
         L lorem
         L  version.rb
        LICENSE.txt   
        lorem.gemspec
        Rakefile
        README.md
        spec
         L spec_helper.rb


2. In the lorem.gemspec file, add `spec.add_development_dependency "rspec"` before `end`

        \# coding: utf-8
        lib = File.expand_path('../lib', __FILE__)
        $LOAD_PATH.unshift(lib) unless $LOAD_PATH.include?(lib)
        require 'lorem/version'
    
        Gem::Specification.new do |spec|
          spec.name          = "lorem"
          spec.version       = Lorem::VERSION
          spec.platform      = Gem::Platform::RUBY
          spec.authors       = ["your name"]
          spec.email         = ["your@email.com"]
          spec.homepage      = ""
          spec.summary       = %q{Jenipsum generator}
          spec.description   = %q{Generates jenipsum text}
          spec.license       = "MIT"

          spec.add_development_dependency "bundler", "~> 1.3"
          spec.add_development_dependency "rake"
          spec.add_runtime_dependency "thor"
          spec.add_development_dependency "rspec"
            
          spec.files         = `git ls-files`.split($/)
          spec.test_files    = spec.files.grep(%r{^(test|spec|features)/})
          spec.executables   = spec.files.grep(%r{^bin/}) { |f|  File.basename(f) }
          spec.require_paths = ["lib"]

        end 

3. Create a `lorem_spec.rb` file in your spec directory. This is where you will write your test.

    $ touch spec/lorem_spec.rb


4. Write this into your spec/lorem_spec.rb

        require 'spec_helper'

        module Lorem
          describe Lorem do
            it "should not be empty" do
              expect(Lorem.ipsum).to_not be_empty
            end

            it "should include 'Fantastic'" do
              expect(Lorem.ipsum).to include('Fantastic')
            end
          end
        end

5. Require your lorem gem in your spec/spec_helper.rb

        require "lorem"

        # This file was generated by the `rspec --init` command. Conventionally, all
        # specs live under a `spec` directory, which RSpec adds to the `$LOAD_PATH`.
        # Require this file using `require "spec_helper"` to ensure that it is only
        # loaded once.
        #
        # See http://rubydoc.info/gems/rspec-core/RSpec/Core/Configuration
        RSpec.configure do |config|
          config.treat_symbols_as_metadata_keys_with_true_values = true
          config.run_all_when_everything_filtered = true
          config.filter_run :focus

          # Run specs in random order to surface order dependencies. If you find an
          # order dependency and want to debug it, you can fix the order by providing
          # the seed, which is printed after each run.
          #     --seed 1234
          config.order = 'random'
        end 

6. It is now time to run your test.

        $ rspec spec/lorem_spec.rb

  You should see two passing tests written in green. If instead you see red failures 
  read your errors and keep fixing your code until both tests pass. 


# Congratulations. You now have a simple Lorem Ipsum gem to experiment with.

  Try adding flags and your own Lorem Ipsum. Install it. Publish it. Have fun with your new gem.
