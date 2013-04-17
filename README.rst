Installation
============

To start a new project with this template::

    django-admin.py startproject --template=https://github.com/caktus/rapidsms-deploy-dotcloud/zipball/master --extension=py,rst <``project_name``>

.. Or to use a released version:
    django-admin.py startproject --template=https://github.com/rapidsms/rapidsms-project-template/zipball/release-0.13.0 --extension=py,rst <``project_name``>

``project_name``
========================

This project template is based off the standard `rapidsms-project-template
<https://github.com/rapidsms/rapidsms-project-template>`_. With the addition
of configuration and scripts aimed at deploying your project to dotCloud.

Below you will find basic setup instructions for the ``project_name``
project. To begin you should have the following applications installed on your
local development system:

- `Python >= 2.6 (2.7 recommended) <http://www.python.org/getit/>`_
- `pip >= 1.1 <http://www.pip-installer.org/>`_
- `virtualenv >= 1.8 <http://www.virtualenv.org/>`_

Local Setup
-----------

To setup your local environment you should create a virtualenv and install the
necessary requirements::

    virtualenv --distribute project_name-env

On Posix systems you can activate your environment like this::

    source project_name-env/bin/activate

On Windows, you'd use::

    project_name-env\Scripts\activate

Then::

    cd project_name
    pip install -r requirements/base.txt

Run syncdb::

    python manage.py syncdb

You should now be able to run the development server::

    python manage.py runserver

Deploying to dotCloud
---------------------

Configuring deployment to dotCloud involves downloading the relevant dotCloud
CLI interface tool and using it to authenticate, provision services, and push
your code up. The steps covered here are described in the `dotCloud First
Steps`_ documentation.

Install and configure the dotCloud CLI::

   pip install dotcloud 
   dotcloud setup

Create a new project::

   dotcloud create <project_name>

Deploy your project::

   dotcloud push

Description of dotCloud Deployment
----------------------------------

The services provisioned using this setup are: web application server, web
static media server, a postgres database, a redis queue server, and python
queue workers. The services provisioned are outlined in the ``dotcloud.yml``
file. The RapidSMS configuration included in this template is using the
`Database Router`_ and `Database Backend`_.  The overall configuration is
described in more depth in the `dotCloud First Steps`_ and `dotCloud Django`_
documentation.

Web Application and Static Media Server
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

dotCloud exposes the web server configuration through the ``wsgi.py`` and
``nginx.conf`` files. ``wsgi.py`` defines the application server settings there
is more information about this in the `Django with WSGI`_ and `dotCloud
Django`_ documenation. The ``nginx.conf`` file is the web server file used to
serve static media.  The paths in this file must match up with where the static
media lands when it is collected using the ``collectstatic`` management command
run by the ``postinstall`` script.

Database Server
^^^^^^^^^^^^^^^

The Postgres database service will be provisioned and the project's
``settings.py`` file reads a ``.json`` file created by dotCloud to find the
login credentials for the database server.

Queue Server and Task Workers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

dotCloud easily supports Redis services so we have chosen that as a Celery
broker for deploying on dotCloud. This is configured using a configuration
variable that dotCloud sets in a ``.json`` file and that we read in the
``settings.py`` file. More background about this is provided in the Celery
documentation in the `Celery First Steps with Django`_ and `Celery Using
Redis`_ sections as well as in the RapidSMS documentation on `Using Celery for
Scheduling Tasks`_.

The python workers are setup using a separate service. They are configured to
run the ``celeryd`` management command through ``supervisord``. For more
information about how this works, see the ``supervisord.conf`` file. There is
more documentation in the `Django and Celery on dotCloud`_ and `Background
Processes on dotCloud`_ documentation.

.. _dotCloud First Steps: http://docs.dotcloud.com/firststeps/
.. _dotCloud Django: http://docs.dotcloud.com/tutorials/python/django/
.. _Django and Celery on dotCloud: http://docs.dotcloud.com/tutorials/python/django-celery/
.. _Background Processes on dotCloud: http://docs.dotcloud.com/guides/daemons/
.. _Celery First Steps with Django: http://docs.celeryproject.org/en/latest/django/first-steps-with-django.html
.. _Celery Using Redis: http://docs.celeryproject.org/en/latest/getting-started/brokers/redis.html
.. _Django with WSGI: https://docs.djangoproject.com/en/dev/howto/deployment/wsgi/
.. _Using Celery for Scheduling Tasks: http://rapidsms.readthedocs.org/en/latest/topics/celery.html
.. _Database Backend: http://rapidsms.readthedocs.org/en/latest/topics/backends/database.html 
.. _Database Router: http://rapidsms.readthedocs.org/en/latest/topics/router/db.html
