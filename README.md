# devops-challenge
A small coding challenge for our open devops position.  This is intended to be completed independently, without consulting other developers or any Plotly employees.  Consulting online references is fine!  If you're really unsure about something, make an assumption and state it when you submit the pull request.

## Basic steps to complete this challenge:

1. Fork this repo.
2. Create and test your solution.
3. Update this README to explain how to deploy to various environments with
   this solution.
4. Open a pull request on our repo.  Explain how you've tested it, and what further testing you feel
   would be needed before putting this into production.  (We realize you may
   not have the servers set up to test this as much as you'd like in a
   reasonable amount of time.)


### Design principles 

1. For the sack of simplicity, I only adopt a "naive" approach to finish this project without fancy `group_vars` etc., and also did concern the different linux distributions. 
2. In order to minimize the duplication inside role under the scalability concern, use template file to combine the settings for different production/staging settings (probably immature) 
3. Use production/staging as key words to distinguish the production environment from dev/testing environment. 
4. Inside nginx role, 
   - add `vars/production.yml` file to override the parameters settings for staging
   - nginx.conf.j2: use condiction clause to seperate the production/staging configuration
   - in tasks, use `production` key word to add specific operations for production environment



### Current test strategies

1. create dummy localhost for syntax checking

   ```
   $ ansible-playbook --syntax-check --list-tasks -i tests/ansible_hosts playbook.yml
   ```

2. Add service testing inside playbook (see inside playbook.yml)


3. Add the uri testing in `tests.yml`, for running test:

   ```
   $ ansible-playbook -i hosts tests.yml
   ```

### Further test strategies (Not yet implemented)

1. Run system testing on staging hosts
2. Add variables under role nginx for centos/redhat deployment
3. Build security testing for production hosts



## Requirements:

This repo uses Ansible to install and configure nginx to serve static content
from a directory.  The target host is expected to be running Ubuntu 14.04 (but
other Ubuntu versions will likely work).

We need to add a reverse proxy to the site, but *only* for our production
site.  When the code is deployed on a development or test site, the reverse
proxy should be disabled.  The location /meow/ on the production site should
proxy requests to http://placekitten.com/ .

## Usage instructions:


### Define your hosts information

In the `hosts` inventory file, put your production hosts under the `[production]` section, and your staging hosts uder the `[staging]` section. 


### Run the deployment procedure

To deploy a production machine, run:

```
$ ansible-playbook --ask-become-pass -i hosts playbook.yml --extra-vars "stage=production"

```

To deploy a staging (dev/test) machine, run:

```
$ ansible-playbook --ask-become-pass -i hosts playbook.yml --extra-vars "stage=staging"

```

### Modify the nginx roles for different environment

1. All basic parameters are set in `roles/nginx/defaults\main.yml`
2. The specific production settings are in `roles/nginx/vars/production.yml`
3. Inside `roles/nginx/templates/nginx.conf.j2`, using `nginx_production` to distinguish the production settings from staging 


## Resources:


* [geerlingguy asible nginx role](https://github.com/geerlingguy/ansible-role-nginx/blob/master/templates/nginx.conf.j2)
* [How to use Ansible for Vagrant and production](http://future500.nl/articles/2014/05/how-to-use-ansible-for-vagrant-and-production/)
* [Ansible Documentation](http://docs.ansible.com/)
* [nginx Documentation](http://nginx.org/en/docs/)
* [Amazon Web Services Free Tier](http://aws.amazon.com/free/) - one way to
  test your work (other ways are totally fine)
