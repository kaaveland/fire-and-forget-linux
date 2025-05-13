Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"

  config.vm.provision "ansible" do |ansible|
    # Run with `vagrant up; then vagrant provision`. Won't work if you use vaulted secrets!
    ansible.playbook = "initialize.yml"
  end
end