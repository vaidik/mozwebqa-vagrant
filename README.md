# Vagrant Setup for MozWebQA Projects

This project is to help get your environment setup for contributing to our
WebQA projects as quickly and as easily as possible.

### Base Requirements

- [Vagrant][Vag] - you will need to Vagrant installed in your machine. Get 
  the latest version of Vagrant for your OS [here][VagDL].
- [VirtualBox][VB] - you will also need VirtualBox (or any other virtual 
  machine provider). Get VirtualBox [here][VBDL].

[Vag]: http://www.vagrantup.com/
[VagDL]: http://downloads.vagrantup.com/
[VB]: https://www.virtualbox.org/
[VBDL]: https://www.virtualbox.org/wiki/Downloads

## Usage Guide

### Getting started with Selenium based projects

Most of our WebQA projects use Selenium Webdriver. If you are interested in
contributing to our Selenium based projects, you may want to check out the
following projects:

- [marketplace-tests](https://github.com/mozilla/marketplace-tests)
- [mcom-tests](https://github.com/mozilla/mcom-tests)
- [sumo-tests](https://github.com/mozilla/sumo-tests)
- [addons-tests](https://github.com/mozilla/Addon-Tests)
- [qmo-tests](https://github.com/mozilla/qmo-tests)
- [remo-tests](https://github.com/mozilla/remo-tests)
- [socorro-tests](https://github.com/mozilla/Socorro-Tests)
- [mozillians-tests](https://github.com/mozilla/mozillians-tests)
- [mdn-tests](https://github.com/mozilla/mdn-tests)
- [moztrap-tests](https://github.com/mozilla/moztrap-tests)

#### Requirements for Selenium based projects

- Make sure you have Firefox installed.
- Make sure you have [Java Runtime][JRE] installed.
- Download Selenium Standalone Server JAR from [Selenium on Google Code][GC].

[JRE]: http://java.com/en/download/index.jsp
[GC]: http://code.google.com/p/selenium/downloads/list

#### Setting up Vagrant and the VM

1.  **Get mozwebqa-vagrant**

    1.  If you don't have Git, then download the latest archive from [here][archive].
        Unzip the downloaded file and change directory to `mozwebqa-vagrant-master`.

        ```
        cd mozwebqa-vagrant-master
        ```

    2.  If you have Git installed, then clone the repo:

        ```
	    # clone mozwebqa-vagrant from Github
	    git clone git@github.com:retornam/mozwebqa-vagrant.git

	    # get into the cloned project's directory
	    cd mozwebqa-vagrant
	    ```

2.  **Start Selenium Server**

    On Linux and Mac (you can just double-click as well on Mac to run the JAR):

    ```
    java -jar selenium-server-standalone-<version>.jar
    ```

    On Windows:

    ```
    # Actually I don't know, can someone fill in the right way of doing it?
    ```

3.  **Launch VM using Vagrant**

    ```
    # use vagrant to start the VM with our configuration
    vagrant up

    # ssh into the VM to use it
    vagrant ssh -- -R 4444:localhost:4444
    ```

[archive]: https://github.com/retornam/mozwebqa-vagrant/archive/master.zip

#### Running tests

To get you started, lets take [mcom-tests][mt] as an example. Run the following
commands in the `ssh session` you started using Vagrant in the previous section.

[mt]: https://github.com/mozilla/mcom-tests

```
# change directory to mozilla-projects
cd ~/mozilla-projects

# clone mcom-tests
git clone https://github.com/mozilla/mcom-tests.git

# change directory to mcom-tests
cd mcom-tests

# create a new virtual environment for this project
mkvirtualenv mcom-tests

# install python requirements
pip install -r requirements.txt

# run one test
py.test --baseurl=http://mozilla.org --host=localhost --port=4444 \
	--browsername=firefox --platform=mac \
	tests/test_about.py	-k test_footer_link_destinations_are_correct
```

In the `py.test` command, `--platform` should be the platform on which you are
running Selenium Server. In this case, it is the OS you are using. So if you
are using Linux, set `--platform=linux` and if you are using Mac, then set 
`--platform=mac`.
