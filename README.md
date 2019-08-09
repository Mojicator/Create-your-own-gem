# Create your own Ruby Gem with Bundler

Bundler provides a consistent environment for Ruby projects by tracking and installing the exact gems and versions that are needed.

Bundler is an exit from dependency hell, and ensures that the gems you need are present in development, staging, and production. Starting work on a project is as simple as `bundle install`

## Getting Started

Getting started with bundler is easy! Open a terminal window and run this command:

```
gem install bundler
```

To begin to create a gem using Bundler, use the `bundle gem` command like this:

```
bundle gem my_name_gem
```

This command creates a scaffold directory for our new gem and, if we have Git installed, initializes a Git repository in this directory so we can start committing right away. If this is your first time running the `bundle gem` command, you will be asked about which testing framework do you prefer and if you want to include **CODE_OF_CONDUCT.md** and **LICENSE.txt** files with your project.

* Do you want to generate tests with your gem? (rspec/minitest)
* Do you want to license your code permissively under the MIT license? (yes/no)
* Do you want to include a code of conduct in gems you generate? (yes/no)

***

* **Gemfile**: Used to manage gem dependencies for our library’s development. This file contains a `gemspec` line meaning that Bundler will include dependencies specified in foodie.gemspec too. It’s best practice to specify all the gems that our library depends on in the gemspec.

* **Rakefile**: Requires Bundler and adds the `build`, `install` and `release` Rake tasks by way of calling `Bundler::GemHelper.install_tasks`. The `build` task will build the current version of the gem and store it under the pkg folder, the `install` task will build and install the gem to our system (just like it would do if we `gem install`‘d it) and `release` will push the gem to Rubygems for consumption by the public.

* **CODE_OF_CONDUCT.md**: Provides a code of conduct that you expect all contributors to your gem to follow. Will only be included if you chose to have it included.

* **LICENSE.txt**: Includes the MIT license. Will only be included if you chose to have it included.

* **.gitignore**: (only if we have Git). This ignores anything in the pkg directory (generally files put there by `rake build`), anything with a .gem extension and the .bundle directory.

* **my_name_gem.gemspec**: The Gem Specification file. This is where we provide information for Rubygems’ consumption such as the name, description and homepage of our gem. This is also where we specify the dependencies our gem needs to run.

    > If you attemp to `require 'my_name_gem'` inside irb and get the following error:

    ```ruby
    LoadError: cannot load such file -- my_name_gem
    ```

    The `spec.files` entry of your `gemspec` doesn’t include the `my_name_gem.rb` file, so that file won‘t be in the gem when it is built. Only files listed in this entry will be included in the final gem.

    The simplest solution would be to just add `my_name_gem.rb` to the array:

    ```ruby
    spec.files = ['lib/command.rb', 'lib/connection.rb', 'lib/mygem.rb']
    ```

    This is a fairly simple fix:

    ```ruby
    spec.files = Dir['lib/**/*.rb']
    ```

* **lib/my_name_gem.rb**: Code for your package is placed within the `lib` directory. The convention is to have one Ruby file with the same name as your gem, since that gets loaded when require 'hola' is run. That one file is in charge of setting up your gem’s code and API. The main file to define our gem’s code. 

* **lib/my_name_gem**: This folder should contain all the code (classes, etc.) for our gem. If our gem has multiple uses, separating this out so that people can require one class/file at a time can be really helpful.

* **lib/my_name_gem/version.rb**: Defines a module and in it, a `VERSION` constant. This file is loaded by the foodie.gemspec to specify a version for the gem specification. When we release a new version of the gem we will increment a part of this version number to indicate to Rubygems that we’re releasing a new version.

***

### Minitest

> For more detailed information, we recomend the following post: [Introduction to Minitest](https://launchschool.com/blog/assert-yourself-an-introduction-to-minitest)

***

## Releasing the gem

Now we’re going to make sure that our gem is ready to be published. To do this, we can run `rake build` which will build a local copy of our gem and then `gem install pkg/foodie-0.1.0.gem` to install it. Then we can try it locally by running the commands that it provides.