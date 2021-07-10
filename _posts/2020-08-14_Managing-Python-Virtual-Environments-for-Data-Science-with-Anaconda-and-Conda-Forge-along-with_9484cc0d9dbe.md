# Managing Python Virtual Environments for Data Science with Anaconda and Conda-Forge along with…

(This article assumes Linux / Unix setup)

(This article assumes Linux / Unix setup)

![](https://cdn-images-1.medium.com/max/1200/1*Bonwa6ovdX80KeEZFxMaRA.jpeg)
undefined

### What is an Environment in Python

An environment is a way of starting with a new Python installation, that doesn’t look at your already installed packages. In this way, it simulates having a fresh install of Python. If two applications require different versions of Python, you can simply create different environments for them. If you start from a fresh environment and install as you go, you are able to generate a list of all packages installed in the environment so that others can easily duplicate it.

There are few steps to using an environment (with a third step needed if you want to use Jupyter notebooks)

*   Creating the environment, either from scratch (a new project) or from a yaml file (duplicating an environment)
*   Activating the environment for use.
*   Register the environment with Jupyter.
*   To leave an environment, we have to deactivate it.

### Why Environment management is so important

Desciplined environment management practices reduce dependency version conflicts between your projects and keep the base/root development environment from becoming bloated and unmanageable, helping users to create reproducible projects.

Its also recommend to always install your packages inside a new environment instead the base/root environment from anaconda/miniconda. Using envs make it easier to debug problems with packages and ensure the stability of your root env.

### What is the difference between python virtualenv and a conda environment?

Conda environments are essentially a replacement for virtualenv. Both conda environments and virtualenv are aimed at creating an “environment” with isolated package installs. They aim to solve the problem of having multiple python projects on the same system with conflicting package requirements. If you have project A that requires tensorflow-1.8 and project B that uses tensorflow-1.9, these environments allow you to have the right version installed for each project.

### pip vs conda

**pip** is a package manager that facilitates installation, upgrade, and uninstallation of **python packages**. It also works with virtual **python** environments. So pip is about

*   Python packages only.
*   Compiles everything from source. **EDIT: pip now installs binary wheels, if they are available.**
*   Blessed by the core Python community (i.e., Python 3.4+ includes code that automatically bootstraps pip).

**conda,** on the other hand is a package manager for **any software** (installation, upgrade and uninstallation). It also works with virtual **system** environments.

One of the goals with the design of conda is to facilitate package management for the entire software stack required by users, of which one or more python versions may only be a small part. This includes low-level libraries, such as linear algebra, compilers, such as mingw on Windows, editors, version control tools like Hg and Git, or _whatever else requires distribution and management_.

For version management, pip allows you to switch between and manage multiple **python** environments.

Conda allows you to switch between and manage **multiple general purpose environments** across which multiple other things can vary in version number, like C-libraries, or compilers, or test-suites, or database engines and so on.

**So conda is**

*   Python agnostic. The main focus of existing packages are for Python, and indeed Conda itself is written in Python, but you can also have Conda packages for C libraries, or R packages, or really anything.
*   Installs binaries. There is a tool called `conda build` that builds packages from source, but `conda install`itself installs things from already built Conda packages.
*   External. Conda is the package manager of Anaconda, the Python distribution provided by Continuum Analytics, but it can be used outside of Anaconda too. You can use it with an existing Python installation by pip installing it (though this is not recommended unless you have a good reason to use an existing installation).

A key difference between the two tools is that conda has the ability to create isolated environments that can contain different versions of Python and/or the packages installed in them. This can be extremely useful when working with data science tools as different tools may contain conflicting requirements which could prevent them all being installed into a single environment. Pip has no built in support for environments but rather depends on other tools like virtualenv or venv to create isolated environments. Tools such as pipenv, poetry, and hatch wrap pip and virtualenv to provide a unified method for working with these environments.

Next to the root environment, you can create as many additional environments as you want. And the whole point is that these additional environments can contain different versions of Pythons and other packages. So it means that, for example, if your precious little application is not working anymore in the newest, state-of-the-art environment you’ve just set up, you can always go “back” and use some another version(s) of some packages (including Python — Python itself is a package

**Conda and pip serve different purposes, they are NOT competitors** and only directly compete in a small subset of tasks: namely installing Python packages in isolated environments.

#### Anaconda Python: where are the conda virtual environments stored?

In Linux your environments are located in `Anaconda3\envs\<yourEnv_directory>`

You can also run `conda info` which will give you details

![](https://cdn-images-1.medium.com/max/800/1*F-_3abvvLaOpWrPVVta2cw.png)
undefined

### Now lets get started with all the commands to create and manage conda environments.

![](https://cdn-images-1.medium.com/max/800/1*_jiyl5LirniZc2-8C1q2rg.jpeg)

#### First create a Python virtual environment

`conda create --name env_name python=3.7`

Confirm its creation by finding its location in your file system at **~/anaconda3/envs/env\_name.** Any downloads for the environment will be located here.

The above line literally says: Create an environment using the conda create command whose name is env\_name (you can choose any name) that runs on python=3.7

Because we used a single ‘=’, we tell conda to use the latest version in the Python 3.7 tree. If we would have used two equal signs ‘==’ we would tell conda to give exactly version 3.7, so there is a subtle difference.

#### Command To Make exact copy of an environment

conda create — clone py35 — name py35–2

### Downloading from specific Channel while creating a new environment

When I run below to install [**jupyterlab/debugger**](https://github.com/jupyterlab/debugger)

`conda create -n jupyterlab-debugger -c conda-forge xeus-python=0.8.0 notebook=6 jupyterlab=2 ptvsd nodejs`

**In above the “-c” stands for channel — meaning with above command jupyterlab-debugger will be downloaded from conda-forge channel**

**Then activate that with** `**conda activate jupyterlab-debugger**`

Then, run the following command to install the extension:

`jupyter labextension install @jupyterlab/debugger`

### Pre and Post Activation python bin file location

**First note before activating any conda environment (i.e. when I am in pip environment) if you run below**

`which python`

It would give `/usr/bin/python`

**Then activate a conda environment e.g. with**

`**conda activate jupyterlab-debugger**`

And then run `which python` in the same terminal window

And it would give

```
(jupyterlab-debugger) paul@paul-B85M-D3H:~$ which python/home/paul/anaconda3/envs/jupyterlab-debugger/bin/python
```

### Activating a new environment

**For work on conda 4.6 and later versions**

`conda activate` and `conda deactivate`

**For conda versions prior to 4.6, run:**

Windows: `activate` or `deactivate`

Linux and macOS: `source activate` or `source deactivate`

#### Connecting my new environment with Jupyter Notebook

If you want Jupyter notebooks to see your new environment, you need a couple of extra instructions. Jupyter sees the different environments as different kernels. Once we create a new environment, we need to tell Jupyter that it is there:

**Note you’ll want to do this in the new environment.**

**First we will need the ipykernel package**

`conda install ipykernel`

This tells jupyter to take the current environment (test\_env) and make a “kernel” option named “test kernel” in the kernel menu

`python -m ipykernel install --user --name myenv --display-name "test kernel"`

For example if my newly created env name is **“jupyterlab-debugger”** I need to run the below command (which is slightly simplified version of the above)

`python -m ipykernel install --user --name=jupyterlab-debugger`

#### To delete or remove the environment, type the following in your terminal:

`conda remove --name env_name --all`

#### Make my new environment exportable and usable by others

Now you want to make an environment.yaml file that will allow others to recreate the environment from scratch. To make this file, we use the export command and send the output to environment.yaml:

**While in test\_env in your Terminal, export the packages used to an environment file**

`conda env export > environment.yaml`

Once we are done with the environment, we can deactivate and delete the environment:

**Leave the environment**

**For conda 4.6 and later versions.**

`conda activate env-name` and `conda deactivate`

**For conda versions prior to 4.6, run:**

Windows: `activate` or `deactivate`

Linux and macOS: `source activate` or `source deactivate`

**Now we are no longer in test\_env, we can delete it**

`conda env remove --name test_env`

#### Making the environment again from the yaml file

If you have the yaml file (created from conda env export), then recreating the environment is a single command:

`conda env create --file environment.yaml`

Note that you don’t need to supply the name of the new environment, as the yaml file also contains the name of the environment it saved. Make sure you don’t give your environment an embarassing name, as everyone who recreates from the yaml file will see the name you used!

#### Finding conda environments on your system

Of course, you may choose to deactivate your environment but keep it around for later. If you want to see the environments installed on your system, use

`conda env list`

### **The concept of Channels — Should conda, or conda-forge be used for Python environments**

#### First What are Conda channels?

Channels are the locations of the repositories (directories) online containing Conda packages. Upon Conda’s installation, Continuum’s (Conda’s developer) channels are set by default, so without any further modification, these are the locations where your Conda will start searching for packages.

#### Default Channels

Conda doesn’t just have a single repository where all uploaded packages live. When a package is uploaded to conda, it must be uploaded to a specific channel, which is just a separate URL where packages published to that channel reside.

There is a default conda channel where the stock conda packages live. These packages are maintained by conda and are generally very stable. If you do not specify channels in your configuration settings, whenever you run conda install x, conda will search its default channel for that package; if the package isn’t on that channel, it will throw an error.

#### Basics Rules of Conda Channels

Channels in Conda are ordered. The channel with the highest priority is the first one that Conda checks, looking for the package you asked for. You can change this order, and also add channels to it (and set their priority as well).

If multiple channels contain a package, and one channel contains a newer version than the other one, the order of the channels’ determines which one of these two versions are going to be installed, even if the higher priority channel contains the older version.

As an example, to install a package (say **samtools**) that is inside a channel that is on your channel list, run this command (if you don’t specify which version you want, it’ll automatically install the latest available version from the highest priority channel):

`conda install samtools`

**You can also specify the package’s version:**

`conda install samtools=1.9`

#### So should I use conda, or conda-forge for Python environments?

The short answer is you can use either, however conda-forge is recommended in most cases.

The long answer:

So `conda-forge` is an additional channel from which packages may be installed. In this sense, it is not any more special than the default channel, or any of the other hundreds (thousands?) of channels that people have posted packages to. You can add your own channel if you sign up at [https://anaconda.org](https://anaconda.org/) and upload your own Conda packages.

Anaconda Inc. (formerly Continuum IO), the main developers of the `conda` software, also maintain a separate channel of packages, which is the default when you type `conda install packagename` without changing any options.

**To see your channel configuration, you can write:**

conda config --show channels

And this details are coming from `~/.condarc` file.

**OR you can also run below to see list out the active channels and their priorities**

`conda config --get channels`

You can control the order that channels are searched with `conda config`. You can write:

\`conda config --add channels some-channel\`

to add the channel `some-channel` to the top of the `channels` configuration list. This gives `some-channel` the highest priority. Priority determines ([in part](http://conda.pydata.org/docs/channels.html)) which channel is selected when more than one channel has a particular package.

**To add the channel to the end of the list and give it the lowest priority, type**

conda config --append channels some-channel

Thats why many people recommend — append channel insteading of — add adding them

\`conda config --append channels CHANNEL\_NAME\`

Why: If you do:

\`conda config --add channels conda-forge\`

This will make conda-forge first hit channel. Your anaconda’s default channel will get lower priority. Some of your packages will start updating to conda-forge.

Instead, do this:

\`conda config --append channels conda-forge\`

This keeps your default channel high in priority. Packages will be searched on your default before going to conda-forge

**If you would like to remove the channel that you added, you can do so by writing**

conda config --remove channels some-channel

See

conda config -h

for more options.

With all of that said, there are four main reasons to use the `conda-forge` channel instead of the `defaults`channel maintained by Anaconda:

1.  Packages on `conda-forge` _may_ be more up-to-date than those on the `defaults` channel. The `conda-forge` channel is where you can find packages that have been built for conda but yet to be part of the official Anaconda distribution.
2.  There are packages on the `conda-forge` channel that aren't available from `defaults`

#### Command to download from specific Channel

When I run below to install [**jupyterlab/debugger**](https://github.com/jupyterlab/debugger)

`conda create -n jupyterlab-debugger -c conda-forge xeus-python=0.8.0 notebook=6 jupyterlab=2 ptvsd nodejs`

**In above the “-c” stands for channel — meaning with above command jupyterlab-debugger will be downloaded from conda-forge channel**

Then activate that with `conda activate jupyterlab-debugger`

Then, run the following command to install the extension:

`jupyter labextension install @jupyterlab/debugger`

#### To update Anaconda itself to the latest version.

Windows: Open the Start Menu and choose Anaconda Prompt.  
macOS or Linux: Open a terminal window.

Enter these commands:

```
conda update condaconda update anaconda=VersionNumber
```

#### To updates all packages in the current environment to the latest version.

`conda update --all`

will unpin everything. In doing so, it drops all the version constraints from the history and tries to make everything as new as it can.

[From Official Doc](https://docs.anaconda.com/anaconda/install/update-version/)

This has the same behavior with removing packages. If any packages are orphaned by an update, they are removed. conda update — all may not be able to make everything the latest versions because you may have conflicting constraints in your environment.

`conda update --all` will only update the selected environment. If you have other environments you’d like to update, you can update them in the command line:

`conda update -n myenv --all`

When you use conda update pkgName or conda install pkgName, conda may not be able to update or install that package without changing something else you specified in the past.

In the case of the Anaconda metapackage, when you say conda update ipython but you have Anaconda 2019.03, conda can and should “downgrade” Anaconda to the “custom” version so that iPython can be updated.

### Further explanations on `conda update --all`

What 95% of People actually want in most cases when they say that I want to update Anaconda is to execute the command:

`conda update --all`

This will update all packages in the current environment to the latest version — with the small print being that it may use an older version of some packages in order to satisfy dependency constraints (often this won’t be necessary and when it is necessary the package plan solver will do its best to minimize the impact).

#### If you are only interested in updating an individual package

`conda update package-1 package-2`

OR for a specific environment

`conda update -n env-name package-1`

### Difference between `conda update anaconda` and `conda update --all`

**Update Anaconda**

Anaconda is a Python distribution that bundles together a ton of packages. Presumably, a bunch of testing goes into verifying that all the package versions and builds are compatible with each other. Because this takes time to do, the Anaconda team only releases new distributions (i.e., a new anaconda version) every couple months or so. So, **If you want a stable set of packages that have been tested for interoperability, then do**

`conda update anaconda`

**Update All**

In between Anaconda releases, new versions of many packages are still released on the Anaconda channel, and if you run `conda update --all` you're going to inevitably get ahead of the versions specified in the anaconda bundle. If you want to live on the cutting edge, with package builds that maybe aren't thoroughly tested for integration, then run `conda update --all`.  
However in this case, many people prefer **Conda Forge**, because it tends to have newer package releases.

**In my opinion, there’s no point to installing Anaconda if you’re going to switch most packages to Conda Forge anyway. Instead, just install Miniconda and only install what you want from Conda Forge at the start.**

### Now the most frequently used commands that you will use regularly

![](https://cdn-images-1.medium.com/max/800/1*Xuw4aA2ufkWDN1xadJ6slA.jpeg)

#### First lets look at what t[he Official Doc](https://docs.conda.io/projects/conda/en/latest/commands/install.html) says

```
conda install [-h] [--revision REVISION] [-n ENVIRONMENT | -p PATH]                     [-c CHANNEL] [--use-local] [--override-channels]                     [--repodata-fn REPODATA_FNS] [--strict-channel-priority]                     [--no-channel-priority] [--no-deps | --only-deps]                     [--no-pin] [--copy] [-C] [-k] [--offline] [-d] [--json]                     [-q] [-v] [-y] [--download-only] [--show-channel-urls]                     [--file FILE] [--force-reinstall]                     [--freeze-installed | --update-deps | -S | --update-all | --update-specs]                     [-m] [--clobber] [--dev]                     [package_spec [package_spec ...]]
```

From the above some of the most frequently used aliase are

**\-n, --name => Name of environment**

**\-c, --channel** => channel to search for packages

### General Basic Commands

`conda --version`

`which conda`

`conda info` which will give you details of your conda

#### List all existing environments

You can run `conda info --envs`, and that will show the paths to all your environments

**Most recommended way to install any new package e.g. TensorFlow**

`conda install -c conda-forge tensorflow`

Because `**conda-forge**` channel has few benefits over the regular conda channel

**If you want to install a specific package inside a specific conda environment, you can use the following command.**

First activate the conda environment and then do:

```
conda install --name <conda_env_name> -c <channel_name> <package_name>
```

For a concrete example, let’s assume that you want to install [chainer](http://chainer.org/) from the _channel_ `anaconda` to an already created conda environment named `chainerenv`, then you can do:

`conda install --name chainerenv -c anaconda chainer`

#### To list all of the packages in the active environment, use:

`conda list`

for the packages that pip recognizes, type : `pip list`

**To list all of the packages in a deactivated environment, use:**

`conda list -n myenv`

### Commands to check/verify if a specific package is installed:

`conda list -n jupyterlab-debugger html5lib` => For a specific environment

conda list html5lib\` => For current environment

**which outputs something like this if installed:**

```
# packages in environment at C:\ProgramData\Anaconda3:    #    # Name                    Version                   Build  Channel    html5lib                  1.0.1                    py37_0
```

**or something like this if not installed:**

```
# packages in environment at C:\ProgramData\Anaconda3:    #    # Name                    Version                   Build  Channel
```

you don’t need to type the exact package name. Partial matches are supported:

conda list html

This outputs all installed packages containing ‘html’:

```
# packages in environment at C:\ProgramData\Anaconda3:    #    # Name                    Version                   Build  Channel    html5lib                  1.0.1                    py37_0    sphinxcontrib-htmlhelp    1.0.2                      py_0    sphinxcontrib-serializinghtml 1.1.3                      py_0
```

Another example for my `xeus-python` installation (which is installed in my `jupyterlab-debugger` environment but no in my base environment )

#### Display Conda environment information

`conda info`

![](https://cdn-images-1.medium.com/max/800/1*dReKz17ydldr860pv6Ad6w.png)
undefined

**List all existing environments**

`conda env list`

### Environment management Related Commands

**Create a new environment named py35, install Python 3.5**

`conda create --name py35 python=3.5`

**Activate AND DeActivate — the new environment to use it**

**For conda 4.6 and later versions.**

`conda activate env-name` and `conda deactivate`

**For conda versions prior to 4.6, run:**

Windows: `activate` or `deactivate`

Linux and macOS: `source activate` or `source deactivate`

**Make exact copy / clone of an environment**

conda create — clone py35 — name py35–2

#### Downloading from specific Channel

When I run below to install [**jupyterlab/debugger**](https://github.com/jupyterlab/debugger)

`conda create -n jupyterlab-debugger -c conda-forge xeus-python=0.8.0 notebook=6 jupyterlab=2 ptvsd nodejs`

**In above the “-c” stands for channel — meaning with above command jupyterlab-debugger will be downloaded from conda-forge channel**

Then activate that with `conda activate jupyterlab-debugger`

Then, run the following command to install the extension:

`jupyter labextension install @jupyterlab/debugger`

### To delete or remove the environment, type the following in your terminal:

`conda remove --name env_name --all`

#### To find the location of the `.condarc` file

`conda config --show-sources`

### Cleaning up

You can free some space with:

`conda clean --all`

That will clean and remove unused packages and caches.

**Removing Packages**

Use the terminal or an Anaconda Prompt for the following steps.

To remove a package such as SciPy in an environment such as myenv:

`conda remove -n myenv scipy`

To remove a package such as SciPy in the current environment:

`conda remove scipy`

To remove multiple packages at once, such as SciPy and cURL:

`conda remove scipy curl`

To confirm that a package has been removed:

`conda list`

And those are all the commands that I had to use on a daily basis.

By [Rohan Paul](https://medium.com/@paulrohan) on [August 14, 2020](https://medium.com/p/9484cc0d9dbe).

[Canonical link](https://medium.com/@paulrohan/managing-python-virtual-environments-for-data-science-with-anaconda-and-conda-forge-along-with-9484cc0d9dbe)

Exported from [Medium](https://medium.com) on December 12, 2020.