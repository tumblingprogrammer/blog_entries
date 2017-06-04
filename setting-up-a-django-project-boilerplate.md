### Introduction
_____

Through this tutorial we will setup a django project boilerplate that we can quickly use to develop and deploy django applications.  Among other things, we will  go beyond the default project layout structure that `django-admin startproject [project_name]` creates when we execute that command.  the objective is to develop a layout that is ready for deployment, with security and a clear separation of configuration / settings for development and production environments.

The source code and the files needed to complete the tutorial can be found @ [github](https://github.com/tumblingprogrammer/djangoboilerplate "tumbling programmer's django project boilerplate git repository").

This tutorial follows the best practices laid out on the book [two scoops of django](https://www.twoscoopspress.com/products/two-scoops-of-django-1-8), [the django project's documentation](https://docs.djangoproject.com/en/1.11/), as well my own experience in dealing with the subject.

### Conventions throughout this tutorial
_____

Please review the [conventions page](http://www.tumblingprogrammer.com/conventions-used-on-tumbling-programmer-dot-com/ "tumbling programmer's conventions page"), which will help you understand how I communicate with you through this blog.

Let's dive in.

### Assumptions
_____

This tutorial assumes that you have at least completed the [django turorial](https://docs.djangoproject.com/en/1.11/intro/tutorial01/), and are familiar with the following:

1. Installing python.
2. Setting up a virtual environment, and more spicifically, [virtualenvwrapper](http://virtualenvwrapper.readthedocs.io/en/latest/install.html).
3. Running commands from the CLI (command line interface), or the terminal; more specifically, throughout this tutorial, when I ask you to execute `manage.py runserver` on the terminal, please be aware that there are several ways to do that, depending on how your environment is setup; for example, in my system (Mac OS), I have it setup so I only have to type `./manage.py runserver`; you may have to type `python manage.py runserver` on yours.
4. Installing and working with the [git](https://git-scm.com/) distributed version control system (or similar). 

### Installing our virtual environment and django
_____

Let's install our virtual environment and call it `djangoboilerplate` : `mkvirtualenv --python=python3.5.2 djangoboilerplate`. Once it runs, make sure that your newly created environment is activated (if not already activated) by executing `workon djangoboilerplate`. 

Now, let's install django by running `pip install django=1.11.1`.

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

![welcome to django page](https://www.tumblingprogrammer.com/media/2017/06/welcome_to_django.png "welcome to django page")

Let's go ahead and run `manage.py migrate` to get rid of django's warning about unapplied migrations. When we migrate, django adds a `db.sqlite3` file within the second tier `apps` folder, which is the file of the `sqlite` database that we will use on our boilerplate application.

Let's go back into the root folder, and let's initialize a `git` repository by executing `git init`.  By now you probably have a `.gitignore` file that you use on your projects.  If not, feel free to grab mine from [here](https://www.tumblingprogrammer.com/media/global/.gitignore "tumbling programmer's .gitignore file"). Go ahead and put a copy of the `.gitignore` file under the root folder. Our file structure should now look like the below structure.  Remember, per the [conventions page](http://www.tumblingprogrammer.com/blog/edit-entry/conventions-used-in-tumbling-programmer-dot-com/ "tumbling programmer's conventions page"), certain files (including git and python cache files, are omitted). 

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
    
Let's edit the above `readme.md` markdown file add the following `### This folder will contain project-wide documentation`. We'll leave it at that.

Now, on the command line go into the `requirements` folder and execute `pip freeze > requirements.txt`. A new file will be created there, listing the packages that we have installed when we ran `mkvirtualenv --python=python3.5.2 djangoboilerplate` and `pip install django=1.11.1` commands.  The packages are:

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

Let's commit the changes: `git add .` + `git status` (reminder: verify!) + `git commit -m "add docs and requirements folders and files"`.

Let's refactor the second tier `apps` folder and rename it as `config`, which is a more fitting name for that folder.

For the refactoring to work, we must search for the string `apps` within the files under the first tier `apps` folder, and replace the relevant instances with the word `config`.  [PyCharm](https://www.jetbrains.com/pycharm/), my favorite python development IDE, has a very handy tool to do just that; a snippet of the instances that PyCharm finds is shown below.  You could also use `grep`, for instance.

![PyCharm refactoring results](https://www.tumblingprogrammer.com/media/2017/06/pycharm_refactor_boilerplate_to_config.png "PyCharm refactoring results")

Once we swap `apps` for `config`, we run the server (i.e., execute `manage.py runserver`) to make sure that our refactoring works OK; we should get the typical `Welcome to django!` page.

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

Let's commit and call the commitment `"rename second tier apps folder as config"`.

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

Below are the listings of the `base.py`, `local.py`, and `production.py` files resulting from following the logic I just described.

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

By keeping `DEBUG = True` limited to our local environment, when we deploy we don't have to worry about editing django's settings file to turn `DEBUG` off which, for security reasons, [is very important to do](https://docs.djangoproject.com/en/1.11/ref/settings/#debug). 

Now, how do we go about telling django which settings file to use?  For that, we will need to set an environment variable that we can use to programmatically instruct django to use the proper settings file.

We'll call our variable `DJANGO_EXECUTION_ENVIRONMENT`. On our local environment, its value will be `LOCAL` and on our deployment environment, it will be `PRODUCTION`.

For Mac OS (my development OS of choice), I just simply run `nano -w ~/.bash_profile` on the command line, and add the following line to my `.bash_profile` file:

    export DJANGO_EXECUTION_ENVIRONMENT="LOCAL"

Once I save the changes, I run `source ~/.bash_profile` on the command line, and then test that it worked by running `echo $DJANGO_EXECUTION_ENVIRONMENT`, the result of which, if successful, should be `LOCAL`.

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

We added the function `get_env_variable`, which allows us to humanize the error message given by django if for some reason it can't read the environment variable. Thanks to the folks at [two scoops press](https://www.twoscoopspress.com/) for this and other valuable tips.  Buy their book if you haven't done that.  

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

Let's commit and call the commitment `"refactor settings.py into settings + base | local | production"`.

### django's SECRET_KEY
_____

One of the main reasons we are doing all this refactoring / re-arranging is security.  As an example, by storing our `SECRET_KEY` in a file being tracked by `git` (and potentially being uploaded to a public repository like github, we may be exposing it.  Refer to [django's documentation as to why it is important to keep it secret](https://docs.djangoproject.com/en/1.11/ref/settings/#secret-key).   

We now know how to store variables in our environment. This time around, though, we will use a different procedure to accomplish that. We'll see why in a bit.

By now you should be using `virtualenv wrapper` (highly recommended!). By now I assume also that for every application you setup a different virtual environment (highly recommended too!). We will store django's secret key in our virtual environment.  Go to the bin folder of your `virtualenv`.  For me, the path to the bin folder looks like `/Users/puma/.virtualenvs/djangoboilerplate/bin`.  Within it there is a file called `postactivate`; edit it as follows:

    #!/bin/bash
    # This hook is sourced after this virtualenv is activated.
    
    #your DJANGO_SECRET_KEY variable goes here
    export DJANGO_SECRET_KEY="YOUR_SECRET_KEY_GOES_HERE"

As the second line informs us, whatever is in that file will be sourced post activation of the virtual environment. Go ahead and copy the key that django generated for you when you executed `django-admin startproject apps`.

The reason I use the virtual environment to store django's `SECRET_KEY` is that I make it different for each django application.  Hence, as I activate the app's virtual environment, I get the app's unique key.  As for the `DJANGO_EXECUTION_ENVIRONMENT` variable, its value (i.e., `LOCAL`) is always the same regardless of the application, and as such I store it in my system's `.bash_profile` and make it available system-wide.

Make sure that what you just did works by deactivating and reactivating your virtual environment, and then by running `echo $DJANGO_SECRET_KEY` on your command line. It should output your secret key.

Next, edit your `settings/base.py` file so it goes from

    import os
    ...
    SECRET_KEY = '...SECRET_KEY_GENERATED_BY_DJANGO...'
    ...

to

    import os
    
    from manage import get_env_variable 
    # the above is the function that we added to...
    # ...manage.py to humanize potential configuration errors
    
    DJANGO_SECRET_KEY = get_env_variable('DJANGO_SECRET_KEY')
    ...
    SECRET_KEY = DJANGO_SECRET_KEY
    ...

Let's run `manage.py runserver` to make sure that django is working OK. We should see the typical `Welcome to django!` page.

Let's commit and call the commitment `"hide django's secret key"`.

There you have it. Your django's secret key is indeed secret now, and not out for the world to see when, for instance, you decide to upload your code to github :)

### Distinguishing between django, local, and third party applications
_____
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

As usual, let's test that django runs OK.  If it runs smoothly, let's commit and call our commitment `"distinguish between django, local, and third party applications"`.

### Distinguishing between development and production requirements
_____
Similar to what we did with `settings.py`, we will refactor our `requirements.txt` in such a way that we keep track of and use the right packages for our development and production environments.

Let's add the following files to the `requirements` folder: `base.txt`, `local.txt`, and `production.txt`.

Let's commit and call the commitment `"add base.txt, local.txt, and production.txt requirement files"`.

As far as figuring out where packages belong, the logic that we applied to refactor the `settings.py` file will be similar too:

    Is this package needed for both development and production? 
        Yes:
            It goes into base.txt
        Else:
            Is it needed for development only?
            Yes:
                It goes into local.txt
            Else:
                It goes into production.txt

Let's install `django-bootstrap3` to illustrate the workflow, which is as follows:

1. Start with a fresh git commitment (i.e., no changes yet to be committed);
2. Start with a baseline file `requirements.txt`, which resides under folder `requirements`; it's imperative that it's kept up to date (i.e., follow step 4 below every time that a new package is installed); at the moment, the contents of our `requirements.txt` haven't changed:

        appdirs==1.4.3
        Django==1.11.1
        packaging==16.8
        pyparsing==2.2.0
        pytz==2017.2
        six==1.10.0

    The above are required for the local and production environments, so they go into our `base.txt` file. 

3. Install the new package (don't do bundle installation of packages [i.e., `pip install package_1 package_2`]; I recommend doing it one by one so you can keep proper track of dependencies); let's activate our virtual environment (if not already activated) and execute `pip install django-bootstrap3==8.2.3`;
4. In the command line, and under folder `requirements`, execute `pip freeze > requirements.txt`;
5. If you are using an IDE that integrates with git, it's likely capable of telling you that there is a new line, and will highlight it for you; if you don't have an IDE, running a `git diff` on the `requirements.txt` file will tell you what new lines were added. The listing is below, showing our brand new package listed there.

        appdirs==1.4.3
        Django==1.11.1
        django-bootstrap3==8.2.3 #the recently added package
        packaging==16.8
        pyparsing==2.2.0
        pytz==2017.2
        six==1.10.0

6. We can go ahead and copy the `django-bootstrap3==8.2.3` line into our `base.txt` file, for the package will be required by both our production and local environments. File `base.txt` at the moment reads as follows:

        # python and django packages
        appdirs==1.4.3
        Django==1.11.1
        packaging==16.8
        pyparsing==2.2.0
        pytz==2017.2
        six==1.10.0
        
        django-bootstrap3==8.2.3

    Notice how I leave a one line space between packages. Later on we will see how some of them will have dependencies, and having that separation is useful. 
    
Let's add `-r base.txt` at the top of the contents of both the `local.txt` and `production.txt` files, which will tell `pip` to extend the requirements found in the `base.txt` file.  That's all we have in those two files, for now.

We now have the backbone to distinguish between local, production, and common packages. We'll be adding packages later as we complete our boilerplate, and we will see the workflow in action again.

For now, let's commit and call the commitment `"separate base, local, and production packages"`.

### Setting up the `main` app
_____

Every app has a part that functions as the main or core portion of the application that acts as the glue that keeps all apps together.  We will call this app the `main` app.  Some developers call it the `core` app.  I have stuck with `main` to avoid confusion with, for instance, the `django.core` library during module import operations. One could argue that `main` could be problematic too, given that, for instance, it could clash with the line `if __name__ == "__main__":` found in django's `manage.py`. It looks like we are out of meaningful words for such an app.

At any rate, while under the `apps` folder in the terminal, let's run `manage.py startapp main`. Once it's done, our structure should look as follows:  

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
    | | |____main
    | | | |______init__.py
    | | | |____admin.py
    | | | |____apps.py
    | | | |____migrations
    | | | | |______init__.py
    | | | |____models.py
    | | | |____tests.py
    | | | |____views.py
    | | |____manage.py
    | |____docs
    | | |____readme.md
    | |____requirements
    | | |____base.txt
    | | |____local.txt
    | | |____production.txt
    | | |____requirements.txt

Within the `settings/base.py` file, let's register our application with django by changing:

    ...
    LOCAL_APPS = [
    ]
    ...

to 

    ...
    LOCAL_APPS = [
        'main',
    ]
    ...

We'll continue setting up our `main` app at a later time. 

For now, let's commit and call our commitment `"add main app"` (don't forget to run `git add .`, `git status` and double checking what's being committed).

### Setting up our application system folders
_____
We will install `django-environ` to enhance our app's ability to interact with the system environment. More info on `django-environ` can be found [here](https://github.com/joke2k/django-environ).

Let's follow the workflow that we discussed in our **_Distinguishing between development and production requirements_** section. 

First, this package will be needed for both development and production.

We are starting with a fresh commitment (you can verify that by running `git status`, which should result in a `nothing to commit, working tree clean` statement on your terminal).

So, from the command line, and assuming that your `djangoboilerplate` virtual environment is active, run `pip install django-environ==0.4.3`. Once the installation process is over, let's go into the `requirements` folder on the command line and run `pip freeze > requirements.txt`. A new line pops up in our `requirements.txt` file, which reads `django-environ==0.4.3`.  Let's copy that line into the `requirements/base.txt` file, which now should look as follows:

    # python and django packages
    appdirs==1.4.3
    Django==1.11.1
    packaging==16.8
    pyparsing==2.2.0
    pytz==2017.2
    six==1.10.0
    
    django-bootstrap3==8.2.3
    
    django-environ==0.4.3

Note that there is no need to add `django-environ` to the list of third party apps within the `settings/base.py` file.  We can import it directly where needed by invoking `import environ`.

Let's commit, and call our commitment `"add django-environ"`.

### Setting up our templates, static, and media directories
_____
Let's add the following folders and files within our `apps` folder:

    | | |____media
    | | | |____twitter48.png
    | | |____static
    | | | |____css
    | | | | |____global.css
    | | | |____img
    | | | | |____twitter48.png
    | | | |____js
    | | | | |____global.js
    
Our layout should now look like the following:

    |____djangoboilerplate
    | |____apps
    | | |____config
    | | | |____(...)
    | | |____main
    | | | |____(...)
    | | |____manage.py
    | | |____media
    | | | |____twitter48.png
    | | |____static
    | | | |____css
    | | | | |____global.css
    | | | |____img
    | | | | |____twitter48.png
    | | | |____js
    | | | | |____global.js
    | |____docs
    | | |____readme.md
    | |____requirements
    | | |____(...)

Let's commit and call our commitment `"add media and static folders"`.

While at it, let's add a `templates` folder under the `apps` folder, and a `staticfiles` folder under the root folder (i.e., the `djangoboilerplate` folder). `templates` is where we will store our html templates.  We will use `staticfiles` as the destination where we will tell django to collect static files to serve them to our users. If you go into the `.gitignore` file, you will find toward the bottom a `/staticfiles/` line, by which we tell `git` to ignore the contents of that folder. That's by design as those files exist somewhere else and are collected there by django just for web serving purposes. 

By now our full blown layout should look like the following:

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
    | | |____main
    | | | |______init__.py
    | | | |____admin.py
    | | | |____apps.py
    | | | |____migrations
    | | | | |______init__.py
    | | | |____models.py
    | | | |____tests.py
    | | | |____views.py
    | | |____manage.py
    | | |____media
    | | | |____twitter48.png
    | | |____static
    | | | |____css
    | | | | |____global.css
    | | | |____img
    | | | | |____twitter48.png
    | | | |____js
    | | | | |____global.js
    | | |____templates
    | |____docs
    | | |____readme.md
    | |____requirements
    | | |____base.txt
    | | |____local.txt
    | | |____production.txt
    | | |____requirements.txt
    | |____staticfiles

Now, let's edit `settings/base.py` so it goes from:

    ...
    BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
    ...

to

    ...
    import environ
    ...
    ROOT_DIR = str(environ.Path(__file__) - 4)
    APPS_DIR = str(os.path.join(ROOT_DIR,'apps'))
    TEMPLATES_DIR = str(os.path.join(APPS_DIR,'templates'))
    STATIC_DIR = str(os.path.join(APPS_DIR,'static'))
    MEDIA_ROOT = str(os.path.join(APPS_DIR,'media'))
    DATABASE_FILE_NAME = str(os.path.join(APPS_DIR,'db.sqlite3'))
    STATIC_ROOT = str(os.path.join(ROOT_DIR,'staticfiles'))
    MEDIA_URL = '/media/'
    STATICFILES_DIRS = [STATIC_DIR, ]
    ...

Most of the code above is self-explanatory.  Basically, we are telling django where stuff will reside, including static files, media files (which is normally made up of files uploaded by users), as well as the full path (including the filename) of our database file (in our case, we are using SQLite).

Worth noticing is line `ROOT_DIR = str(environ.Path(__file__) - 4)`, by which we tell django to use the directory four levels above the `base.py` file as the `ROOT_DIR` (or our `djangoboilerplate` directory), which becomes the basis of all subsequent directories.

We can test all the above by executing the below python3+ script:

    import os
    import environ
    
    # edit the below line so it matches your base.py; 
    # below is what mine looks like
    file ="/Users/puma/Desktop/projects/articles/final/djangoboilerplate/apps/config/settings/base.py"

    ROOT_DIR = str(environ.Path(file) - 4)
    print(ROOT_DIR)
    APPS_DIR = str(os.path.join(ROOT_DIR,'apps'))
    print(APPS_DIR)
    TEMPLATES_DIR = str(os.path.join(APPS_DIR,'templates'))
    print(TEMPLATES_DIR)
    STATIC_DIR = str(os.path.join(APPS_DIR,'static'))
    print(STATIC_DIR)
    MEDIA_ROOT = str(os.path.join(APPS_DIR,'media'))
    print(MEDIA_ROOT)
    DATABASE_FILE_NAME = str(os.path.join(APPS_DIR,'db.sqlite3'))
    print(DATABASE_FILE_NAME)
    STATIC_ROOT = str(os.path.join(ROOT_DIR,'staticfiles'))
    print(STATIC_ROOT)
    
When I execute the script, I get the following:

    /Users/puma/Desktop/projects/articles/final/djangoboilerplate
    /Users/puma/Desktop/projects/articles/final/djangoboilerplate/apps
    /Users/puma/Desktop/projects/articles/final/djangoboilerplate/apps/templates
    /Users/puma/Desktop/projects/articles/final/djangoboilerplate/apps/static
    /Users/puma/Desktop/projects/articles/final/djangoboilerplate/apps/media
    /Users/puma/Desktop/projects/articles/final/djangoboilerplate/apps/db.sqlite3
    /Users/puma/Desktop/projects/articles/final/djangoboilerplate/staticfiles

Which is exactly what I need.

We can now edit the `base.py`, like so:

`TEMPLATES` @ `settings/base.py`

    ...
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [
                TEMPLATES_DIR,
            ],
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
    ...
    
`DATABASES` @ `base.py`

    ...
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': DATABASE_FILE_NAME,
        }
    }
    ...

If you run the django server now, you will notice that another `db.sqlite3` file pops up under the `apps` folder, and that, again, we have unapplied migrations.  What has happened is that, via the new `DATABASE_FILE_NAME`, we told django to store the database under that folder, as opposed to under the `config` folder, and django went ahead and created a brand new `db.sqlite3` there -hence the unapplied migrations.  It makes more sense to store it under the `apps` folder, and we will keep it there.  We can safely remove the old one, and apply migrations to get rid of django's warning.

Our layout should look as follows:

    |____djangoboilerplate
    | |____apps
    | | |____config
    | | | |______init__.py
    | | | |____settings
    | | | | |______init__.py
    | | | | |____base.py
    | | | | |____local.py
    | | | | |____production.py
    | | | |____urls.py
    | | | |____wsgi.py
    | | |____db.sqlite3
    | | |____main
    | | | |______init__.py
    | | | |____admin.py
    | | | |____apps.py
    | | | |____migrations
    | | | | |______init__.py
    | | | |____models.py
    | | | |____tests.py
    | | | |____views.py
    | | |____manage.py
    | | |____media
    | | | |____twitter48.png
    | | |____static
    | | | |____css
    | | | | |____global.css
    | | | |____img
    | | | | |____twitter48.png
    | | | |____js
    | | | | |____global.js
    | | |____templates
    | |____docs
    | | |____readme.md
    | |____requirements
    | | |____base.txt
    | | |____local.txt
    | | |____production.txt
    | | |____requirements.txt
    | |____staticfiles

It's time to commit our changes.  Let's call this commitment `"add templates_dir and staticfiles folders; refactor our database location"`.

### A home page
_____
It's time to set up our home page.  Under the `templates` folder, let's add a folder called `main` to host our `main` app templates. Within the `main` folder, let's add a `home.html` file.  For now, let's just add the following code to it:

    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>djangoboilerplate</title>
    </head>
    <body>
        <div class="jumbotron centered-text">
            <h1>Hello djangonaut!</h1>
        </div>
        <p class="centered-text">This is regular text (i.e., with no Bootstrap applied)</p>
        <p class="centered-text"><span class="text-muted">This is a bootstrapped text - if different from the one above (i.e., it is greyish),
            Bootstrap works!</span></p>
    </body>
    </html>

We need to add a `urls.py` file under the `main` folder to hold the `urls` for our `main` app. Let's add the following code to it:

    from django.conf.urls import url
    from . import views
    
    urlpatterns = [
        url(r'^$', views.home, name='home'),
    ]    

Let's edit the `settings/urls.py` so it includes the urls in `main/urls.py`, like so:

    from django.conf.urls import url, include
    from django.contrib import admin
    
    urlpatterns = [
        url(r'^admin/', admin.site.urls),
        url(r'', include('main.urls')),  
    ]

Finally, we need to create our view.  Let's edit the `main/views.py` as follows:

    from django.shortcuts import render
    
    def home(request):
        return render(request, 'main/home.html', context=None)

If everything goes well, you should see the screen below on your browser once you run your django server.

![tumbling programmer's django project boilerplate home page](https://www.tumblingprogrammer.com/media/2017/06/hello_djangonaut_plain.png "tumbling programmer's django project boilerplate home page")

You may have noticed that we have some `bootstrap` classes in our `home.html` file (like `jumbotron`, or `text-muted`.  They don't render properly at the moment because we haven't completely setup `bootstrap`. We'll do that later.

For now, let's commit and call our commitment `"add home view"`.

Our layout at the moment should look as follows:

    |____djangoboilerplate
    | |____apps
    | | |____config
    | | | |______init__.py
    | | | |____settings
    | | | | |______init__.py
    | | | | |____base.py
    | | | | |____local.py
    | | | | |____production.py
    | | | |____urls.py
    | | | |____wsgi.py
    | | |____db.sqlite3
    | | |____main
    | | | |______init__.py
    | | | |____admin.py
    | | | |____apps.py
    | | | |____migrations
    | | | | |______init__.py
    | | | |____models.py
    | | | |____tests.py
    | | | |____urls.py
    | | | |____views.py
    | | |____manage.py
    | | |____media
    | | | |____twitter48.png
    | | |____static
    | | | |____css
    | | | | |____global.css
    | | | |____img
    | | | | |____twitter48.png
    | | | |____js
    | | | | |____global.js
    | | |____templates
    | | | |____main
    | | | | |____home.html
    | |____docs
    | | |____readme.md
    | |____requirements
    | | |____base.txt
    | | |____local.txt
    | | |____production.txt
    | | |____requirements.txt
    | |____staticfiles


### Twitter bootstrap
_____
So far we have installed `bootstrap` on our virtual environment, but we haven't told django about it.  We will do so shortly, and while at it, we will also add other settings that will enable `bootstrap` to work properly.  Let's go ahead and edit the `settings/base.py` file as follows:

    ...
    THIRD_PARTY_APPS = [
        'bootstrap3',
    ]
    ...
    # Bootstrap settings
    BOOTSTRAP3 = {
    
        # The URL to the jQuery JavaScript file
        'jquery_url': '//code.jquery.com/jquery-3.1.1.min.js',
    
        # The Bootstrap base URL
        'base_url': '//maxcdn.bootstrapcdn.com/bootstrap/3.3.7/',
    
        # The complete URL to the Bootstrap CSS file (None means derive it from base_url)
        'css_url': None,
    
        # The complete URL to the Bootstrap CSS file (None means no theme)
        # 'theme_url': None,
        'theme_url': '//maxcdn.bootstrapcdn.com/bootswatch/3.3.7/readable/bootstrap.min.css',
    
        # The complete URL to the Bootstrap JavaScript file (None means derive it from base_url)
        'javascript_url': None,
    
        # Put JavaScript in the HEAD section of the HTML document (only relevant if you use bootstrap3.html)
        'javascript_in_head': False,
    
        # Include jQuery with Bootstrap JavaScript (affects django-bootstrap3 template tags)
        'include_jquery': True,
    
        # Label class to use in horizontal forms
        'horizontal_label_class': 'col-md-3',
    
        # Field class to use in horizontal forms
        'horizontal_field_class': 'col-md-9',
    
        # Set HTML required attribute on required fields, for Django <= 1.8 only
        'set_required': True,
    
        # Set HTML disabled attribute on disabled fields, for Django <= 1.8 only
        'set_disabled': False,
    
        # Set placeholder attributes to label if no placeholder is provided
        'set_placeholder': True,
    
        # Class to indicate required (better to set this in your Django form)
        'required_css_class': '',
    
        # Class to indicate error (better to set this in your Django form)
        'error_css_class': 'has-error',
    
        # Class to indicate success, meaning the field has valid input (better to set this in your Django form)
        'success_css_class': 'has-success',
    
        # Renderers (only set these if you have studied the source and understand the inner workings)
        'formset_renderers': {
            'default': 'bootstrap3.renderers.FormsetRenderer',
        },
        'form_renderers': {
            'default': 'bootstrap3.renderers.FormRenderer',
        },
        'field_renderers': {
            'default': 'bootstrap3.renderers.FieldRenderer',
            'inline': 'bootstrap3.renderers.InlineFieldRenderer',
        },
    }

I tend to put `bootstrap` settings toward the end of the `settings/base.py` file.  Note that you don't have to have all the above settings in the `settings/base.py` file.   

You can find out more about `django-bootstrap` [here](https://django-bootstrap3.readthedocs.io/en/latest/settings.html). 

`bootstrap` should be good to go.  To fully take advantage of it, let's add a base template that inherits from `bootstrap` base template.

### The base template
_____
Let's add a `base.html` file directly under the `templates` folder, and let's add the following code to it:

    {% extends 'bootstrap3/bootstrap3.html' %}
    {% load bootstrap3 %}
    
    {% block bootstrap3_title %}
        {% block title %}
        {% endblock %} - django boilerplate App
    {% endblock %}
    
    {% block bootstrap3_extra_head %}
        {% bootstrap_javascript %}
        {% block head %}        
        {% endblock %}
    {% endblock %}
    
    {% block bootstrap3_content %}
        {% block content %}
        {% endblock %}
    {% endblock %}
    
    {% block bootstrap3_extra_script %}
        {% block scripts %}
        {% endblock %}
    {% endblock %}

Now let's edit the `home.html` template so it reads as follows:

    {% extends "base.html" %}
    {% load bootstrap3 %}
    
    {% block title %} Home
    {% endblock %}
    
    {% block head %}
    {% endblock %}
    
    {% block content %}
        <div class="jumbotron centered-text">
            <h1>Hello djangonaut!</h1>
        </div>
        <p class="centered-text">This is regular text (i.e., with no Bootstrap applied)</p>
        <p class="centered-text"><span class="text-muted">This is a bootstrapped text - if different from the one above (i.e., it is greyish),
            Bootstrap works!</span></p>
    {% endblock %}
    
    {% block scripts %}
    {% endblock %}

If we run our django server and hit the home page, we should see the following:

![tumbling programmer's django project boilerplate bootstrapped home page](https://www.tumblingprogrammer.com/media/2017/06/hello_djangonaut_bootstrapped.png "tumbling programmer's django project boilerplate bootstrapped home page")

As you can see, `bootstrap` works properly.

Let's commit and call our commitment `"finish bootstrap, add base.html"`.

### Linking our static files
_____
Let's edit `global.css` as follows:

    /* global css code goes here */
    .centered-text {
        text-align: center;
    }

If you examine the code in `home.html`, we had a class called `centered-text` there.  When django rendered the page, the text wasn't centered because the `global.css` is not "seen" by `home.html`, yet.

Let's edit the `global.js` file as follows:

    // global js code goes here
    alert("js is working");

If we link the `global.js` file properly, we should see an alert generated by javascript as soon as the page loads.  The final step to link our static files is to introduce `{% load staticfiles %}`, the following block of code 

    <link rel="stylesheet" href="{% static 'css/global.css' %}"/>
    {% bootstrap_javascript %}

and `<script src="{% static 'js/global.js' %}"></script>` to the code within `base.html`, which should now read as follows:

    {% extends 'bootstrap3/bootstrap3.html' %}
    {% load bootstrap3 %}
    {% load staticfiles %}
    
    {% block bootstrap3_title %}
        {% block title %}
        {% endblock %} - django boilerplate App
    {% endblock %}
    
    {% block bootstrap3_extra_head %}
        <link rel="stylesheet" href="{% static 'css/global.css' %}"/>
        {% bootstrap_javascript %}
        {% block head %}
        {% endblock %}
    {% endblock %}
    
    {% block bootstrap3_content %}
        {% block content %}
        {% endblock %}
    {% endblock %}
    
    {% block bootstrap3_extra_script %}
        <script src="{% static 'js/global.js' %}"></script>
        {% block scripts %}
        {% endblock %}
    {% endblock %}

If we run our django server, we should get the following, which show that we have successfully linked `global.js` and `global.css`, and that they are ready to receive and serve code.

![tumbling programmer's django project boilerplate home page with javascript working](https://www.tumblingprogrammer.com/media/2017/06/hello_djangonaut_js_works.png "tumbling programmer's django project boilerplate home page with javascript working")

Our `centered-text` class in `home.html` now works.

![tumbling programmer's django project boilerplate home page with css working](https://www.tumblingprogrammer.com/media/2017/06/hello_djangonaut_css_is_properly_linked.png "tumbling programmer's django project boilerplate home page with css working")

Let's go ahead and comment out line `alert("js is working");` in our `global.js` file (you can later use this line to further test your environment and to make sure that things are working properly, if in doubt).

Let's commit and call our commitment `"link javascript and css; link staticfiles"`.

### Getting `django-livereload-server` up and running
_____
`django-livereload-server` (more info [here](https://github.com/tjwalch/django-livereload-server) watches for changes on the files that make up your app, including `html`, `css`, `javascript`, and django files.  Once it catches the changes, it reloads your browser automatically so you don't have to constantly hit reload on it.

First, let's install it.  As usual, we will follow the workflow that we described under section **_Distinguishing between django, local, and third party applications_**. This will be a local only package, and we are starting with a fresh commitment.

With our `djangoboilerplate` virtual environment activated, let's run `pip install django-livereload-server==0.2.3`.

On the terminal, let's navigate into our `requirements` folder and run `pip freeze > requirements.txt`.  A `git diff` on the file will tell us that the following lines were added to it:

    beautifulsoup4==4.6.0
    django-livereload-server==0.2.3
    tornado==4.5.1

Let's add them to our `requirements/local.txt` file, which now should read as follows:

    -r base.txt
    
    django-livereload-server==0.2.3
    beautifulsoup4==4.6.0
    tornado==4.5.1

Notice how I placed `django-livereload-server==0.2.3` on top, by which I signify that the bottom two are required by `django-livereload-server`.  It's just a convention I use.  When you install the requirements from a `txt` file, the order in which they are placed shouldn't really matter (unless there were conflicts between required packages).

Let's edit our `settings/local.py` file so it reads as follows:

    from .base import *
    
    DEBUG = True
    
    MIDDLEWARE_CLASSES += (
        'livereload.middleware.LiveReloadScript',
    )
    INSTALLED_APPS += [
        'livereload',
    ]
    
Stop your django server if it's running, run `manage.py livereload` (leave it running), and on another terminal tab, run your django server.  Open your home page on your browser. Keep your browser visible.

Let's open our `home.html` file and add the following block of code right below the `jumbotron`:

    <div class="container-fluid">
        <h3>This is the home of a django project boilerplate</h3>
        <p>You can find the article / tutorial that explains how it got created by visiting
            <a href="https://www.tumblingprogrammer.com/setting-up-a-django-project-boilerplate/">
                https://www.tumblingprogrammer.com/setting-up-a-django-project-boilerplate/</a></p>
        <p>You can find the source code for it at <a href="https://github.com/tumblingprogrammer/djangoboilerplate">
            https://github.com/tumblingprogrammer/djangoboilerplate</a></p>
    </div>
    <hr>
    
As soon as you save the changes on your `home.html` file, you should automatically see the changes on your browser, which should look as follows:

![tumbling programmer's django project boilerplate home page livereload demo](https://www.tumblingprogrammer.com/media/2017/06/hello_djangonaut_livereload.png "tumbling programmer's django project boilerplate home page livereload demo")

Let's commit and call the commitment `"add django-livereload-server"`.

### Getting `django-fontawesome` up and running
_____
I usually find myself needing more icons than what twitter bootstrap's glyphicons offer.  [django-fontawesome](https://github.com/redouane/django-fontawesome) comes to the rescue.

This package will go into the `base.txt` requirements file.

As usual, we activate our virtual environment (if not already activated), and run `pip install django-fontawesome==0.3.1`. `pip` will install it, along with `PyYAML==3.12`, which is required by `django-fontawesome`.

Once we update it, our `requirements/base.txt` should read as follows:

    # python and django packages
    appdirs==1.4.3
    Django==1.11.1
    packaging==16.8
    pyparsing==2.2.0
    pytz==2017.2
    six==1.10.0
    
    django-bootstrap3==8.2.3
    
    django-environ==0.4.3
    
    django-fontawesome==0.3.1
    PyYAML==3.12

Let's edit our `settings.base.py` file, as follows:

    ...
    THIRD_PARTY_APPS = [
        'bootstrap3',
        'fontawesome',
    ]
    ...

Now we need to add `{% load fontawesome %}` `{% fontawesome_stylesheet %}` to our `base.html` file, like so:

    {% extends 'bootstrap3/bootstrap3.html' %}
    {% load bootstrap3 %}
    {% load staticfiles %}
    {% load fontawesome %}
    ...
    {% block bootstrap3_extra_head %}
        <link rel="stylesheet" href="{% static 'css/global.css' %}"/>
        {% fontawesome_stylesheet %}
        {% bootstrap_javascript %}
        {% block head %}
        {% endblock %}
    {% endblock %}
    ...

In our `home.html` file, let's add `{% load fontawesome %}` and some html to test that the installation works, like so:

    {% extends "base.html" %}
    {% load bootstrap3 %}
    {% load fontawesome %}
    ...    
        <p class="centered-text"><span class="text-muted">This is a bootstrapped text - if different from the one above 
            (i.e., it is greyish),
            Bootstrap works!</span></p>
        <p class="centered-text">If you can see a user icon {% fontawesome_icon 'user' %} here, font awesome works!</p>
    ...

If everything goes well, our home page will show the following at the bottom of it:

![tumbling programmer's django project boilerplate home page with font awesome](https://www.tumblingprogrammer.com/media/2017/06/hello_djangonaut_fontawesome_works.png "tumbling programmer's django project boilerplate home page with font awesome")

Let's commit and call our commitment `"add fontawesome"`.

### Creating our `DatesBaseModel`
_____
Let's add one handy model to our `main` app models. Edit `main/models.py` so it reads as follows:

    from django.db import models
    
    # models created with this base will inherit created_date and modified_date automatically
    class DatesBaseModel(models.Model):
        created_date = models.DateTimeField(auto_now_add=True)
        modified_date = models.DateTimeField(auto_now=True)
    
        class Meta:
            abstract = True

As the comment above explains, if we were to extend this model (as in `class Entry(DatesBaseModel):`), the "extension" model (`Entry`, in our example) will automatically have the fields `created_date` and `modified_date`.  Handy indeed.  We could use the same approach if, for instance, we had models that had address fields in common.  In general, we look for common denominators, throw those denominators into a base model, and extend that base model as needed.

At this point, we don't need to make migrations or migrate because we have used the `abstract = True` statement (more info on that [here](https://docs.djangoproject.com/en/1.11/topics/db/models/#abstract-base-classes).

Let's commit and call our commitment `"add DatesBaseModel"`.

### Serving `media` and `static` files
_____
We still have homework to do to finish off serving of media and static files.

Let's edit our `base.py` file and add `"django.template.context_processors.media",` to our list of context processors in `settings/base.py` file, like so:

    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [
                TEMPLATES_DIR,
            ],
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                    "django.template.context_processors.media",
                ],
            },
        },
    ]

Let's also edit our `config/urls.py` file from:

    from django.conf.urls import url, include
    from django.contrib import admin
    
    urlpatterns = [
        url(r'^admin/', admin.site.urls),
        url(r'', include('main.urls')),
    ]

to 

    from django.conf.urls import url, include
    from django.contrib import admin
    from manage import get_env_variable
    from django.conf import settings
    from django.conf.urls.static import static
    
    urlpatterns = [
        url(r'^admin/', admin.site.urls),
        url(r'', include('main.urls')),
    ]

    DJANGO_EXECUTION_ENVIRONMENT = get_env_variable('DJANGO_EXECUTION_ENVIRONMENT')
    if DJANGO_EXECUTION_ENVIRONMENT == 'LOCAL':
        urlpatterns = urlpatterns + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

Also, let's edit our `home.html` file as follows:

    {% extends "base.html" %}
    {% load bootstrap3 %}
    {% load fontawesome %}
    {% load staticfiles %}
    ...
        <p class="centered-text">If you can see a user icon {% fontawesome_icon 'user' %} here, font awesome works!</p>
    
        <hr>
        <div class="container-fluid">
            <p>Our media server works if you can successfully see twitter's logo here:
                <a href="https://twitter.com/tumblingprgrmmr" title="tumbling programmer's twitter account">
                    <img src="{{ MEDIA_URL }}twitter48.png" alt="twitter logo"></a>
            </p>
            <p>Static-serving files works too if you can successfully see twitter's logo here:
                <a href="https://twitter.com/tumblingprgrmmr" title="tumbling programmer's twitter account">
                    <img src="{% static 'img/twitter48.png'%}" alt="twitter logo"></a>
            </p>
        </div>
    {% endblock %}
    ...

Adding `"django.template.context_processors.media"` to our list of context processors in our `settings/base.py` file is what makes possible to use the dynamic url `{{ MEDIA_URL }}` to serve media files.

If all goes well, you should see the below info at the bottom of your home page:

![tumbling programmer's django project boilerplate home page with static and media files](https://www.tumblingprogrammer.com/media/2017/06/serving_media_and_static_files.png "tumbling programmer's django project boilerplate home page with static and media files")

Let's commit and call our commitment `"finalize and test media and static file serving "`.

### `.gitignore` and png files
_____
By default, my `.gitignore` file includes `png` (and other image extensions, for that matter) so they stay out of version control.  I realized, though, that I need to include them in the `git` [repository](https://github.com/tumblingprogrammer/djangoboilerplate "tumbling programmer's django project boilerplate tutorial files"), so readers get everything that is needed to follow the tutorial.  So, I excluded the `png` files from the git `.gitignore` file, and committed the changes; I called the commitment `"modify .gitignore to exclude png files"`. Below is what got committed:

	modified:   ../.gitignore
	new file:   media/twitter48.png
	new file:   static/img/twitter48.png
	
### Adding error pages
_____
We need to add html templates to let users know that errors have occurred. Under the `templates` folder, add the following files; the respective code for each is listed following the file name.

`403_csrf.html`:

	{% extends "base.html" %}
	
	{% block title %}Forbidden (403){% endblock %}
	
	{% block content %}
	    <div class="container-fluid">
	        <h1>Forbidden (403)</h1>
	        <p>CSRF verification failed. Request aborted.</p>
	    </div>
	{% endblock content %}	
	
`404.html`:

	{% extends 'base.html' %}
	
	{% block title %}Page not found{% endblock %}
	
	{% block content %}
	    <div class="container-fluid">
	        <h1>Page not found</h1>
	        <p>Sorry but we couldn't find the page that you are looking for.</p>
	        <p>Try refreshing the page, or go to our <a href="{% url 'home' %}">home page</a></p>
	    </div>
	{% endblock content %}

`500.html`:

	{% extends "base.html" %}
	
	{% block title %}Server Error{% endblock %}
	
	{% block content %}
	    <div class="container-fluid">
	        <h1>Ooops!!! 500</h1>
	        <h3>Looks like something went wrong!</h3>
	        <p>Try refreshing the page, or go to our <a href="{% url 'home' %}">home page</a></p>
	        <p>We track these errors automatically, but if the problem persists feel free to contact us.</p>
	    </div>
	{% endblock content %}

Let's commit and call our commitment `"add error templates"`.

### The resulting layout
_____
Below is what the resulting layout looks like:

***At a high level***

	|____djangoboilerplate
	| |____apps
	| | |____config
	| | |____db.sqlite3
	| | |____main
	| | |____manage.py
	| | |____media
	| | |____static
	| | |____templates
	| |____docs
	| | |____readme.md
	| |____requirements
	| | |____base.txt
	| | |____local.txt
	| | |____production.txt
	| | |____requirements.txt
	| |____staticfiles

***All the gory details***

	|____djangoboilerplate
	| |____apps
	| | |____config
	| | | |______init__.py
	| | | |____settings
	| | | | |______init__.py
	| | | | |____base.py
	| | | | |____local.py
	| | | | |____production.py
	| | | |____urls.py
	| | | |____wsgi.py
	| | |____db.sqlite3
	| | |____main
	| | | |______init__.py
	| | | |____admin.py
	| | | |____apps.py
	| | | |____migrations
	| | | | |______init__.py
	| | | |____models.py
	| | | |____tests.py
	| | | |____urls.py
	| | | |____views.py
	| | |____manage.py
	| | |____media
	| | | |____twitter48.png
	| | |____static
	| | | |____css
	| | | | |____global.css
	| | | |____img
	| | | | |____twitter48.png
	| | | |____js
	| | | | |____global.js
	| | |____templates
	| | | |____403_csrf.html
	| | | |____404.html
	| | | |____500.html
	| | | |____base.html
	| | | |____main
	| | | | |____home.html
	| |____docs
	| | |____readme.md
	| |____requirements
	| | |____base.txt
	| | |____local.txt
	| | |____production.txt
	| | |____requirements.txt
	| |____staticfiles
	
### Where to take from here
_____

Although extensive, this tutorial covered only a fraction of all that is required to have a ready-to-be-deployed django project boilerplate.  Issues like testing, cron jobs, python helpers / utilities, user registration, the django debug toolbar, further security considerations, as well as others that will come along the way, may be worth studying and implementing, depending on the application's needs.