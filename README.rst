Installation
------------

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

.. _Django and Celery on dotCloud: http://docs.dotcloud.com/tutorials/python/django-celery/
.. _Background Processes on dotCloud: http://docs.dotcloud.com/guides/daemons/
.. _Celery First Steps with Django: http://docs.celeryproject.org/en/latest/django/first-steps-with-django.html
.. _Celery Using Redis: http://docs.celeryproject.org/en/latest/getting-started/brokers/redis.html
.. _Database Backend: http://www.rapidsms.org/en/latest/topics/backends/database.html
