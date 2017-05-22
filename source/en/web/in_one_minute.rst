One minute start
================

If you want to quickly use Zan PHP framework in just one minute, we
recommend that you use the zan-installer.

Installation
------------

The recommended way to install zan-installer is through
`Composer <http://getcomposer.org>`__.

.. code:: bash

    # Install Composer
    curl -sS https://getcomposer.org/installer | php

See `Composer Getting
Started <https://getcomposer.org/doc/00-intro.md>`__

.. code:: bash

    composer global require youzan/zan-installer

The composer repository on packagist.org:
`youzan/zan-installer <https://packagist.org/packages/youzan/zan-installer>`__
。

Update
------

Use the following command to update zan-installer:

.. code:: bash

    composer global update

Use
---

Now, you can open your favorite Terminal tool, type ``zan`` to use!

.. code:: bash

    ~/Sites zan↵
       __    __
      /\ \  /\ \
      \ `\`\\/'/ ___   __  __  ____      __      ___
       `\ `\ /' / __`\/\ \/\ \/\_ ,`\  /'__`\  /' _ `\
         `\ \ \/\ \L\ \ \ \_\ \/_/  /_/\ \L\.\_/\ \/\ \
           \ \_\ \____/\ \____/ /\____\ \__/.\_\ \_\ \_\
            \/_/\/___/  \/___/  \/____/\/__/\/_/\/_/\/_/
    Create a new ZanPhp application.
    Which type application would you create? (use <space> to select)
    ❯ ● HTTP
      ○ TCP
    Your application name: (ex: zanphp-demo) demo
    Your application namespace: (ex: zanphp/zanhttp) youzan/demo
    Please input a output directory:
    ~/Sites
    Downloading the source code archive ...
    Extracting archive ...
    Congratulations, your application has been generated to the 
    following directory.
    ~/Sites/demo/
    See ~/Sites/demo/README.md for information on how to run.
    Composer installing...
    Loading composer repositories with package information
    Updating dependencies (including require-dev)
      - Installing zanphp/zan (dev-master o1o0x2x4)
        Cloning b6d8d443a7a3545a3d1796b39e54fcbc2d276a10

    Writing lock file
    Generating autoload files

Also see
--------

-  `Youzan Zan PHP framework ✈ <https://github.com/youzan/zan>`__
-  `ZanPhp HTTP demo for zan framework
   ✈ <https://github.com/youzan/zanhttp>`__

License
-------

The zan-installer is open-sourced software licensed under the Apache-2.0
license.
