![No longer maintained](https://img.shields.io/badge/Maintenance-OFF-red.svg)
### [DEPRECATED] This repository is no longer maintained
> While this project is fully functional, the dependencies are no longer up to date. You are still welcome to explore, learn, and use the code provided here.
>
> Modus is dedicated to supporting the community with innovative ideas, best-practice patterns, and inspiring open source solutions. Check out the latest [Modus Labs](https://labs.moduscreate.com?utm_source=github&utm_medium=readme&utm_campaign=deprecated) projects.

[![Modus Labs](https://res.cloudinary.com/modus-labs/image/upload/h_80/v1531492623/labs/logo-black.png)](https://labs.moduscreate.com?utm_source=github&utm_medium=readme&utm_campaign=deprecated)

---

Protractor and Selenium Standalone Cookbook
===========================================

This cookbook presently installs the following items for an Ubuntu or Debian
server, to allow headless end to end in-browser testing of AngularJS web
applications via [Protractor][0]:

  * Protractor
  * Selenium Standalone Server
  * Xvfb
  * Chromium
  * Firefox
  * PhantomJS

OS Support
----------

Currently this cookbook offers a limited variety of OS support and is best
suited to provisioning a Vagrant VM, used either for experimentation or in a
build process.

Selenium and Xvfb Services
--------------------------

The protractor-selenium-server::services recipe sets up services for both Xvfb
and the Selenium standalone server.

Xvfb is a minimal X Server implementation to allow headless testing of browsers,
and the services ensure that it and Selenium coordinate sufficiently to run end
to end tests in Chromium and Firefox on a headless server.

Firefox Notes
-------------

Firefox will issue the following complaint if launched standalone on a display
managed by Xvfb:

```
 Xlib: extension "RANDR" missing on display ":10"
```

The recommended solution is to launch Xvfb with this option:

```
-extension RANDR
```

This, however, does not at this time prevent the above error message.
Fortunately the message is not fatal and Firefox will still work just fine.

PhantomJS Notes
---------------

If attempting to use use PhantomJS for testing, note that when using PhantomJS
through Selenium and the Webdriver bindings, as of Q4 2013 there appears to be
no ability to change the log file location to be anything other than
`/phantomjsdriver.log`.

It is an easy change on the command line if invoking PhantomJS directly, but
there is no option that will pass through Protractor or Selenium standalone
configuration. As a workaround this file is created during provisioning and made
globally writable.

There are other blocking issues as of Q4 2013 that prevent easy use of PhantomJS
with Protractor. See, for example:

[https://github.com/angular/protractor/issues/85][1]

Selenium Standalone Server Notes
--------------------------------

There is a known issue with the Selenium standalone server (with Java, really)
in which it [fails to bind to its port on restart][2] after receiving a SIGTERM.
To avoid this you must pass this argument to Java on the command line:

```
-Djava.security.egd=file:/dev/./urandom
```

This is done in the init.d script for Selenium here. If you are starting a
Selenium standalone instance manually in your Protractor script, then you would
have to do something like this:

```
seleniumArgs: ['-Djava.security.egd=file:/dev/./urandom'],
```

[0]: https://github.com/angular/protractor
[1]: https://github.com/angular/protractor/issues/85
[2]: http://stackoverflow.com/questions/14058111/selenium-server-doesnt-bind-to-socket-after-being-killed-with-sigterm
