On this article we will setup a django project boilerplate that we can quickly use to develop and deploy django applications.  Among other things, we will  go beyond the default project layout structure that `django-admin startproject [project_name]` creates when we execute that command.

By doing that, we will be going after the following objectives:

1. Objective 1

2. etc.

### Summary

### Conventions throughout this article

Please review the conventions page, which will help you understand how I communicate with you through this blog.

Let's dive in.

Let's call our project `djangoboilerplate`.  Note: we will be using pythonanywhere.com to host the boilerplate project to make sure that it runs fine, and to test our deployment strategy; I made sure to grab the username already :)

This tutorial assumes...

1. virtual environment with python (I used 3.5.2 for this article) 

2. django has been installed (

3. mac and windows

Let's install our virtual environment and call it **_djangoboilerplate_** : `mkvirtualenv --python=python3 djangoboilerplate`. Once it runs, make sure that your newly created environment is activated (if not already activated) by executing `workon djangoboilerplate`. 

Now, let's install django by running `pip install django`.

Now go to the location where your boilerplate project will reside, and create a folder called `djangoboilerplate`. Going forward, we will refer to it as the `root folder`. Open a terminal window within the root folder, activate the `djangoboilerplate` virtual environment, and execute `django-admin startproject apps`. If successful, you would have a newly created folder called `apps` within your `djangoboilerplate` folder. 

Inside of it, you should see the following file structure:

    |____djangoboilerplate
    | |____apps
    | | |____apps
    | | | |______init__.py
    | | | |____settings.py
    | | | |____urls.py
    | | | |____wsgi.py
    | | |____manage.py

Let's run the django server to make sure that it is ready to roll by going into the top `apps` folder and executing `manage.py runserver`.  django will warn us about unapplied migrations (let's ignore that) but we should see the typical `Welcome to django!` page on our browser of choice if we point it to `http://127.0.0.1:8000/`.

![welcome to django page](/media/2017/welcome_to_jango.png "welcome to django page")

Let's go ahead and run `manage.py migrate` to get rid of django's warning about unapplied migrations. When we migrate, django adds a `db.sqlite3` file within the second tier `apps` folder, which is the file of the `sqlite` database that we will use on our boilerplate application.

Let's go back into the root folder, and let's initialize a `git` repository by executing `git init`.  By now you probably have a `.gitignore` file that you use on your projects.  If not, feel free to grab mine from here {{  link  }}. Go ahead and put a copy of the `.gitignore` file under the root folder. Our file structure should now look like the below structure.  Remember, per the conventions page {{ link }}, certain files (inlcuding git and python cache files, are omitted). 

    |____djangoboilerplate
    | |____apps
    | | |____apps
    | | | |______init__.py
    | | | |____settings.py
    | | | |____urls.py
    | | | |____wsgi.py
    | | |____db.sqlite3
    | | |____manage.py

Once we get the above we are ready to do our first commit, by executing the following:

1. 'git add .'
2. 'git status' - always verify what's being committed. In my case, I get the following output:

        Changes to be committed:
          (use "git rm --cached <file>..." to unstage)
        
        	new file:   .gitignore
        	new file:   apps/apps/__init__.py
        	new file:   apps/apps/settings.py
        	new file:   apps/apps/urls.py
        	new file:   apps/apps/wsgi.py
        	new file:   apps/manage.py
    	
3. `git commit -m "django initial setup"`.

Let's go ahead and add the following folders and files to the root folder:

    docs
        readme.md
    requirements
    
We can edit the above `readme.md` markdown file add the following `### This folder will contain project-wide documentation`. We'll leave it at that.

Now, on the command line go into the `requirements` folder and execute `pip freeze > requirements.txt`. A new file will be created there, listing the packages that we have installed so far by running the `mkvirtualenv --python=python3 djangoboilerplate` and `pip install django` commands.  The packages are:

	appdirs==1.4.3
	Django==1.11.1
	packaging==16.8
	pyparsing==2.2.0
	pytz==2017.2
	six==1.10.0

We will come back to the `requirements` later.

Our layout should now look like the following:

    |____djangoboilerplate
    | |____apps
    | | |____apps
    | | | |______init__.py
    | | | |____settings.py
    | | | |____urls.py
    | | | |____wsgi.py
    | | |____db.sqlite3
    | | |____manage.py
    | |____docs
    | | |____readme.md
    | |____requirements
    | | |____requirements.txt

Lets' commit the changes: `git add .` + `git status` (reminder: verify!) + `git commit -m "add docs and requirements folders and files".

