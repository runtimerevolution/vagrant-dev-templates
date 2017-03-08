# Vagrant Dev Templates

## Description

This repository purpose is to provide ready to use vagrant templates to provision development machines.

## Requirements

* [VirtualBox](https://www.virtualbox.org)
* [Vagrant](https://www.vagrantup.com)
* [ChefDK](https://downloads.chef.io/chefdk)

* Vagrant Plugins
  * [vagrant-berkshelf](https://github.com/berkshelf/vagrant-berkshelf)
  * [vagrant-omnibus](https://github.com/chef/vagrant-omnibus)
  * [nugrant](https://github.com/maoueh/nugrant) - Optional
  * [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) - Optional

## Usage

Copy the Berksfile, Berksfile.lock and the Vagrantfile to your project folder.
Edit the Vagrantfile and uncomment the lines to install the software that you need for your project.
You should also check the \*\_config hashes to change versions and other configurations.

After that just run ```$ vagrant up``` to start the development machine and let it run (First time it will take longer)
Here's the list of commands most widely used:
* vagrant up - Starts the development machine
* vagrant halt - Stops the development machine
* vagrant reload - Restarts the development machine
* vagrant ssh - Gets in the development machine.

## Roadmap

* Simplify configuration
* Add specific use case examples (e.g: Heroku)
* Provide configurations for other popular software

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/runtimerevolution/vagrant-dev-templates.
