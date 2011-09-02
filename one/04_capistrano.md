!SLIDE bullets
# Capistrano #
* www.github.com/capistrano/capistrano


!SLIDE bullets incremental
# platformy
* Linux
* Windows Server


!SLIDE bullets incremental
# wymagania
* SSH
* jedno hasło do wszystkich serwerów
* lub klucze publiczne


!SLIDE commandline incremental
# instalacja #
    $ gem install capistrano
    $ cd projects/beer_machine
    $ capify .
    [add] writing './Capfile'
    [add] writing './config/deploy.rb'
    [done] capified!


!SLIDE bullets incremental
# Capfile
* czyli Makefile, Rakefile


!SLIDE
# git #
    @@@ ruby
    set :scm, "git"
    set :repository,
      "git@amberbit.com:beer_machine.git"


!SLIDE
# svn
    @@@ ruby
    set :scm, "svn"
    set :repository,
      "svn+ssh://wojtek@svn.amberbit.com/beer_machine"


!SLIDE
# gałąź git/svn
    @@@ ruby
    set :branch, "master"


!SLIDE
# moduły git
    @@@ ruby
    set :git_enable_submodules, 1


!SLIDE
# strategia wdrożenia
    @@@ ruby
    set :deploy_via, :remote_cache
    # or
    set :deploy_via, :export


!SLIDE
# app/deploy user
    @@@ ruby
    set :user, "beer_machine"


!SLIDE
# gdzie?
    @@@ ruby
    set :deploy_to, "/home/beer_machine/app"


!SLIDE
# historia
    @@@ ruby
    set :keep_releases, 10


!SLIDE
# serwer
    @@@ ruby
    role :web, "beer-machine.com"
    role :app, "beer-machine.com"
    role :db,  "beer-machine.com", :primary => true


!SLIDE
# serwery (bigger than Google!)
    @@@ ruby
    role :web, "beer-machine.com"
    role :app,
      "app1.beer-machine.com", "app2.beer-machine.com"
    role :db,
      "db1.beer-machine.com", :primary => true
    role :db,
      "db2.beer-machine.com", "db3.beer-machine.com"


!SLIDE commandline incremental
# ich pierwszy raz ♥
    $ cap deploy:setup
    /home/beer_machine/app
    /home/beer_machine/app/releases
    /home/beer_machine/app/shared
    /home/beer_machine/app/shared/log
    /home/beer_machine/app/shared/pids
    /home/beer_machine/app/shared/system


!SLIDE bullets incremental
# ~/app/releases
* zawiera kolejne wdrożenia
* ~/app/releases/20110901201000
* ~/app/releases/20110903110415
* ~/app/releases/...


!SLIDE commandline incremental
# pasy zapięte?
    $ cap deploy:check


!SLIDE commandline incremental
# pierwsza aktualizacja kodu
    $ cap deploy:update


!SLIDE bullets incremental
# deploy:update_code
* pobierz najnowszy kod z git/svn
* przejdź na gałąź
* skopiuj kod do app/releases/20110901191102


!SLIDE bullets incremental
# deploy:finalize_update
* ustaw datę modyfikacji plików na teraz


!SLIDE commandline incremental
# do dzieła!
    $ cap deploy


!SLIDE bullets incremental
# deploy:symlink
* stwórz link symboliczny app/current → app/releases/20110901191102


!SLIDE bullets
# deploy:restart
* zrestartuj serwery aplikacji

!SLIDE image center
![AmberBit](images/message.jpg)

!SLIDE bullets
# fail?
* cap deploy:rollback

!SLIDE bullets
# ssh?
* cap shell


!SLIDE command
# config/deploy.rb
    @@@ ruby
    set :scm, :git
    set :repository, "git@amberbit.com:beer_machine.git"
    set :branch, "v1.24.2"
    set :deploy_via, :remote_cache
    set :keep_releases, 10

!SLIDE command
# config/deploy.rb
    @@@ ruby
    set :application, "beer_machine"
    set :stage, "production"
    set :user, "#{application}-#{stage}"
    set :group, "#{application}-#{stage}"

!SLIDE command
# config/deploy.rb
    @@@ ruby
    role :web, "amberbit.com"
    role :app, "amberbit.com"
    role :db,  "amberbit.com", :primary => true
    set :url, "beer-machine.amberbit.com"
    set :deploy_to, "/home/#{user}/app"

!SLIDE command
# config/deploy.rb
    @@@ ruby
    namespace :deploy do
      task :start do ; end
      task :stop do ; end

      desc "Restart passenger"
      task :restart, :roles => :app do
        file = File.join(current_path, 'tmp',
          'restart.txt')
        run "touch #{file}"
        run "curl #{fetch :url} &"
      end
    end

!SLIDE command
# config/deploy.rb
    @@@ ruby
    namespace :config do
      desc "Link shared configurations in release"
      task :link_shared_configurations, :roles => :app do
        files = %w[mongoid.yml application/config.yml]
        files.each do |f|
          from = "#{shared_path}/config/#{f}"
          to = "#{release_path}/config/#{f}"
          run "ln -nsf #{from} #{to}"
        end
      end
    end

!SLIDE command
# config/deploy.rb
    @@@ ruby
    after "deploy:update_code",
      "config:link_shared_configurations"
    after "deploy:update_code",
      "deploy:cleanup" # remove old releases