Let's refactor the second tier `apps` folder and rename it as `config`, which is a more fitting name for that folder.

For the refactoring to work, we must search for the string `apps` within the files under the first tier `apps` folder, and replace the relevant instances with the word `config`.  PyCharm, my favorite python development IDE, has a very handy tool to do just that; a snippet of the instances that PyCharm finds is shown below.  You could also use `grep`, for instance.

![PyCharm refactoring results](/media/2017/pycharm_refactor_boilerplate_to_config.png "PyCharm refactoring results").

Once we swap `apps` for `config`, we run the server (i.e., execute `manage.py runserver`) to make sure that our refactoring works OK, and we get the typical `Welcome to django!` page.

Our layout at the moment should look as follows:

    |____djangoboilerplate
    | |____apps
    | | |____config
    | | | |______init__.py
    | | | |____settings.py
    | | | |____urls.py
    | | | |____wsgi.py
    | | |____db.sqlite3
    | | |____manage.py
    | |____docs
    | | |____readme.md
    | |____requirements
    | | |____requirements.txt

No more duplicate folders. Good bye confusion.

Let's commit and call it the commitment "rename second tier apps folder as config".

### The settings module
___________________

Next, let's create a folder called `settings` within the `config` folder, and add the files shown below.

    | | | |____settings
    | | | | |______init__.py
    | | | | |____base.py
    | | | | |____local.py
    | | | | |____production.py

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
    SECRET_KEY = 'ew9vz)1fm2vwhca^2lq5d-g0+g_27!pih!z6(97=@vb7vip-!n'
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

Now, how do we go about telling django which settings file to use?  For that, we will need to set an environment variable that we can use to programmatically instruct django to use the proper settings file.

We'll call our variable `DJANGO_EXECUTION_ENVIRONMENT`. On our local environment, its value will be `LOCAL` and on our deployment environment, it will be `PRODUCTION`.

For Mac OS (my development OS of choice), I just simply run `nano -w ~/.bash_profile` on the command line, and add the following line to my `.bash_profile` file:

    export DJANGO_EXECUTION_ENVIRONMENT="LOCAL"

Once I save the changes, I run `source ~/.bash_profile` on the command line, and then test that it worked by running `echo $DJANGO_EXECUTION_ENVIRONMENT`, the result of which, if successful, is `LOCAL`.

If you use a system other than Mac OS, switch ASAP! :) Nah, just kidding. A relevant google search will point you in the right direction.

We'll cover the deployment portion (i.e., how to set the environment variable in our production environment) when we get to it.

Now, let's tell django which settings file to use. Let's change the contents of our `manage.py` file from:

    import os
    import sys
    
    if __name__ == "__main__":
        os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings")
        try:
            from django.core.management import execute_from_command_line
        except ImportError:
            # The above import may fail for some other reason. Ensure that the
            # issue is really that Django is missing to avoid masking other
            # exceptions on Python 2.
            try:
                import django
            except ImportError:
                raise ImportError(
                    "Couldn't import Django. Are you sure it's installed and "
                    "available on your PYTHONPATH environment variable? Did you "
                    "forget to activate a virtual environment?"
                )
            raise
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

We added the function `get_env_variable`, which allows us to humanize the error message given by django if for some reason it can't read the environment variable. 

The below code block tells django which settings file to use, depending on the value of the `DJANGO_EXECUTION_ENVIRONMENT` variable.

    ...
    DJANGO_EXECUTION_ENVIRONMENT = get_env_variable('DJANGO_EXECUTION_ENVIRONMENT')
    if DJANGO_EXECUTION_ENVIRONMENT == 'LOCAL':
        os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.local")
    if DJANGO_EXECUTION_ENVIRONMENT == 'PRODUCTION':
        os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.production")
    ...    

We can safely move somewhere else (or delete) the `settings.py` file.

Our layout now should look as follows:

    |____djangoboilerplate
    | |____apps
    | | |____config
    | | | |______init__.py
    | | | |____db.sqlite3
    | | | |____settings
    | | | | |______init__.py
    | | | | |____base.py
    | | | | |____local.py
    | | | | |____production.py
    | | | |____urls.py
    | | | |____wsgi.py
    | | |____db.sqlite3
    | | |____manage.py
    | |____docs
    | | |____readme.md
    | |____requirements
    | | |____requirements.txt

If you notice, you will see that there are two `db.sqlite3` files.  django automatically dropped a `db.sqlite3` file as a result of our refactoring the `settings.py` file into  `settings + base.py | local.py | production.py`. We can safely delete the bottom one.

