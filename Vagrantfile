# -*- mode: ruby -*-
  # vi: set ft=ruby ts=2 sw=2 tw=0 et :
# bump your IPs and include version when adding new boxes
# https://app.vagrantup.com/debian

  role = File.basename(File.expand_path(File.dirname(__FILE__)))
  gui = true

  boxes = [
    {
      :name => "jessie",
      :distribution => "debian",
      :box => "debian/jessie64",
      :version => "9.4.0",
      :ip => '10.0.0.11',
      :cpu => "50",
      :disk_size => "10GB",
      :ram => "256"
    },
    {
      :name => "disco",
      :distribution => "ubuntu",
      :box => "ubuntu/disco64",
      :version => "20170427.0.0",
      :ip => '10.0.0.21',
      :cpu => "50",
      :disk_size => "10GB",
      :ram => "256"
    },

    {
      :name => "xenial",
      :distribution => "ubuntu",
      :box => "ubuntu/xenial64",
      :version => "20180522.0.0",
      :ip => '10.0.0.23',
      :cpu => "50",
      :disk_size => "10GB",
      :ram => "256"
    },

    {
      :name => "centos7",
      :distribution => "redhat",
      :box => "centos/7",
      :version => "1804.02",
      :ip => '10.0.0.32',
      :cpu => "50",
      :disk_size => "10GB",
      :ram => "256"
    },
  ]

  Vagrant.configure("2") do |config|
    boxes.each do |box|
#      config.vbguest.auto_update = true
#     https://github.com/dotless-de/vagrant-vbguest
#     run something like this:
#     mkdir ~/resources/sw/ubuntu/16.04/virtualbox/5.2.16
#     wget http://download.virtualbox.org/virtualbox/5.2.16/VBoxGuestAdditions_5.2.16.iso
#     then the config.vbguest.iso_path below will work:
#
#       config.vbguest.iso_path = "#{ENV['HOME']}/resources/sw/ubuntu/16.04/virtualbox/%{version}/VBoxGuestAdditions_%{version}.iso"
#      config.vbguest.iso_upload_path = "/tmp"
      config.vm.define box[:name] do |vms|
        vms.vm.box = box[:box]
        vms.disksize.size = box[:disk_size]
        vms.vm.box_version = box[:version]
        vms.vm.guest = box[:distribution]
        vms.vm.hostname = box[:name]
  #      vms.vm.synced_folder ".vagrant/synced", "/home/vagrant"


        config.vm.provider :libvirt do |libvirt|
            libvirt.cpus = 2
            libvirt.cputopology :sockets => '2', :cores => '2', :threads => '1'
        end

        vms.vm.network :private_network, ip: box[:ip]

#        vms.vm.provision "shell",
#          inline: "echo 'up and running'"

        vms.vm.provision "vai" do |ansible|
          ansible.inventory_dir='tests/vagrant'
          ansible.inventory_filename='generated_inventory'
          # optional: add a group listing all vagrant machines
          ansible.groups = {
            'debian' => ["jessie"],
            'ubuntu' => ["xenial", "disco"],
            'redhat' => ["centos7"],
          #  '_provided_by_vagrant_'=> HOSTS.keys,
          }
        vms.vm.provision "shell",
          inline: "echo 'system booted'"
#        end
        vms.vm.provision :ansible do |ansible|
          ansible.playbook = "tests/vagrant/site.yml"
          ansible.verbose = "vv"
        end
      end
    end
  end
end
