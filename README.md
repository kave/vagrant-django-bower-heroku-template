vagrant-django-bower-heroku-template
=======

A template for new Django 1.7 projects developed under Vagrant. Features offered include:

* A Vagrantfile for building an Ubuntu Trusty based VM
* A virtualenv (configured to be active on login), with project dependencies managed through a requirements.txt file
* A PostgreSQL database (with the same name as the project, pre-configured in the project settings file)
* Separation of configuration settings into base.py, dev.py and production.py (and optionally local.py, kept outside
  of version control) as per http://www.sparklewise.com/django-settings-for-production-and-development-best-practices/
* Django-devserver, django-pipeline, django-debug-toolbar, Bower Integration, Heroku Deployment Configurations, Production AWS S3 File Serving Configurations, Production CSS/JS Compression out of the box
* A boilerplate base template with jquery included, and various other ideas and best practices borrowed from https://github.com/h5bp/html5-boilerplate

Setup
------------
Install Django 1.7 on your host machine. (Be sure to explicitly uninstall earlier versions first, or use a virtualenv -
having earlier versions around seems to cause pre-1.4-style settings.py and urls.py files to be generated alongside the
new ones.)

To start a new project with Vagrant, run the following commands:

    django-admin.py startproject --template https://github.com/kave/vagrant-django-bower-heroku-template/zipball/master --name=Vagrantfile myproject
    cd myproject
    vagrant up
    vagrant ssh
      (then, within the SSH session:)
    ./manage.py runserver 0.0.0.0:8000
    
To start a new project without Vagrant, run the following commands:

    django-admin.py startproject --template https://github.com/kave/vagrant-django-bower-heroku-template/zipball/master --name=Vagrantfile myproject
    cd myproject
    mv $PROJECT_DIR/_heroku $PROJECT_DIR/.heroku    

This will make the app accessible on the host machine as http://localhost:8111/ . The codebase is located on the host
machine, exported to the VM as a shared folder; code editing and Git operations will generally be done on the host.


Heroku Deployment (After [Git Initialization](http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository))
-----
* `heroku create {appName}`
* `heroku config:add BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git`
* `git push heroku master`
* `heroku addons:add heroku-postgresql`
* `heroku ps:scale web=1`
* `heroku run python manage.py syncdb`

See also
--------
[vagrant-django-base](https://github.com/torchbox/vagrant-django-base)- a recipe for a Vagrant base box that can be used in place of precise32
in the Vagrantfile - this has more of the server setup baked in, so that we can save time by not having to re-run those
steps every time we create a new VM instance.
