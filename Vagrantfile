# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

require 'yaml'
CONFIG = YAML.load_file(File.expand_path("../box.yml", __FILE__))

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  if CONFIG['provider'] == 'virtual_box'
    config.vm.box = "virtual_box"
    config.vm.box_url = CONFIG["box_url"]

    # Disable automatic box update checking. If you disable this, then
    # boxes will only be checked for updates when the user runs
    # `vagrant box outdated`. This is not recommended.
    # config.vm.box_check_update = false

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    config.vm.network "forwarded_port", guest: 3000, host: 3000

    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    config.vm.network "private_network", ip: CONFIG["ip"]

    # Create a public network, which generally matched to bridged network.
    # Bridged networks make the machine appear as another physical device on
    # your network.
    # config.vm.network "public_network"

    # If true, then any SSH connections made will enable agent forwarding.
    # Default value: false
    # config.ssh.forward_agent = true
    # config.ssh.private_key_path = "~/.ssh/id_rsa"

    # Share an additional folder to the guest VM. The first argument is
    # the path on the host to the actual folder. The second argument is
    # the path on the guest to mount the folder. And the optional third
    # argument is a set of non-required options.
    config.vm.synced_folder "mnt", "/vagrant"

    # Provider-specific configuration so you can fine-tune various
    # backing providers for Vagrant. These expose provider-specific options.
    # Example for VirtualBox:
    #
    config.vm.provider "virtualbox" do |vb|
    #   # Don't boot with headless mode
    #   vb.gui = true
    #
    #   # Use VBoxManage to customize the VM. For example to change memory:
      vb.customize ["modifyvm", :id, "--memory", CONFIG["memory"]]
      vb.customize ["modifyvm", :id, "--cpus", CONFIG["cpus"]]
    end
    #
    # View the documentation for the provider you're using for more
    # information on available options.

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
    # config.vm.provision "puppet" do |puppet|
    #   puppet.manifests_path = "manifests"
    #   puppet.manifest_file  = "site.pp"
    # end

    # Enable provisioning with chef solo, specifying a cookbooks path, roles
    # path, and data_bags path (all relative to this Vagrantfile), and adding
    # some recipes and/or roles.
    #
    # config.vm.provision "chef_solo" do |chef|
    #   chef.cookbooks_path = "../my-recipes/cookbooks"
    #   chef.roles_path = "../my-recipes/roles"
    #   chef.data_bags_path = "../my-recipes/data_bags"
    #   chef.add_recipe "mysql"
    #   chef.add_role "web"
    #
    #   # You may also specify custom JSON attributes:
    #   chef.json = { mysql_password: "foo" }
    # end

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
  elsif CONFIG['provider'] == 'digital_ocean'
    config.vm.hostname = 'dev-box'

    config.vm.provider :digital_ocean do |provider, override|
      override.ssh.private_key_path = CONFIG['private_key_path']
      override.vm.box = 'digital_ocean'
      override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"

      provider.ssh_key_name = CONFIG['key_name']
      provider.token = CONFIG['token']
      provider.image = 'ubuntu-14-04-x64'
      provider.region = CONFIG['region']
      provider.size = CONFIG['size']

      config.vm.synced_folder "mnt", "/vagrant", type: "rsync", rsync_exclude: ".git/"
    end
  end
end
