On this article we will setup a django project boilerplate that we can quickly use to develop and deploy django applications.  Among other things, we will  go beyond the default project layout structure that `django-admin startproject [project_name]` creates when we execute that command.

By doing that, we will be going after the following objectives:

1. Objective 1

2. etc.

### Summary

Let's dive in.

Let's call our project `djangoboilerplate`.  Note: we will be using pythonanywhere.com to host the boilerplate project to make sure that it runs fine, and to test our deployment strategy; I made sure to grab the username already :)

This tutorial assumes...

1. virtual environment with python (I used 3.5.2 for this article) 

2. django has been installed (

3. mac and windows

Let's install our virtual environment and call it **_djangoboilerplate_** : `mkvirtualenv --python=python3 djangoboilerplate`. Once it runs, make sure that your newly created environment is activated (if not already activated) by executing `workon djangoboilerplate`. 

Now, let's install django by running `pip install django`.

let's run `pip freeze` to see what packages we have installed so far by running the `mkvirtualenv --python=python3 djangoboilerplate` and `pip freeze` commands.

Here is the result:

	appdirs==1.4.3
	Django==1.11.1
	packaging==16.8
	pyparsing==2.2.0
	pytz==2017.2
	six==1.10.0

Don't lose sight of the above, for we will need it later.

Now go to the location where your boilerplate project will reside, and execute `django-admin startproject djangoboilerplate`. If successful, you would have a newly created folder in your preferred location called **_djangoboilerplate_**. Inside of it, you should see the following file structure:

	|____djangoboilerplate
	| |____djangoboilerplate
	| | |______init__.py
	| | |____settings.py
	| | |____urls.py
	| | |____wsgi.py
	| |____manage.py

Let's call the top folder (i.e., folder `|____djangoboilerplate`) the root folder. 

Let's go into the root folder, and let's initialize a `git` repository by executing `git init`.  Our file structure should now look like the below:

	|____.git
	| | |____ (... a bunch of folders and files created by git)
	|____djangoboilerplate
	| |______init__.py
	| |____settings.py
	| |____urls.py
	| |____wsgi.py
	|____manage.py

By now you probably have a `.gitignore` file that you use on your projects.  If not, feel free to grab mine from here {{  link  }}. Go ahead and put a copy of the `.gitignore` file under the root folder.

Let's execute `pip freeze > requirements.txt` to dump all the project requirements into the `requirements.txt` file.

We are ready to do our first commit.  Let's execute `git add .` to add all the relevant files. If we run `git status` we should see the following:

	new file:   .gitignore
	new file:   djangoboilerplate/__init__.py
	new file:   djangoboilerplate/settings.py
	new file:   djangoboilerplate/urls.py
	new file:   djangoboilerplate/wsgi.py
	new file:   manage.py
	new file:   requirements.txt

Once we get the above we are ready to do our first commit: `git commit -m "django initial setup"`.

### Setting up the main app
---
Every app has a part that acts as the main or core portion of the application that acts as the head that glues all apps together.  We will call this app the **_main_** app.  Some people call it the **_core_** app.  I have stuck with **_main_** to avoid confusion with, for instance, the **_django.core_** library during module import operations. Note that we would normally add the **_main_** app to the list of `INSTALLED_APPS` in the `settings.py` file.  We'll touch on that later. 

So, while under the root folder, let's run `manage.py startapp main`. Once it's done, our structure should look as follows.  Note that for clarity and brevity, I have omitted the `.git` related files and folders.  

	|____djangoboilerplate
	| |____djangoboilerplate
	| | |______init__.py
	| | |______pycache__
	| | | |______init__.cpython-35.pyc
	| | | |____settings.cpython-35.pyc
	| | |____settings.py
	| | |____urls.py
	| | |____wsgi.py
	| |____main
	| | |______init__.py
	| | |____admin.py
	| | |____apps.py
	| | |____migrations
	| | | |______init__.py
	| | |____models.py
	| | |____tests.py
	| | |____views.py
	| |____manage.py
	| |____requirements.txt

Let's run the django server to make sure that it is ready to roll by running `manage.py runserver`.  django will warn us about unapplied migrations (let's ignore that) but we should see the typical `Welcome to django!` page on our browser of choice if we point it to `http://127.0.0.1:8000/`.

![welcome to django page](/media/2017/welcome_to_jango.png "welcome to django page")

Let's go ahead and run `manage.py migrate` to get rid of django's warning about unapplied migrations.

