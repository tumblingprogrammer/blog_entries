


### Introduction
_____
+ Typical approach

* kkss
* 

This is a follow up article to article blah blah blah 





### Opening an account at [pythonanywhere.com](http://pythonanywhere.com)
_____
If you don't have one, opening an account at pythonanywhere is easy, and you can start with a [beginner account](https://www.pythonanywhere.com/pricing/), which is free.  Go ahead and do that. I have opened one to host the [django project boilerplate app](http://www.tumblingprogrammer.com/setting-up-a-django-project-boilerplate/ "tumbling programmer - setting up a django project boilerplate"), and was lucky enough to find out that `djangoboilerplate` wasn't taken, so I took it.  So far my experience with pythonanywhere (PAW) was been great.  Their customer support really shines, and their giving back to the community is great.

**Tip!** | For a begineer account, your username will also be the subdomain where your app will live.  In my case, for example, the app can be reached at  [http://djangoboilerplate.pythonanywhere.com](http://djangoboilerplate.pythonanywhere.com "tumbling programmer - django project boilerplate app at pythonanywhere.com").

If you haven't done that, go ahead and [sign up for a beginner account](https://www.pythonanywhere.com/registration/register/beginner/). 

### Getting your project files to PAW
_____
To accomplish this, let's go ahead and open a `bash` console. We do that by looking for the dashboard, and then by clicking on the link shown below.

[shot]()

By dedault, your bash console will start your session on your home directory (you will see something like `04:22 ~ $` on the terminal.  If you run `pwd` (the `print working directory` command) at the terminal, it will output something like `/home/[your_username]`.  In my case, it outputs `/home/djangoboilerplate`.  If I run `ls` (list files command), I only get `README.txt`, which is a file that PAW installs there by default.  

The source code for the django project boilerplate lives at [https://github.com/tumblingprogrammer/djangoboilerplate](https://github.com/tumblingprogrammer/djangoboilerplate).  On our home directory (i.e., `/home/[your_username]`), let's run...

    git clone https://github.com/tumblingprogrammer/djangoboilerplate.git
    
Your terminal will output a listing as shown below:

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
    
Notice the new `djangoboilerplate` folder, which is the root folder of the boilerplate.

### Creating our virtual environment
_____

PAW is setup with [virtual environment wrapper](http://virtualenvwrapper.readthedocs.io/en/latest/), which is great as we don't have to deal with getting it installed.  We'll use `djangoboilerplate` as the name of our virtual environemnt.  Let's go ahead and run the following command:

    mkvirtualenv djangoboilerplate --python=/usr/bin/python3.5

After a little while, our terminla prompt should display the name our virtual environment, like so:

    (djangoboilerplate) 05:11 ~ $ 

Which means that our virtual environment is active.

### Setting our environment variables
_____

As part of making our application more secure, and to programmatically tell django that we are in a production environment, we will need to setup two variables: `DJANGO_SECRET_KEY`, which will be limited to the scope of our `djangoboilerplate` virtual environment, and `DJANGO_EXECUTION_ENVIRONMENT`, which can be set system-wide so all our django apps running on PAW know that they are operating in production and that they need to use production settings.

Read [http://www.tumblingprogrammer.com/setting-environment-variables/](http://www.tumblingprogrammer.com/setting-environment-variables/ "tumbling programmer's setting environment variables article")to learn more about the why and how of setting environment variables.

On your PAW dashboard, use the `files` tab to navigate to the `postactivate` file of your `djangoboilerplate` virtual environment, and go ahead and clik on it, to edit it (PAW's default editor will open it up).

Edit it so it reads as follows:

    #!/bin/bash
    # This hook is sourced after this virtualenv is activated.
    
    export DJANGO_SECRET_KEY="your_django_secret_key_goes_here"
    
To generate a value for your `DJANGO_SECRET_KEY` you can use [this site](http://www.miniwebtool.com/django-secret-key-generator/) or, better yet, program your own python script to do it.

Once you edit the file, click on the `save` button on the top right corner of the window, or press 'ctrl` + `l` simultaneously.

Go back to the terminal and make sure to deactivate your `djangoboilerplate` virtual environment (if it is active) by running `deactivate djangoboilerplate` and re-activating it by running `workon djangoboilerplate`.

**WARNING!** | Make sure to use opening and closing quotes (`" "`) on line `DJANGO_SECRET_KEY="your_django_secret_key_goes_here"`. Otherwise, `bash` will complain that it can't find a proper `EOF` closing.

**TIP!**| to make my life easier, I usually have a tab on my browser pointing to PAW's dashboard, and another one with an up-and-running `bash` terminal.

Once your `djangoboilerplate` virtual environment has been reactivated, run `echo $DJANGO_SECRET_KEY`, after which the terminal should output your secret key.

Go back to your PAW's dashboard.  Navigate to your home directory until you find your `.bashrc` profile.  Click to open it, and add the following at the bottom of the file.

    export DJANGO_EXECUTION_ENVIRONMENT="PRODUCTION"

Save the file and go back to the terminal.  Once on it, run `source ~/.bashrc`, which will run commands in the `.bashrc` file will load functions contained in the file into our `bash` shell script, including making our `DJANGO_EXECUTION_ENVIRONMENT` available.  We can test this by running `echo $DJANGO_EXECUTION_ENVIRONMENT` on the terminal, which should output `PRODUCTION`.

**TIP!**| Once we run `source ~/.bashrc`, one of the things that it will do is deactivate our virtual environment.  We will need to reactivate it if in the future we want to execute commands that are directly related to our relevant python environment.

### Installing our requirements
_____
On the terminal, navigate to the `requirements` folder of the application.  The path for it should look like `/home/[your_username]/djangoboilerplate/requirements`.  

**TIP!** You can also use PAW's to get there, as shown below.

![shot]

Once your terminal session is open, make sure to activate the `djangoboilerplate` virtual environment. Once that's done, let's execute `pip install -r production.txt`.  The process should run smoothly and should take a little while.  Once it's done, if we run `pip freeze` we should get the following listing:

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




























