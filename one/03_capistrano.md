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


!SLIDE commandline incremental
# ich pierwszy raz
    $ cap deploy:setup


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
