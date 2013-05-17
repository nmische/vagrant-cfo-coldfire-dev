# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"
  config.vm.network :private_network, ip: "192.168.33.33"
  config.vm.synced_folder ".", "/vagrant", :owner => "nobody"
  config.vm.provider :virtualbox do |vb|
    vb.customize [
      "modifyvm", :id,
      "--name", "ColdFireDevVM",
      "--memory", "1536"
    ]
  end

  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "cookbooks"
    chef.data_bags_path = "data_bags"
    chef.log_level = :debug

    chef.add_recipe "apt"
    chef.add_recipe "java"
    chef.add_recipe "ant"
    chef.add_recipe "git"
    chef.add_recipe "rng-tools"
    chef.add_recipe "apache2"
    chef.add_recipe "apache2::mod_ssl"
    chef.add_recipe "coldfusion10"
    chef.add_recipe "coldfusion10::apache"
    chef.add_recipe "coldfusion10::trustedcerts"
    chef.add_recipe "coldfusion10::configure"
    chef.add_recipe "railo334"
    chef.add_recipe "mxunit"
    chef.add_recipe "mysql::server"
    chef.add_recipe "coldfire-dev"

    chef.json = {
      "cf10" => {
        "installer" => {
          "local_file" => "/vagrant/ColdFusion_10_WWEJ_linux32.bin",
          "install_samples" => true
        },
        "config_settings" => {
          "runtime" => {
            "cacheProperty" => [ 
              {"propertyName" => "TrustedCache", "propertyValue" => false},
              {"propertyName" => "InRequestTemplateCache", "propertyValue" => false},
              {"propertyName" => "ComponentCache", "propertyValue" => false},
              {"propertyName" => "CacheRealPath", "propertyValue" => false},
              {"propertyName" => "SaveClassFiles", "propertyValue" => false}
            ]
          }
        }
      },
      ## Put your ColdFire repo settings here. ##
      #"coldfire-dev" => {
      #  "git" => {
      #    "repository" => "",
      #    "revision" => "",
      #    "deployment_key" => ""
      #  }
      #},
      ## # # # # # # # # # # # # # # # # # # # ##
      "java" => {
        "install_flavor" => "oracle",
        "java_home" => "/usr/lib/jvm/java-7-oracle",
        "jdk_version" => "7",
        "oracle" => {
          "accept_oracle_download_terms" => true
        }        
      },
      "mysql" => {
        "server_debian_password" => "vagrant",
        "server_root_password" => "vagrant",
        "server_repl_password" => "vagrant",
        "bind_address" => "0.0.0.0",
        "allow_remote_root" => true 
      }
    }
  end

end
