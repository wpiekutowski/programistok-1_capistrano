!SLIDE bullets
# Capistrano #
* www.github.com/capistrano/capistrano


!SLIDE bullets incremental
# platformy
* Linux
* Windows Server (gem capistrano-windows-server)


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
    set :repository, "git@amberbit.com:beer_machine.git"


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


!SLIDE
# user
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
      ["app1.beer-machine.com", "app2.beer-machine.com"]
    role :db,
      "db1.beer-machine.com", :primary => true
    role :db,
      ["db2.beer-machine.com", "db3.beer-machine.com"]


!SLIDE commandline incremental
# ich pierwszy raz
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


!SLIDE commandline incremental
# do dzieła!
    $ cap deploy


!SLIDE bullets incremental
# deploy:update_code
* pobierz najnowszy kod z git/svn
* przejdź na gałąź
* skopiuj kod do app/releases/20110901191102


!SLIDE bullets incremental
# deploy:finalize_update
* ustaw datę modyfikacji plików na teraz


!SLIDE bullets incremental
# deploy:symlink
* stwórz link symboliczny app/current → app/releases/20110901191102


!SLIDE bullets incremental
# deploy:restart
* zrestartuj serwery aplikacji
