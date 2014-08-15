# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  #config.vm.box = "precise64"
  #config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-amd64-vagrant-disk1.box"
  #config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.100.100"

  # vagrant-hostmanager plugin
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  config.vm.define 'trusty64-box' do |node|
    node.vm.hostname = 'trusty64'
    node.vm.network :private_network, ip: '192.168.100.100'
    node.hostmanager.aliases = %w(trusty64.local trusty64-box-alias)
  end

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"
  #config.vm.synced_folder "./docroot", "/var/www", type: "nfs"
  #config.vm.synced_folder ".", "/home/vagrant/devlocal", type: "nfs"
  config.vm.synced_folder "docroot", "/var/www/nginx-default"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode
    # vb.gui = true

    vb.cpus = 2
    vb.customize ["modifyvm", :id, "--name", "trusty64"]
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ['modifyvm', :id, '--ioapic', 'on']
  end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  # Shell Provisioner
  config.vm.provision :shell, :path => "provisioners/shell/timezone.sh"
  #config.vm.provision :shell, :path => "provisioner/shell/upgrade.sh"
  #config.vm.provision :shell, :path => "provisioner/shell/buildessential.sh"

  # Enable provisioning with CFEngine. CFEngine Community packages are
  # automatically installed. For example, configure the host as a
  # policy server and optionally a policy file to run:
  #
  # config.vm.provision "cfengine" do |cf|
  #   cf.am_policy_hub = true
  #   # cf.run_file = "motd.cf"
  # end
  #
  # You can also configure and bootstrap a client to an existing
  # policy server:
  #
  # config.vm.provision "cfengine" do |cf|
  #   cf.policy_server_address = "10.0.2.15"
  # end

  # Enable provisioning with Puppet stand alone.  Puppet manifests
  # are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in
  # the file default.pp in the manifests_path directory.
  #
  #config.vm.provision :puppet do |puppet|
  #  puppet.manifests_path = "provisioner/puppet/manifests"
  #  puppet.manifest_file  = "local.pp"
  #  puppet.module_path    = "provisioner/puppet/modules"
  #end

  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.
  #
  config.vm.provision "chef_solo" do |chef|
    chef.custom_config_path = 'chef_streaming_fix.rb'
    chef.cookbooks_path = "./provisioners/chef-solo/cookbooks"
    chef.roles_path = "./provisioners/chef-solo/roles"
    chef.data_bags_path = "./provisioners/chef-solo/data_bags"
    
    chef.add_recipe "apt"

    chef.add_recipe "openssl"

    chef.add_recipe "git"
    chef.add_recipe "npd-nload"
    chef.add_recipe "npd-siege"

    chef.add_recipe "nfs"
    #chef.add_recipe "gluster"
    chef.add_recipe "npd-glusterfs"

    #chef.add_recipe "mysql"
    chef.add_recipe "percona"
    chef.add_recipe "percona::package_repo"
    chef.add_recipe "percona::client"
    chef.add_recipe "percona::server"
    chef.add_recipe "percona::toolkit"

    chef.add_recipe "memcached"
    chef.add_recipe "npd-redis"
    #chef.add_recipe "redis::client"
    #chef.add_recipe "redis::default"
    #chef.add_recipe "redis::install_from_package"
    #chef.add_recipe "redis::install_from_release"
    #chef.add_recipe "redis::server"

    chef.add_recipe "npd-nginx"
    #chef.add_recipe "nginx"

    chef.add_recipe "php-fpm"
    chef.add_recipe "php"
    chef.add_recipe "npd-php"

    chef.add_recipe "java"
    chef.add_recipe "npd-solr-tomcat"
    #chef.add_recipe "apache2"
    
    chef.add_recipe "varnish"
    chef.add_recipe "haproxy"

    # Drupal App
#    chef.add_recipe "npd-drush"
    
    # Wordpress App

    # Symfony App

    chef.json = {
      # apt: {
      #   'compile_time_update' => true
      # },
      # memcached: {
      #   'memory' => '128'
      # },
      # java: {
      #   'install_flavor' => 'openjdk',
      #   'jdk_version' => '7'
      # },
      # haproxy: {
      #   'incoming_port' => '81'
      # }

      #apache: {
      #  default_site_enabled: true
      #},
      #mysql: {
      #  server_hostname: 'localhost',
      #  server_username: 'root',
      #  server_root_password: 'root'
      #}
    }
  end

  # Enable provisioning with chef server, specifying the chef server URL,
  # and the path to the validation key (relative to this Vagrantfile).
  #
  # The Opscode Platform uses HTTPS. Substitute your organization for
  # ORGNAME in the URL and validation key.
  #
  # If you have your own Chef Server, use the appropriate URL, which may be
  # HTTP instead of HTTPS depending on your configuration. Also change the
  # validation key to validation.pem.
  #
  # config.vm.provision "chef_client" do |chef|
  #   chef.chef_server_url = "https://api.opscode.com/organizations/ORGNAME"
  #   chef.validation_key_path = "ORGNAME-validator.pem"
  # end
  #
  # If you're using the Opscode platform, your validator client is
  # ORGNAME-validator, replacing ORGNAME with your organization name.
  #
  # If you have your own Chef Server, the default validation client name is
  # chef-validator, unless you changed the configuration.
  #
  #   chef.validation_client_name = "ORGNAME-validator"
end
