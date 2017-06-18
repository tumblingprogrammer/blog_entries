### Introduction
_____
Windows 7



### Conventions throughout this tutorial
_____
Please review the [conventions page](http://www.tumblingprogrammer.com/conventions-used-on-tumbling-programmer-dot-com/ "tumbling programmer's conventions page"), which will help you understand how I communicate with you through this blog.


### Installing git
_____
Let's install [git for windows](https://git-scm.com/download/win). As of this writing, the latest version is 2.13.1.

Below are the screenshots of the options that I chose during the installation process:

![ tumbling programmer - setting windows for python development - git - select components ]( https://www.tumblingprogrammer.com/media/2017/06/git-select-components.png " tumbling programmer - setting windows for python development - git - select components ")

From the above, it's critical to select the `Git Bash Here` option.  It will give you the ability to run *nix like bash commands on windows (like `ls`, `grep`, etc.).

![ tumbling programmer - setting windows for python development - git - adjusting your path environment ]( https://www.tumblingprogrammer.com/media/2017/06/git-adjusting-your-path-environment.png " tumbling programmer - setting windows for python development - git - adjusting your path environment ")

Ignore the red font and the warning (unless you are an advanced user of the windows command prompt; most people aren't).

![ tumbling programmer - setting windows for python development - git - choosing the ssh executable ]( https://www.tumblingprogrammer.com/media/2017/06/git-choosing-the-ssh-executable.png " tumbling programmer - setting windows for python development - git - choosing the ssh executable ")

![ tumbling programmer - setting windows for python development - git - choosing https transport backend ]( https://www.tumblingprogrammer.com/media/2017/06/git-choosing-https-transport-backend.png " tumbling programmer - setting windows for python development - git - choosing https transport backend ")

![ tumbling programmer - setting windows for python development - git - configuring the line ending conventions ]( https://www.tumblingprogrammer.com/media/2017/06/git-configuring-the-line-ending-conventions.png " tumbling programmer - setting windows for python development - git - configuring the line ending conventions ")

![ tumbling programmer - setting windows for python development - git - configuring the terminal emulator to use with git bash ]( https://www.tumblingprogrammer.com/media/2017/06/git-configuring-the-terminal-emulator-to-use-with-git-bash.png " tumbling programmer - setting windows for python development - git - configuring the terminal emulator to use with git bash ")

![ tumbling programmer - setting windows for python development - git - configuring extra options ]( https://www.tumblingprogrammer.com/media/2017/06/git-configuring-extra-options.png " tumbling programmer - setting windows for python development - git - configuring extra options ")

Learning to use git or configuring it is beyond the scope of this tutorial.  There is plenty of info out there, including [Getting Started - Git Basics](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics), [Getting Started - First-Time Git Setup](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup) and [Customizing Git - Git Configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration).

You now should be in a position to open a `bash` terminal by right-clicking on any Windows Explorer folder, and selecting the option shown below.

![ tumbling programmer - setting windows for python development - git - git bash here ]( https://www.tumblingprogrammer.com/media/2017/06/git-git-bash-here.png " tumbling programmer - setting windows for python development - git - git bash here ")

![ tumbling programmer - setting windows for python development - git - git bash here ]( https://www.tumblingprogrammer.com/media/2017/06/git-git-bash-here.png " tumbling programmer - setting windows for python development - git - git bash here ")

**TIP!** | When on Windows, I usually start working on my projects by right-clicking on the python project folder, and selecting the `Git Bash Here` option.  

### Installing python 2
_____


Below are the options that I chose when I installed it on my system.

![ tumbling programmer - setting windows for python development - python 2 - select scope of installation ]( https://www.tumblingprogrammer.com/media/2017/06/python2-select-scope-of-installation.png " tumbling programmer - setting windows for python development - python 2 - select scope of installation ")

![ tumbling programmer - setting windows for python development - python 2 - customize python ]( https://www.tumblingprogrammer.com/media/2017/06/python2-customize-python.png " tumbling programmer - setting windows for python development - python 2 - customize python ")

Make sure to select the `Add python.exe to Path` so you can run python directly from the bash terminal (or the windows command prompt) without having to provide the whole path (i.e., this will allow you to run... 

`./python name_of_your_python_script.py` 

as opposed to...

`/c/Python27/python name_of_your_python_script.py`

Notice that for the above example I used `bash` notation, which includes forward-slash for directory separators.

Let's test that python works OK by opening a `bash` terminal anywhere and by running `which python` and `python --version`.  On my system I get the following:

    jose.pumar@host ~
    $ which python
    /c/Python27/pytho
    
    jose.pumar@host ~
    $ python --version
    Python 2.7.13

### Installing virtual environment and virtual environment wrapper
______
While on the `bash` terminal, let's install `virtualenv` and `virtualenvwrapper`.  Below is what it looked like for me:

**virtualenv**

    $ pip install virtualenv
    Collecting virtualenv
      Using cached virtualenv-15.1.0-py2.py3-none-any.whl
    Installing collected packages: virtualenv
    Successfully installed virtualenv-15.1.0

**virtualenvwrapper**   

    $ pip install virtualenvwrapper
    Collecting virtualenvwrapper
      Using cached virtualenvwrapper-4.7.2.tar.gz
    Requirement already satisfied: virtualenv in c:\python27\lib\site-packages (from
     virtualenvwrapper)
    Collecting virtualenv-clone (from virtualenvwrapper)
      Using cached virtualenv-clone-0.2.6.tar.gz
    Collecting stevedore (from virtualenvwrapper)
      Using cached stevedore-1.23.0-py2.py3-none-any.whl
    Collecting six>=1.9.0 (from stevedore->virtualenvwrapper)
      Using cached six-1.10.0-py2.py3-none-any.whl
    Collecting pbr!=2.1.0,>=2.0.0 (from stevedore->virtualenvwrapper)
      Using cached pbr-3.0.1-py2.py3-none-any.whl
    Installing collected packages: virtualenv-clone, six, pbr, stevedore, virtualenv
    wrapper
      Running setup.py install for virtualenv-clone ... done
      Running setup.py install for virtualenvwrapper ... done
    Successfully installed pbr-3.0.1 six-1.10.0 stevedore-1.23.0 virtualenv-clone-0.
    2.6 virtualenvwrapper-4.7.2
    
Once `virtualenvwrapper` is installed, let's add the `virtualenvwrapper.sh` script to our `.bashrc` file by running:

    echo "source virtualenvwrapper.sh" >> ~/.bashrc
    
Let's source our `.bashrc` by running:

    source ~/.bashrc

Below is the output that I got:

    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\premkproject
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\postmkproject
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\initialize
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\premkvirtualenv
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\postmkvirtualenv
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\prermvirtualenv
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\postrmvirtualenv
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\predeactivate
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\postdeactivate
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\preactivate
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\postactivate
    virtualenvwrapper.user_scripts creating C:\Users\jose.pumar\.virtualenvs\get_env_details

We are ready to test if `virtualenvwrapper` works by running `mkvirtualenv test`, which createst a virtual environment called `test`, and which is stored under `C:\Users\[your_user_name]\.virtualenvs\`. All your virtual environments will be stored there. Once the script is finished running, my terminal outputs the following:

    (test)
    jose.pumar@host MINGW64 ~
    $

Notice the name of the virtual environment that we just created (i.e., `(test)`), which signifies that our virtual environment is active.

Let's go ahead and deactivate and delete the virtual environment by running `deactivate test` and `rmvirtualenv test`.  Another useful command is `lsvirtualenv`, which lists all virtual environments in your system.

### Installing python 3
_____

![ tumbling programmer - setting windows for python development - python 3 - install python ]( https://www.tumblingprogrammer.com/media/2017/06/python3-install-python.png " tumbling programmer - setting windows for python development - python 3 - install python ")

![ tumbling programmer - setting windows for python development - python 3 - advanced installation options ]( https://www.tumblingprogrammer.com/media/2017/06/python3-advanced-installation-options.png " tumbling programmer - setting windows for python development - python 3 - advanced installation options ")

Note that on my setup I had already chosen to add python 2 to my Path in windows.  Because of that, adding Python 3 to the `PATH`, as shown above, doesn't make much sense - you should only have either python 2 or python 3 but not both for one of them will not work anyways.  That's where having `virtualenvwrapper` is handy, for when we create a virtual env we can specify which python version to use, and our virtual environment will remeber that for us.

Note also the path I chose to install python 3 under.  The default that the installer picks is buried deep in a typical windows user app folder.  I purposedly chose an easier path, similar to the one that the python 2 installer picked.









![ tumbling programmer's deploying the boilerplate app to pythonanywhere - PAW dashboard - opening a bash console ]( https://www.tumblingprogrammer.com/media/2017/06/pythonanywhere-opening-a-bash-console.png " tumbling programmer's deploying the boilerplate app to pythonanywhere - PAW dashboard - opening a bash console ")
