


### Introduction
_____
+ Typical approach

* kkss
* 

This is a follow up article to article blah blah blah 





### Opening an account at [pythonanywhere.com](http://pythonanywhere.com)
_____
If you don't have one, opening an account at pythonanywhere is easy, and you can start with a [beginner account](https://www.pythonanywhere.com/pricing/), which is free. I have opened one to host the [django project boilerplate app](http://www.tumblingprogrammer.com/setting-up-a-django-project-boilerplate/ "tumbling programmer - setting up a django project boilerplate"), and was lucky enough to find out that the username `djangoboilerplate` wasn't taken, so I took it.

**Tip!** | For a beginner account, your username will also be the subdomain where your app will live.  In my case, for example, the app can be reached at  [http://djangoboilerplate.pythonanywhere.com](http://djangoboilerplate.pythonanywhere.com "tumbling programmer - django project boilerplate app at pythonanywhere.com")

So far my experience with pythonanywhere (PAW) has been great.  Their customer support really shines, and their giving back to the community is great.

If you haven't done that, go ahead and [sign up for a beginner account](https://www.pythonanywhere.com/registration/register/beginner/)

### Getting your project files to PAW
_____
To accomplish this, let's go ahead and open a `bash` console. We do that by looking for the PAW dashboard, and then by clicking on the link shown below.

![tumbling programmer's deploying the boilerplate app to pythonanywhere - PAW dashboard - opening a bash console](https://www.tumblingprogrammer.com/media/2017/06/pythonanywhere-opening-a-bash-console.png "tumbling programmer's deploying the boilerplate app to pythonanywhere - PAW dashboard - opening a bash console")

By default, your bash console will start your session on your home directory (you will see something like `04:22 ~ $` on the terminal.  If you run `pwd` (the `print working directory` command) at the terminal, it will output something like `/home/[your_username]`.  In my case, it outputs `/home/djangoboilerplate`.  If I run `ls` (the `list files` command), I only get `README.txt`, which is a file that PAW installs there by default.  

The source code for the django project boilerplate lives at [https://github.com/tumblingprogrammer/djangoboilerplate](https://github.com/tumblingprogrammer/djangoboilerplate).  While at our home directory on our terminal (i.e., `/home/[your_username]`), let's run...

    git clone https://github.com/tumblingprogrammer/djangoboilerplate.git
    
Your terminal will output a listing similar to the one shown below:

    Cloning into 'djangoboilerplate'...
    remote: Counting objects: 182, done.
    remote: Compressing objects: 100% (110/110), done.
    remote: Total 182 (delta 56), reused 170 (delta 48), pack-reused 0
    Receiving objects: 100% (182/182), 25.32 KiB | 0 bytes/s, done.
    Resolving deltas: 100% (56/56), done.
    Checking connectivity... done.
    04:47 ~ $
    
If we run `ls`, we should see the following output:

    README.txt  djangoboilerplate
    
Notice the new `djangoboilerplate` folder, which is the root folder of the boilerplate app.

### Creating our virtual environment
_____

PAW is already setup with [virtual environment wrapper](http://virtualenvwrapper.readthedocs.io/en/latest/), which is great as we don't have to deal with getting it installed.  We'll use `djangoboilerplate` as the name of our virtual environment.  Let's go ahead and run the following command:

    mkvirtualenv djangoboilerplate --python=/usr/bin/python3.5

After a little while, our terminal prompt should display the name of our virtual environment, like so:

    (djangoboilerplate) 05:11 ~ $ 

Which means that our virtual environment is active.

### Setting our environment variables
_____

As part of making our application more secure, and to programmatically tell django that we are in a production environment, we will need to setup two variables: `DJANGO_SECRET_KEY`, which will be limited to the scope of our `djangoboilerplate` virtual environment, and `DJANGO_EXECUTION_ENVIRONMENT`, which can be set system-wide so all our django apps running on PAW know that they are operating in production and that they need to use production settings.

Read [http://www.tumblingprogrammer.com/setting-environment-variables/](http://www.tumblingprogrammer.com/setting-environment-variables/ "tumbling programmer's setting environment variables article")to learn more about why and how to set environment variables.

On your PAW dashboard, use the `files` tab to navigate to the `postactivate` file of your `djangoboilerplate` virtual environment, and go ahead and clik on it to edit it (PAW's default editor will open it up). The screenshot below shows how to get there.

![tumbling programmer's deploying the boilerplate app to pythonanywhere - editing the postactivate file](https://www.tumblingprogrammer.com/media/2017/06/pythonanywhere-editing-the-postactivate-file.png "tumbling programmer's deploying the boilerplate app to pythonanywhere - editing the postactivate file")

Edit it so it reads as follows:

    #!/bin/bash
    # This hook is sourced after this virtualenv is activated.
    
    export DJANGO_SECRET_KEY="your_django_secret_key_goes_here"
    
To generate a value for your `DJANGO_SECRET_KEY` you can use [this site](http://www.miniwebtool.com/django-secret-key-generator/) or, better yet, program your own python script to do it.

Once you edit the file, click on the `save` button on the top right corner of the window, or press `ctrl` + `l` simultaneously.

Go back to the terminal and make sure to deactivate your `djangoboilerplate` virtual environment (if it is active) by running `deactivate djangoboilerplate` and re-activating it by running `workon djangoboilerplate`.

**WARNING!** | Make sure to use opening and closing quotes (`" "`) on line `DJANGO_SECRET_KEY="your_django_secret_key_goes_here"`. Otherwise, `bash` will complain that it can't find a proper `EOF` closing.

**TIP!**| To make my life easier, I usually have a tab on my browser pointing to PAW's dashboard, and another one with an up-and-running `bash` terminal.

Once your `djangoboilerplate` virtual environment has been reactivated, run `echo $DJANGO_SECRET_KEY`, after which the terminal should output your secret key.

Go back to your PAW's dashboard.  Navigate to your home directory until you find your `.bashrc` profile.  Click to open it, and add the following at the bottom of the file.

    export DJANGO_EXECUTION_ENVIRONMENT="PRODUCTION"

Save the file and go back to the terminal.  Once on it, run `source ~/.bashrc`, which will run commands in the `.bashrc` file and will load functions contained in the file into our `bash` shell script, including making our variable `DJANGO_EXECUTION_ENVIRONMENT` available for our django app to use.  We can test this by running `echo $DJANGO_EXECUTION_ENVIRONMENT` on the terminal, which should output `PRODUCTION`.

**TIP!**| Once we run `source ~/.bashrc`, one of the things that it will do is deactivate our virtual environment.  We will need to reactivate it if in the future we want to execute commands that are directly related to our relevant python environment.

### Installing our requirements
_____
On the terminal, navigate to the `requirements` folder of the application.  The path for it should look like `/home/[your_username]/djangoboilerplate/requirements`.  

**TIP!** You can also use PAW's to get there, as shown below.

![tumbling programmer's deploying the boilerplate app to pythonanywhere - getting to the bashrc file](https://www.tumblingprogrammer.com/media/2017/06/pythonanywhere-getting-to-the-bashrc-file.png "tumbling programmer's deploying the boilerplate app to pythonanywhere - getting to the bashrc file")

Once your terminal session is open, make sure to activate the `djangoboilerplate` virtual environment. Once that's done, let's execute `pip install -r production.txt` to install the packages required by the application.  The process should run smoothly and should take a little while.  Once it's done, if we run `pip freeze` we should get the following listing:

    appdirs==1.4.3
    Django==1.11.1
    django-bootstrap3==8.2.3
    django-environ==0.4.3
    django-fontawesome==0.3.1
    packaging==16.8
    pyparsing==2.2.0
    pytz==2017.2
    PyYAML==3.12
    six==1.10.0

### Setting up the web app
_____
Go to the `Web` tab of your dashboard, click on the `Add a new web app` button, click `next` and select the `Manual configuration (including virtualenvs)` option on the next window. Select `Python 3.5`, then `next`on the `Manual Configuration...` window. After a while, PWA should show the `Web` tab of your application.  A partial screenshot of what mine looks like follows:

![shot]

Once that's done, let's enter the name of our virtualenv in the Virtualenv section on the web tab and click OK. We would enter `djangoboilerplate` (or whatever name your virtual environment has), and it will automatically complete to its full path in `/home/username/.virtualenvs`.

After doing it, mine looks like so:

![shot]

Now let's edit the WSGI file (found under the `Code` section of the `Web` tab).  Let's delete everything in it, and edit it so it reads as follows:

    import os
    import sys
    
    path = '/home/[your_username_goes_here]/djangoboilerplate/apps/'
    if path not in sys.path:
        sys.path.append(path)
    
    os.environ['DJANGO_SETTINGS_MODULE'] = 'config.settings.production'
    os.environ['DJANGO_SECRET_KEY'] = 'YOUR_SECRET_KEY_GOES_HERE'
    os.environ['DJANGO_EXECUTION_ENVIRONMENT'] = 'PRODUCTION'
    
    from django.core.wsgi import get_wsgi_application
    from django.contrib.staticfiles.handlers import StaticFilesHandler
    application = StaticFilesHandler(get_wsgi_application())

Notice that even though we had already set the `DJANGO_SECRET_KEY` and `DJANGO_EXECUTION_ENVIRONMENT` environment variables, we have to also include them in our PAW's WSGI file as well.  That's a PAW requirement (refer to their help page on the topic [here](https://help.pythonanywhere.com/pages/environment-variables-for-web-apps/)).

You can test that your `WSGI` file works OK by running `python -i /var/www/www_my_domain_com_wsgi.py` on the terminal.  Make sure to activate the virtual environment first.  If this shows any errors and won't even load python (e.g., syntax errors), you'll need to fix them. Please visit PAW's excellent [help pages](https://help.pythonanywhere.com/pages/) to troubleshoot any issues that you may encounter.

**TIP!**|You can find the `/var/www/www_my_domain_com_wsgi.py` portion of the above command under the `Code` section of the `Web` tab of your application. Mine looks like so:

![shot]

### Enabling PAW as our django host
_____
We now need to check that PAW is enabled as a host for our django app. We do that by opening our `../settings/production.py` file and editing it appropriately.

I access such file as shown below.

![shot]

Let's make sure that the file reads as follows:

    from .base import *
    ALLOWED_HOSTS = ['.pythonanywhere.com']

If you have your own domain, your domain would replace the `.pythonanywhere.com` portion (as in `'.tumblingprogrammer.com'`).

Save your changes.

### Setting our `media` files path
_____
We now need to tell PAW about the path where our `media` files will be served from. We do that under the `Static files` section of the `Web` tab. Here is what mine looks like:

![shot]

Make sure to edit yours accordingly.

### Running our app
_____
We are now ready to run our app.  We do that by clicking the button and link shown below, and in the order indicated.

![shot]

If everything goes well, your browser should look like the below.

![shot]

You can see the live site [here](http://djangoboilerplate.pythonanywhere.com/ "tumbling programmer - django project boilerplate on pythonanywhere").

This completes our tutorial.  Congratulations! You now know quite a bit about deploying django apps on PAW!








































