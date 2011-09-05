!SLIDE transition=none
# FPM #
## The Swiss Army knife of the casual packager ##

!SLIDE transition=none
## A day in the life of a DevOps ##

!SLIDE incremental transition=none
* When operating a production environment, one of your best friends is your *configuration management software*.
* *Puppet* is a well known open source configuration management tool.

!SLIDE bullets transition=none
What Puppet typically handles for you:

* Network, Email and system tools
* System user accounts
* Monitoring agent
* Web server
* Database server
* Ruby runtime
* All your configuration files

!SLIDE incremental transition=none
* All you have to do now is deploy your app and maybe some Gems.
* Puppet knows how to install (and uninstall) *native packages*.
* Wanna build one for your app?

!SLIDE incremental transition=none
* Native packaging is <span class="good">great</span>
* Native packaging is <span class="bad">hard</span>
* Distribution policies are here for *good reasons*...
* ...but you don't care about policies if you are not a maintainer!

!SLIDE transition=none
## FPM to the rescue ##

!SLIDE bullets incremental transition=none
FPM puts a *native package blanket* on top of:

* directories
* gems
* Python modules
* Node packages

!SLIDE transition=none
It'll make it deployable by your system packages manager but won't fool a maintainer.

!SLIDE transition=none
## FPM in practice ##

!SLIDE topalign transition=none
### Packaging the runtime deps of *rivierarb.fr* ###
    @@@ sh
    sudo gem install fpm
    
    mkdir -p ~/tmp/gems
    cd ~/tmp
    
    gem install --no-ri --no-rdoc \
                --install-dir ~/tmp/gems \
                jekyll compass rdiscount
    
    find ~/tmp/gems/cache/ -name "*.gem" | xargs -rn1 \
    fpm -s gem -t deb

!SLIDE topalign transition=none
### Packaging the content of *rivierarb.fr* ###
    @@@ sh
    mkdir -p ~/tmp/opt/rivierarb
    
    wget -O - http://github.com/rivierarb/rivierarb/tarball/gh-pages|\
    tar -xz --strip-components=1 -C ~/tmp/opt/rivierarb
    
    fpm -s dir -t deb -n rivierarb -v 0.2.0 -a all \
        -d "rubygem-jekyll (>= 0.11.0)" \
        -d "rubygem-compass (>= 0.11.5)" \
        -d "rubygem-rdiscount (>= 1.6.8)" \
        ./opt/rivierarb

!SLIDE topalign commandline incremental transition=none
### Deploying *rivierarb.fr* ###
    @@@ sh
    $ ls -1 *.deb
    
    rivierarb_0.2.0_all.deb
    rubygem-albino_1.3.3_all.deb
    rubygem-chunky-png_1.2.1_all.deb
    rubygem-classifier_1.3.3_all.deb
    rubygem-compass_0.11.5_all.deb
    rubygem-directory-watcher_1.4.1_all.deb
    rubygem-fast-stemmer_1.0.0_amd64.deb
    rubygem-fssm_0.2.7_all.deb
    rubygem-jekyll_0.11.0_all.deb
    rubygem-kramdown_0.13.3_all.deb
    rubygem-liquid_2.2.2_all.deb
    rubygem-maruku_0.6.0_all.deb
    rubygem-posix-spawn_0.3.6_amd64.deb
    rubygem-rdiscount_1.6.8_amd64.deb
    rubygem-sass_3.1.7_all.deb
    rubygem-syntax_1.0.0_all.deb

!SLIDE topalign bullets incremental transition=none
### Deploying *rivierarb.fr* ###
Next steps:

* Upload all packages to your private repository
* Reference the new version in your Puppet manifest

!SLIDE topalign transition=none
### Deploying *rivierarb.fr* ###
    @@@ ruby
    package { 'rivierarb': ensure => '0.2.0' }

!SLIDE transition=none
# References #
