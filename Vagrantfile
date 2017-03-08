# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.omnibus.chef_version = :latest

  config.berkshelf.enabled = true

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.

  # config.vm.box = "Canonical Ubuntu 16.04 LTS"
  config.vm.box = "ubuntu/xenial64"

  # config.vm.box_url = "https://cloud-images.ubuntu.com/releases/16.04/release/ubuntu-16.04-server-cloudimg-amd64-vagrant.box"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.network "forwarded_port", guest: 3000, host: 3000 # rails app
  config.vm.network "forwarded_port", guest: 5432, host: 5432 # postgres
  config.vm.network "forwarded_port", guest: 6379, host: 6379 # redis
  config.vm.network "forwarded_port", guest: 27017, host: 27017 # mongodb
  config.vm.network "forwarded_port", guest: 3306, host: 3306 # mysql

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.synced_folder ".", "/vagrant", nfs: true

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.ssh.forward_agent = true

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  config.vm.provision :chef_zero do |chef|

      chef.verbose_logging = true

      rvm_config = {
        :rvm => {
          # :group_users => ['ubuntu'], # Added to deal with a issue with vagrant not beloging to the rvm group
          :default_ruby => "2.3.3",
          :global_gems => [
            { :name => 'bundler' }
          ],
          # :rubies => ["2.4.0"]
        }
      }

      mysql_config = {
        'small-stuff' => {
          :mysql => {
            :server_root_password => "dev-password",
            :bind_address => '0.0.0.0',
            :allow_remote_root_access => true
          }
        }
      }

      java_config = {
        :java => {
          :install_flavor => "oracle",
          :jdk_version => "8",
          :oracle => {
            :accept_oracle_download_terms => true
          }
        }
      }

      elasticsearch_config = {
        :elasticsearch => {
          :allocated_memory => '1024m',
          :version => "5.2.0",
          :cluster => {
            :name => "development"
          }
        }
      }

      redis_config = {
        :redisio => {
          :version => '3.2.6'
        }
      }

      nodejs_config = {
        :nodejs => {
          :install_method => 'binary',
          :version => '6.9.1',
          :binary => {
            :checksum => 'd4eb161e4715e11bbef816a6c577974271e2bddae9cf008744627676ff00036a'
          }
        }
      }

      mongodb_config = {
        :mongodb3 => {
          :version => "3.4.2"
        }
      }

      postgresql_config = {
        :postgresql => {
          :version => '9.5',
          :password => {
            :postgres => "dev-password"
          },
          :config => {
            :listen_addresses => '*'
          },
          :pg_hba => [
            #default postgres pg_hba config.
            {:type => 'local', :db => 'all', :user => 'postgres', :addr => nil, :method => 'ident'},
            {:type => 'local', :db => 'all', :user => 'all', :addr => nil, :method => 'ident'},
            {:type => 'host', :db => 'all', :user => 'all', :addr => '127.0.0.1/32', :method => 'md5'},
            {:type => 'host', :db => 'all', :user => 'all', :addr => '::1/128', :method => 'md5'},
            #add external access to postgres.
            {:type => 'host', :db => 'all', :user => 'postgres', :addr => '0.0.0.0/0', :method => 'md5'}
          ]
        }
      }

      chef.nodes_path = './tmp/chef/nodes'

      chef.add_recipe 'apt'

      # Install Java (e.g: for Jruby or elasticsearch)
      # chef.json.merge! java_config
      # chef.add_recipe 'java'

      # Install RVM System-Wide
      # chef.json.merge! rvm_config
      # chef.add_recipe 'rvm::system'

      # Install Redis
      # chef.json.merge! redis_config
      # chef.add_recipe 'redisio'
      # chef.add_recipe 'redisio::enable'

      # Install Imagemagick
      # chef.add_recipe 'imagemagick'

      # Install Images compression packaages
      # chef.add_recipe 'small-stuff::jpegoptim'
      # chef.add_recipe 'small-stuff::optipng'
      # chef.add_recipe 'small-stuff::pngcrush'

      # Install Memcached
      # chef.add_recipe 'memcached'

      # Install aws cli
      # chef.add_recipe 'cloudcli::awscli'

      # Install phantomjs
      # chef.add_recipe 'phantomjs2::default'

      # Install nodejs
      # chef.json.merge! nodejs_config
      # chef.add_recipe 'nodejs'

      # Install elasticsearch
      # chef.json.merge! elasticsearch_config
      # chef.add_recipe 'elasticsearch'

      # Install mysql
      # chef.json.merge! mysql_config
      # chef.add_recipe 'small-stuff::mysql_server'
      # chef.add_recipe 'small-stuff::mysql_client'

      # Install mongodb
      # chef.json.merge! mongodb_config
      # chef.add_recipe 'mongodb3::default'

      # Install postgresql
      # chef.json.merge! postgresql_config
      # chef.add_recipe 'postgresql::server'

    end
end
