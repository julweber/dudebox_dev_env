# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # Use Ubuntu 14.04 Trusty Tahr 64-bit as our operating system
  config.vm.box = "ubuntu/trusty64"

  # Configurate the virtual machine to use 4GB of RAM
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096"]
  end

  # Forward the Rails server default port to the host
  config.vm.network :forwarded_port, guest: 3000, host: 3000
  # postgresql
  config.vm.network :forwarded_port, guest: 5432, host: 5432
  # mongodb
  config.vm.network :forwarded_port, guest: 27017, host: 27017
  config.vm.network :forwarded_port, guest: 28017, host: 28017
  # redis
  config.vm.network :forwarded_port, guest: 6379, host: 6379
  # rabbitmq
  config.vm.network :forwarded_port, guest: 5672, host: 5672
  config.vm.network :forwarded_port, guest: 15671, host: 15671
  config.vm.network :forwarded_port, guest: 25672, host: 25672



  # synced folders
  config.vm.synced_folder "workspace/", "/vagrant/workspace"

  # Chef solo config
  config.vm.provision "chef_solo" do |chef|
    chef.node_name = "dudebox"
    chef.version = '12.10.40'
    chef.cookbooks_path = "cookbooks"

    chef.add_recipe "apt"
    chef.add_recipe "nodejs"
    chef.add_recipe "ruby_build"
    # chef.add_recipe "rbenv::user"
    # chef.add_recipe "rbenv::vagrant"
    chef.add_recipe "rvm::system"
    chef.add_recipe "vim"
    chef.add_recipe "redis2::default_instance"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "postgresql::client"
    chef.add_recipe "mongodb::default"
    chef.add_recipe "rabbitmq::default"
    chef.add_recipe "rabbitmq::mgmt_console"
    chef.add_recipe "golang::default"
    chef.add_recipe "java::default"
    chef.add_recipe "maven::default"
    chef.add_recipe "build-essential::default"
    chef.add_recipe "git::default"

    # Install Ruby 2.2.1 and Bundler
    chef.json = {
      rbenv: {
        user_installs: [{
          user: 'vagrant',
          rubies: ["2.2.1"],
          global: "2.2.1",
          gems: {
            "2.2.1" => [
              { name: "bundler" }
            ]
          }
        }]
      },
      go: {
        version: '1.4.2'
      },
      rabbitmq: {
        web_console_ssl: true,
        web_console_ssl_port: 15671
      },
      java: {
        oracle: {
          accept_oracle_download_terms: true
        },
        install_flavor: "oracle",
        jdk_version: 8
      },
      postgresql: {
        version: '9.3',
        config: {
          log_rotation_age: "1d",
          log_rotation_size: "10MB",
          log_filename: "postgresql-%Y-%m-%d_%H%M%S.log",
          listen_addresses: '0.0.0.0'
        },
        pg_hba: [
          {
            type: 'local',
            db: 'all',
            user: 'postgres',
            addr: nil,
            method: 'ident'
          },
          {
            type: 'local',
            db: 'all',
            user: 'all',
            addr: nil,
            method: 'ident'
          },
          {
            type: 'local',
            db: 'all',
            user: 'all',
            addr: nil,
            method: 'peer'
          },
          {
            comment: '# allow outside connections from all hosts using md5 passwords',
            type: 'host',
            db: 'all',
            user: 'all',
            addr: '0.0.0.0/0',
            method: 'md5'
          }
        ],
        password: {
          postgres: 'test123!'
        }
      }
    }
  end

end
