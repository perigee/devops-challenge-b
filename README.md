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

## Requirements:

This repo uses Ansible to install and configure nginx to serve static content
from a directory.  The target host is expected to be running Ubuntu 14.04 (but
other Ubuntu versions will likely work).

We need to add a reverse proxy to the site, but *only* for our production
site.  When the code is deployed on a development or test site, the reverse
proxy should be disabled.  The location /meow/ on the production site should
proxy requests to http://placekitten.com/ .

## Usage instructions:

To deploy to a machine, run:

```ansible-playbook playbook.yml -i "HOST," --extra-vars="user=USER"```

Replace HOST by the hostname or IP address, and USER by the username (must have
sudo access).

For example, for a machine with IP `52.0.228.95` and a username of `ubuntu`:

```ansible-playbook playbook.yml -i "52.0.228.95," --extra-vars="user=ubuntu"```

## Resources:

* [Ansible Documentation](http://docs.ansible.com/)
* [nginx Documentation](http://nginx.org/en/docs/)
* [Amazon Web Services Free Tier](http://aws.amazon.com/free/) - one way to
  test your work (other ways are totally fine)
