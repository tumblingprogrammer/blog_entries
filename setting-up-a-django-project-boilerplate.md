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
Every app has a part that acts as the main or core portion of the application that acts as the head that glues all apps together.  We will call this app the **_main_** app.  Some people call it the **_core_** app.  I have stuck with **_main_** to avoid confusion with, for instance, the **_django.core_** library during module import operations.

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









before

	|____djangoboilerplate
	| |____djangoboilerplate
	| | |______init__.py
	| | |____settings.py
	| | |____urls.py
	| | |____wsgi.py
	| |____(... other files and folders ...) 

after

	|____djangoboilerplate
	| |____config
	| | |______init__.py
	| | |____settings.py
	| | |____urls.py
	| | |____wsgi.py
	| |____(... other files and folders ...) 

Note that for brevity and clarity I have omitted the `pycache__` and `.pyc` folder and files.
