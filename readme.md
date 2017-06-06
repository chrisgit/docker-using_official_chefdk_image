## Running the ChefDK from a Docker Container

Using ChefDK from a Docker container has the following advantages
* Nothing to install
* Should work out of the box
* Consistency for everyone using the tool

### Requirements
This example requires

VirtualBox https://www.virtualbox.org/
- Needed to download and run an Ubuntu box for the docker container to run in.
- If you already have a Docker compatible O/S then install Docker and pull the ChefDK image
  - https://docs.docker.com/engine/installation/
  - https://hub.docker.com/r/chef/chefdk/

Vagrant https://www.vagrantup.com/
- Great tool to configure settings and control VirtualBox VM's

### Usage

If using Vagrant/VirtualBox then download or clone this git repo, in the root of this demo type

````
vagrant up
````

This will download an ubuntu box from Hashicorp Atlas, install docker and pull the ChefDK image.

When the box is ready login to the box with

````
vagrant ssh
````

### ChefDK in a container

Once logged in to the VirtualBox image go to the /vagrant/cookbooks folder.

Create an alias to the ChefDK docker image 

````
alias chef='docker run -it --rm -v $(pwd):/chef-repo chef/chefdk'
````

This alias will allow you to run the ChefDK container with
* alias of chef for running docker container
* map a drive (-v parameter) to your repo (or cookbook)
* remove the container when your command has finished executing

Use ChefDK from the container, examples below

Run Rubocop against the demo cookbook (from https://github.com/learn-chef)

````
chef rubocop /chef-repo
````

OR run rubocop with the ruleset that suits Chef cookbooks

````
chef rubocop -r cookstyle /chef-repo
````

Run Foodcritic against the demo cookbook

````
chef foodcritic . -f any /chef-repo
````

For more complicated scenario's you can run multiple bash commands together, including installing gems

````
chef /bin/bash -c "cd chef-repo/cookbooks/test_cookbook;rspec --format documentation --out rspec.txt"
````

````
chef /bin/bash -c "gem install rubocop-checkstyle_formatter;cd /chef-repo/cookbooks/test_cookbook;rubocop -r cookstyle --fail-level E --require rubocop/formatter/checkstyle_formatter --format RuboCop::Formatter::CheckstyleFormatter --out checkstyle.xml"
````

````
chef /bin/bash -c "cd chef-repo/cookbooks/test_cookbook;kitchen test"
````

### Test Kitchen run

To perform a full test kitchen run you will need to log in to the Docker container, change directories to the chef-repo folder.

1. Run the ChefDK docker container

````
chef
````

This will take you into the docker container with ChefDK

2. Change directory to the chef-repo, then change directory to the cookbook folder, run your test kitchen commands (example uses EC2 and comes from https://learn.chef.io/tutorials/local-development/rhel/aws/)

````
cd /chef-repo
cd learn_chef_httpd
kitchen list
kitchen converge
````

Remember to run kitchen destroy before exiting your docker container!
