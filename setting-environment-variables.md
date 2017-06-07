Environment variables are quite useful (and needed!) on both development and production environments.  They help us to keep things like django's `SECRET_KEY` secret, as well as a host of other uses like aliases, to keep our fingers from tiring due to excessive typing.

We can set system-wide environment variables that, as the name implies, would be available everywhere our Mac OS / Linux / Unix `bash` terminal lives.  An example of such a variable, in a django application setting, would be to store a variable called `DJANGO_EXECUTION_ENVIRONMENT` with a value that tells django whether we are in `development` mode (in which case its value could be, for example, `LOCAL`), or production mode (in which case its value could be, for example, `PRODUCTION`).

Virtual environments enable us to set system variables whose life is limited to when a specific virtual environment is active, which, as an example, is useful for storing django's secret keys that are unique to each project. 

As a programmer, sooner or later you will need to learn how to set them, which is quite simple.

### Setting system-wide variables for the bash shell
_____

For Mac OS (my development OS of choice), I just simply run `nano -w ~/.bash_profile` on the command line, and add the following line to my `.bash_profile` file:

    export SYSTEM_WIDE_VARIABLE="VALUE_OF_MY_SYSTEM_WIDE_VARIABLE"

Note that I used [nano](https://www.nano-editor.org/) as the text editor; feel free to use another.

Once I save the changes, I run `source ~/.bash_profile` on the command line, and then test that it works by running `echo $SYSTEM_WIDE_VARIABLE`, the result of which, if successful, should be `VALUE_OF_MY_SYSTEM_WIDE_VARIABLE`.

If you use a system other than Mac OS, switch ASAP! :) Nah, just kidding. A relevant google search will point you in the right direction.  

### Setting variables limited to virtual environments 
_____ 

I highly recommend that you use [virtualenv wrapper](https://virtualenvwrapper.readthedocs.io/en/latest/) to manage your virtual environments for your django and python projects. I highly recommend also that for every application you setup a different virtual environment. To set a variable on your virtual environment, go to the bin folder of your `virtualenv`.  For me, the path to the bin folder looks like `/Users/puma/.virtualenvs/djangoboilerplate/bin`.  Within it there is a file called `postactivate`; edit it as follows:

    #!/bin/bash
    # This hook is sourced after this virtualenv is activated.
    
    #your VIRTUAL_ENVIRONMENT_VARIABLE goes here
    export VIRTUAL_ENVIRONMENT_VARIABLE="VALUE_OF_YOUR_VIRTUAL_ENVIRONMENT_VARIABLE"

As the second line informs us, whatever is in that file will be sourced post activation of the virtual environment.

Make sure that what you just did works by deactivating and reactivating your virtual environment, and then by running `echo $VIRTUAL_ENVIRONMENT_VARIABLE` on your command line. It should output `VALUE_OF_YOUR_VIRTUAL_ENVIRONMENT_VARIABLE` when the virtual environment is active, and nothing when it is not.

You can see all the above at work on the [setting up a django project boilerplate](http://www.tumblingprogrammer.com/setting-up-a-django-project-boilerplate/ "tumbling programmer's setting up a django project boilerplate tutorial") tutorial.  On that tutorial, I show how to retrieve the value of the variables with python.
