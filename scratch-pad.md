


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

As part of making our application more secure, and to programmatically tell django that we are in a production environment, we will need to setup two variables: `DJANGO_SECRET_KEY`, which will be limited to the scope of our `djangoboilerplate` virtual environment, and 















