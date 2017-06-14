
  
 # **Not fully ported to markdown and most likely out of date!**
  
  <!-- toc -->
  
  Table of contents
  -----------------
  
    * [Images](#images)
    * [Running](#running)
    * [Information](#information)
    
  <!-- toc stop -->
  
  Setup
  -----
  
  ### Create new rails app
  
      rails new myapp
  
  Add to Gemfile (replace versions/names):
  
  ```
  ruby '2.1.4'
  #ruby-gemset=rails420
  # Optonionally use 'thin' as the webserver
  gem 'thin'
  ```
  
  Add to .gitignore:
  
  ```
  vendor/bundle/*
  public/assets/*
  # Ignore Spring files.
  /spring/*.pid
  ```
  
  DEPRECATED? Source code is public visible? Then change \'''''config/initializers/secret_token.rb\''''':
  
  ```
  require 'securerandom'
  def secure_token
    token_file = Rails.root.join('.secret')
    if File.exist?(token_file)
      # Use the existing token.
      File.read(token_file).chomp
    else
      # Generate a new token and store it in token_file.
      token = SecureRandom.hex(64)
      File.write(token_file, token)
      token
    end
  end
  SampleApp::Application.config.secret_key_base = secure_token
  ```
  
  AngularJS
  ---------
  
  http://rubygems.org/gems/angularjs-rails
  
  ### Installation
  
  Add to Gemfile:
  
      gem 'angularjs-rails'
  
  Add to application.js:
  
  ```
  //= require angular
  //= require angular-route
  ```
  
  Needed for rails´ CSRF security function to work with POST requests. Add the following code somewhere in your angular app:
  
  ```
  app.config(function ($httpProvider) {
      authToken = $("meta[name=\\"csrf-token\\"]").attr("content")
      $httpProvider.defaults.headers.common["X-CSRF-TOKEN"] = authToken
  });
  ```
  
  
  Bootstrap
  ---------
  
  http://rubygems.org/gems/bootstrap-sass
  
  http://rubygems.org/gems/font-awesome-rails
  
  ### Installation
  
  Add to Gemfile (check versions):
  
  ```
  gem 'bootstrap-sass'
  gem 'font-awesome-rails'
  ```
  
  Add to application.js:
  
      //= require bootstrap
  
  
  Rename application.css to have sass functionality:
  
      mv app/assets/stylesheets/application.css app/assets/stylesheets/application.css.scss
  
  
  Add to application.css.scss
  
  ```
  @import "bootstrap";
  //@import "bootstrap/theme";
  @import "font-awesome";
  ```
  
  Custom config with YAML
  -----------------------
  
  Create new file \'''''config/initializers/app_config.rb\''''' (name doesn´t matter, all files in initializers get loaded upon app start). Content:
  
  ```
  require 'ostruct'
  require 'yaml'
  
  # Looks for a 'app_config.yml' file in config/ directory
  config = YAML.load_file( File.join(Rails.root, 'config', 'app_config.yml')) || {}
  AppConfig = OpenStruct.new(config[Rails.env] || {} )
  ```
  
  Create \'''''config/app_config.yml\''''' and add content:
  
  ```
  defaults: &defaults
    animals:
      foo: "rabbit"
      bar: "dog"
    coinarts:
      - BTC
      - FST
      - LTC
  
  development:
    <<: *defaults
  
  test:
    <<: *defaults
  
  production:
    <<: *defaults
  ```
  
  Now you use can something like this anywhere in the app (also in rake tasks):
  
  ```
  puts AppConfig.animals['foo']
  AppConfig.coinarts.each do |coinart|
    puts coinart
  end
  ```
  
  HAML
  ----
  
  ### Installation
  
  Add to Gemfile (check versions):
  
  ```
  gem 'haml'
  gem 'haml-rails'
  gem 'html2haml'
  ```
  
  === Command line tasks ===
  
  Convert and remove all .erb files in current (and sub-) directory:
  
  ```
  find . -name \\*.erb -print | sed 'p;s/.erb$/.haml/' | xargs -n2 html2haml
  rm `find . -name \\*.erb`
  ```
  
  
  Testing
  -------
  
  With the following gems included:
  
  ```
  group :test do
    gem 'minitest-reporters' # Colored output
    gem 'mini_backtrace' # Cuts / Shortens the error output
    gem 'guard-minitest'
  end
  ```
  
  
  Add this to \'''''test/test_helper.rb if you want colored testing output\'''''
  
  ```
  # Enable minitest - reporters to have colored output
  require "minitest/reporters"
  Minitest::Reporters.use!
  ```
  
  
  Add this to \'''''config/initializers/backtrace_silencers.rb\''''' if you want filtered/shortened error output  thats customizable with filters
  
  ```
  # Enable mini_backrace to filter away stacktrace that contains /rvm/
  Rails.backtrace_cleaner.add_silencer { |line| line =~ /rvm/ }
  ```
  
  To invoke a test use
  
      spring rake test
  
  
  
  Active Record
  -------------
  
  ### Associations
  
  #### ser has one Account - Account belongs to User
  
  Assume user model exists. Create account model:
  
      rails g model account user:references balance:float
  
  Update user model to include "''has_one :account [,dependent: :destroy]''" and account model includes  "''belongs_to :user''".
  
  Then in \'''''rails console\''''':
  
  ```
  # One (seemingly bad, cause this way multiple accounts per user can be created) way to create account:
  Account.create(balance: 123.456, user_id: 1)
  u=User.find_by_id(1)
  u.account
  # Better way:
  u=User.find_by_id(1)
  u.create_account(balance: 123.456)
  # Queries:
  u.account.balance
  User.find_by_id(1).account.balance
  # Updates:
  u.account.update(balance: 123.9)
  # Destroy
  u.account.destroy
  ```
  
  
  #### User has many Trading Incomes - Trading Incomes belong to User
  
  Assume user model exists. Create trading_income model:
  
   rails g model trading_income user:references amount:float income_at:datetime from_source:string
  
  Update user model to include "''has_many :trading_incomes [,dependent: :destroy]''" and trading_income model includes  "''belongs_to :user''".
  
  Then in \'''''rails console\''''':
  
  ```
  # Create:
  u=User.find_by_id(1)
  u.trading_incomes.create(amount: 4.33, inc_at: Time.now, from_source: "Bank XY")
  # Queries:
  u.trading_incomes
  u.trading_incomes.first
  ## Returns array:
  u.trading_incomes.where(from_source: "Bank XY")
  ## Returns single object:
  u.trading_incomes.find_by_id(3)
  u.trading_incomes.find_by_from_source("Bank XY")
  # Updates:
  u.trading_incomes.find_by_id(3).update(amount: 9000.0)
  # Destroy
  u.trading_incomes.find_by_id(3).destroy
  ```
  
  Callbacks
  ---------
  
  Example for updating anothter model after saving the first model:
  
  ```
  class TradeFstMininc < ActiveRecord::Base
    belongs_to :user
    after_save do |inc|
      puts "FST Mining income was saved!"
      puts "Running callback..."
      u = User.find_by_id(inc.user_id)
      puts "Old balance:"
      puts u.trade_fst_balance.balance
      new_balance = inc.amount + u.trade_fst_balance.balance
      u.trade_fst_balance.update(balance: new_balance)
      puts "New balance:"
      puts u.trade_fst_balance.balance
    end
  end
  ```
  
  Lokalisierung
  -------------
  
  In \'''"config/application.rb"\''':
  
   config.i18n.default_locale = :de
  
  Datei \'''"config/locales/de.yml"\''' anlegen mit Inhalt ("employee" ist model mit u. a. den genannten Attributen):
  
  <source lang="ruby">
  de:
    activerecord:
      attributes:
        employee:
          first_name: "Vorname"
          last_name: "Nachname"
      errors:
        models:
          employee:
            attributes:
              first_name:
                blank: "ist erforderlich"
              last_name:
                blank: "ist erforderlich"
  </source>
  
  =Deployment=
  
  ==Prepare app on production server==
  
  <source lang="bash">
  bundle install --deployment
  bundle exec rake db:migrate RAILS_ENV=production
  bundle exec rake assets:clean RAILS_ENV=production
  # Or
  bundle exec rake assets:clobber RAILS_ENV=production # deletes also if there where no changes
  bundle exec rake assets:precompile RAILS_ENV=production
  RAILS_ENV=production bundle exec rake secret
  # Change key to generated!
  export SECRET_KEY_BASE=2b8ed4ff79a69a5d30cd73e33e3c2d432c007a0260acea7f6902285c0e84194a21f1ac3ebedf483b169e9bcb352abb707c87d26ecb56722c15f330808a9d049a
  # Star server in daemon mode
  ./bin/rails server -d -e production -p 3001 -b 127.0.0.1
  # Stop server
  kill `cat tmp/pids/server.pid`
  </source>
  
  
  Apache configuration
  --------------------
  
      sudo a2enmod proxy_html proxy_http proxy rewrite
  
  Seit Ubuntu 14.04:
  
      sudo a2enmod proxy_html proxy_http proxy rewrite proxy_balancer lbmethod_byrequests 
  
  
  ```
  <VirtualHost *:80>
          ServerName  mydomain.com
          ServerAlias www.mydomain.com
  
          DocumentRoot /home/chris/rails_deploy_sample/public
          <Directory /home/chris/rails_deploy_sample/public>
              Options Indexes FollowSymLinks Includes ExecCGI
              AllowOverride All
              Require all granted
              Allow from all
          </Directory>
  
          RewriteEngine On
  
          <Proxy balancer://thinservers>
            BalancerMember http://127.0.0.1:3001
          </Proxy>
  
          RewriteCond %{DOCUMENT_ROOT}/%{REQUEST_FILENAME} !-f
          RewriteRule ^/(.*)$ balancer://thinservers%{REQUEST_URI} [P,QSA,L]
  
  #        ProxyPass / balancer://thinservers/
  #        ProxyPassReverse / balancer://thinservers/
          ProxyPreserveHost on
          <Proxy *>
            Order deny,allow
            Allow from All
          </Proxy>
          #<Directory /home/chris/coincountr/public>
          #        AllowOverride All
          #        Options -Multiviews
          #</Directory>
          ErrorLog /var/log/apache2/coincountr-error.log
          CustomLog /var/log/apache2/coincountr-access.log combined
  </VirtualHost>
  ```