Let's run `git commit` to capture our newly created **_main_** app related files. Below is the result of running `git add .` and then `git status`:

	new file:   main/__init__.py
	new file:   main/admin.py
	new file:   main/apps.py
	new file:   main/migrations/__init__.py
	new file:   main/models.py
	new file:   main/tests.py
	new file:   main/views.py

Let's go ahead and commit: `git commit -m "add main app"`.

What we have done so far is pretty standard boilerplate django.  We will now get into tailoring the structure and the setup to make it more secure and deployment friendlier.

### Our target structure
---
Once we are done with the reconfiguration, we should have a structure that resembles the below:

	placeholder for resulting structure

### Workflow
- - - - - -

### django configuration

Let's refactor the **_djangoboilerplate_** folder and rename it as **_config_**. 

Here is where an IDE is really helpful.  The IDE could tell you in advance the places that the refactoring would affect, and would suggest the edits that need to take place.  Without one, if I went ahead and renamed the folder, I would get errors like `ImportError: No module named 'djangoboilerplate'` because django would be looking for info that it wouldn't be able to find.

PyCharm, my preferred IDE, shows the following necessary changes for the refactoring to work:

![PyCharm refactoring results](/media/2017/pycharm_refactor_boilerplate_to_config.png "PyCharm refactoring results").

Let's go ahead and make the necessary changes.  Confirm that running `manage.py runserver` executes OK.

The structure before:

	|____djangoboilerplate
	| |____djangoboilerplate
	| | |______init__.py
	| | |____settings.py
	| | |____urls.py
	| | |____wsgi.py
	| |____(... other files and folders ...) 

The structure after:

	|____djangoboilerplate
	| |____config
	| | |______init__.py
	| | |____settings.py
	| | |____urls.py
	| | |____wsgi.py
	| |____(... other files and folders ...) 

Note that for brevity and clarity I have omitted the `pycache__` and `.pyc` folder and files. What have we gained? Clarity, for one.  No more confusion with folders of the same name!

Let's go ahead and commit the changes; call this commit as "setup config module".

Next, let's create a folder called **_settings_** within the config folder, and add the files shown below.

    | |____settings
    | | |______init__.py
    | | |____base.py
    | | |____local.py
    | | |____production.py

As you may be aware, the `__init__.py` flags the content of the folder `config` as a module, and will be treated as such by python.

`base.py` will host the common denominator settings for both our development (`local.py`) and production (`production.py`) environments, with `base.py` and `production.py` extending `base.py` settings to meet the needs of each. You may also consider having a `staging.py` file with settings specific to an environment where you do your staging / testing right before going live.  We won't touch on that here. 

We can now go through the contents of the `settings.py` file generated by django and ask ourselves, for each line:

    Is this setting needed for both development and production? 
        Yes:
            It goes into base.py
        Else:
            Is it needed for development only?
            Yes:
                It goes into local.py
            Else:
                It goes into production.py

Below are the listings of the resulting `base.py`, `local.py`, and `production.py` files resulting from following the logic I just described.

`base.py`:

    import os
    
    BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    
    SECRET_KEY = '_)2pb4uyb!v=_t#+9=i_))^8&-^ok_aa9mb_ptapty0l2olgdt'
    
    INSTALLED_APPS = (
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
    )
    
    MIDDLEWARE_CLASSES = (
        'django.contrib.sessions.middleware.SessionMiddleware',
        'django.middleware.common.CommonMiddleware',
        'django.middleware.csrf.CsrfViewMiddleware',
        'django.contrib.auth.middleware.AuthenticationMiddleware',
        'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
        'django.contrib.messages.middleware.MessageMiddleware',
        'django.middleware.clickjacking.XFrameOptionsMiddleware',
        'django.middleware.security.SecurityMiddleware',
    )
    
    ROOT_URLCONF = 'config.urls'
    
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
    
    WSGI_APPLICATION = 'config.wsgi.application'
    
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }
    
    LANGUAGE_CODE = 'en-us'
    TIME_ZONE = 'UTC'
    USE_I18N = True
    USE_L10N = True
    USE_TZ = True
    
    STATIC_URL = '/static/'

`local.py`:

    from .base import *
    DEBUG = True

`production.py`:

    from .base import *
    ALLOWED_HOSTS = []
    
Not many changes at the moment, but we have attained a degree of separation between development and production, which we will now expand. Note the `from .base import *` line, which is not part of the original `settings.py` file.  We need it to extend the `base.py` settings into `local.py` and `production.py`.