Let's run `manage.py runserver` to make sure that django is working OK. We should see the typical `Welcome to django!` page. We should also notice that django is telling us again that there are unapplied migrations.  Let's execute `.manage.py migrate` to get rid of that warning.

Let's commit and call the commitment "refactor settings.py into settings + base | local | production".

### django's SECRET_KEY
--------------------------------

One of the main reasons we are doing all this refactoring / re-arranging is security {{ expand on this }}.

We now know how to store variables in our environment. This time around, though, we will use a different procedure to accomplish that. We'll see why in a bit.

By now you should be using virtualenv wrapper (highly recommended!). By now I assume also that for every application you setup a different virtual environment (highly recommended too!). We will store django's sectret key in the virtual environment.  Go to the bin folder of your `virtualenv`.  For me, the path to the bin folder looks like `/Users/puma/.virtualenvs/djangoboilerplate/bin`.  Within it there is a file called `postactivate`; edit it as follows:

    #!/bin/bash
    # This hook is sourced after this virtualenv is activated.
    
    #your DJANGO_SECRET_KEY variable goes here
    export DJANGO_SECRET_KEY="YOUR_SECRET_KEY_GOES_HERE"

As the second line informs us, whatever is in that file will be sourced post activation of the virtual environment. Go ahead and copy the key that django generated for you when you executed `django-admin startproject djangoboilerplate`. {{ verify }}

The reason I use my virtual environment to store django's `SECRET_KEY` is that I make it different for each django application.  Hence, as I activate the app's virtual environment, I get the app's unique key.  As for the `DJANGO_EXECUTION_ENVIRONMENT` variable, its value (i.e., `LOCAL`) is always the same regardless of the application, and as such I store it in my system's `.bash_profile` and make it available system-wide.

Make sure that what you just did works by deactivating and reactivating your virtual environment, and then by running `echo $DJANGO_SECRET_KEY` on your command line. It should output your secret key.

Next, edit your `settings/base.py` file so it goes from

    import os
    ...
    SECRET_KEY = '_)2pb4uyb!v=_t#+9=i_))^8&-^ok_aa9mb_ptapty0l2olgdt'
    ...

to

    import os
    
    from manage import get_env_variable 
    # the above is the function that we added to manage.py
    # to humanize potential configuration errors
    
    DJANGO_SECRET_KEY = get_env_variable('DJANGO_SECRET_KEY')
    ...
    SECRET_KEY = DJANGO_SECRET_KEY
    ...

Let's run `manage.py runserver` to make sure that django is working OK. We should see the typical `Welcome to django!` page.

Let's commit and call the commitment "hide django's secret key".

There you have it. Your django's secret key is indeed secret now, and not out for the world to see when, for instance, you decide to upload your code to github :)

### Distinguishing between django, local, and third party applications
-----------------
Within the `base.py` file, let's refactor the block of `INSTALLED_APPS` codes as follows.

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
    ]
    INSTALLED_APPS = DJANGO_APPS + THIRD_PARTY_APPS + LOCAL_APPS

We have gained clarity and degree of separation between the different apps making up our boilerplate application.

As usual, let's test that django runs OK (we'll develop a test battery for our boilerplate app later). If it runs amoothly, let's commit and call our commitment "distinguish between django, local, and third party applications".



















### Setting up the main app
---
Every app has a part that acts as the main or core portion of the application that acts as the head that glues all apps together.  We will call this app the **_main_** app.  Some developers call it the **_core_** app.  I have stuck with **_main_** to avoid confusion with, for instance, the **_django.core_** library during module import operations. 

So, while under the apps folder in the terminal, let's run `manage.py startapp main`. Once it's done, our structure should look as follows.  Note that for clarity and brevity, I have omitted the `.git` related files and folders.  

	|____djangoboilerplate
	| |____djangoboilerplate
	| | |______init__.py
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


### Further refactoring our layout

This is what our layout looks like at the moment:

    |____djangoboilerplate
    | |____.init
    | |____config
    | | |____ (... config files)
    | |____db.sqlite3
    | |____main
    | | |____(... main  files)
    | |____manage.py
    | |____requirements.txt

Let's create a file within the root folder called `apps`. Let's move the folders `config` and `main`, as well as the file `manage.py` into the `apps` folder {{ need to fix the conventions entry and referring to names above }}.

Our layout now should look like:



### Requirements files

Along with having separate settings for our development and production environments, along with that it is a best practice to have separate requirements files to separate development from production packages.




Let's work on the `base.py` file now.















