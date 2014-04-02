robot-controller
================

Monitors and controls running workflow robots off of priority queues and within a cluster.

## Configuration

In your `Gemfile`, add:

    gem 'robot-controller', :github => 'sul-dlss/robot-controller'

In your `Rakefile`, add the following (if you don't want to include the environment unconditionally):

    require 'resque/tasks'
    Dir.glob('lib/tasks/*.rake').each { |r| import r }
    
Create the following configuration files based on the examples in `config`:

    config/boot.rb
    config/environments/development.rb
    config/environments/bluepill_development.rb
    config/environments/workflows_development.rb

Create a `config.ru` file as follows to install tabs and run Resque monitoring UI:

    require 'resque/server'
    require File.expand_path(File.dirname(__FILE__) + '/./lib/resque/resque-robot-controller')

    Resque.redis = 'localhost:6379:0/resque:development'

    run Rack::URLMap.new \
      "/"       => Resque::Server.new


### Usage

    Usage: controller [ boot | quit ]
           controller [ start | status | stop | restart | log ] [worker]

    Example:
     % controller boot    # start bluepilld and jobs
     % controller status  # check on status of jobs
     % controller log dor_accessionWF_descriptive-metadata # view log for worker
     % controller stop    # stop jobs
     % controller quit    # stop bluepilld