Within the `settings.py` file, let's refactor the block of `INSTALLED_APPS` codes as follows. While at it, let's add our `main` app to our list of `LOCAL_APPS`.

**Before**:

    INSTALLED_APPS = (
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
    )

**After**:

    DJANGO_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
    ]
    THIRD_PARTY_APPS = [
    ]
    LOCAL_APPS = [
        'main',
    ]
    INSTALLED_APPS = DJANGO_APPS + THIRD_PARTY_APPS + LOCAL_APPS

We have gained clarity and degree of separation between the different apps making up our boilerplate application.

Now, how do we go about telling django which settings file to use?  For that, we will need to set an environment variable that we can use to programmatically instruct django to use the proper settings file.

We'll call our variable `DJANGO_EXECUTION_ENVIRONMENT`. On our local environment, its value will be `LOCAL` and on our deployment environment, it will be `PRODUCTION`.

For Mac OS (my development OS of choice), I just simply run `nano -w ~/.bash_profile` on the command line, and add the following line to my `.bash_profile` file:

    export DJANGO_EXECUTION_ENVIRONMENT="LOCAL"

Once I save the changes, I run `source ~/.bash_profile` on the command line, and then test that it worked by running `echo $DJANGO_EXECUTION_ENVIRONMENT`, the result of which, if successful, is `LOCAL`. {{ expand / test storing the variable in the virtualenv wrapper }}

If you use a system other than Mac OS, switch ASAP! :) Nah, just kidding. A relevant google search will point you in the right direction.

We'll cover the deployment portion (i.e., how to set the environment variable in our production environment) when we get to it.

Now, let's tell django which settings file to use. Let's change the contents of our `manage.py` file from:

    import os
    import sys
    
    
    if __name__ == "__main__":
        os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
        from django.core.management import execute_from_command_line
        execute_from_command_line(sys.argv)

to:

    import os
    import sys
    
    
    from django.core.exceptions import ImproperlyConfigured
    
    
    def get_env_variable(var_name):
        try:
            return os.environ[var_name]
        except KeyError:
            error_msg = "Set the {} environment variable".format(var_name)
            raise ImproperlyConfigured(error_msg)
    
    
    if __name__ == "__main__":
        DJANGO_EXECUTION_ENVIRONMENT = get_env_variable('DJANGO_EXECUTION_ENVIRONMENT')
        if DJANGO_EXECUTION_ENVIRONMENT == 'LOCAL':
            os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.local")
        if DJANGO_EXECUTION_ENVIRONMENT == 'PRODUCTION':
            os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.production")
        from django.core.management import execute_from_command_line
        execute_from_command_line(sys.argv)

We added the function `get_env_variable` allows us to humanize the error message given by django if for some reason it can't read the environment variable. 

The below code block tells django which settings file to use, depending on the value of the `DJANGO_EXECUTION_ENVIRONMENT` variable.

    ...
    DJANGO_EXECUTION_ENVIRONMENT = get_env_variable('DJANGO_EXECUTION_ENVIRONMENT')
    if DJANGO_EXECUTION_ENVIRONMENT == 'LOCAL':
        os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.local")
    if DJANGO_EXECUTION_ENVIRONMENT == 'PRODUCTION':
        os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.production")
    ...    

Let's run `manage.py runserver` to make sure that django is working OK. We should see the typical `Welcome to django!` page.

It's time to run `git add .` + `git status` (always double check what's being committed) + `git commit -m "refactor settings (base + local | production)"`.

### django's SECRET_KEY

One of the main reasons we are doing all this refactoring / re-arranging is security {{ expand on this }}.

Well, we now know how to store variables in our environment. This time around, though, we will use a different procedure to accomplish that. We'll see why in a bit.

By now you should be using virtualenv wrapper (highly recommended!). By now I assume also that for every application you setup a different virtual environment (highly recommended too!). Go to the bin folder of your `virtualenv` and edit the `postactivate` file as follows:

    #!/bin/bash
    # This hook is sourced after this virtualenv is activated.
    
    #your DJANGO_SECRET_KEY variable goes here
    export DJANGO_SECRET_KEY="_)2pb4uyb!v=_t#+9=i_))^8&-^ok_aa9mb_ptapty0l2olgdt"

As the second line informs us, whatever is in that file will be sourced post activation of the virtual environment. Go ahead and copy the key that django generated for you when you executed `django-admin startproject djangoboilerplate`. {{ verify }}

Let's store one for `SECRET_KEY` by following the steps we took before {{ link to the section? }}. 





Let's work on the `base.py` file now.















