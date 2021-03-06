.. _contributing:

Contributing
============

.. image:: img/contributing.png
    :align: center
    :alt: contributing
    :target: http://mimiandeunice.com/

What can you do to help this project ?
--------------------------------------

* `Star it on GitHub <https://github.com/elabftw/elabftw>`_
* Talk about it to your colleagues, spread the word!
* Have a look at `the current issues <https://github.com/elabftw/elabftw/issues>`_
* Help with the translation
* Open GitHub issues to discuss bugs or suggest features

.. image:: img/i18n.png
    :align: right

Translating
-----------

Do you know several languages? Are you willing to help localize eLabFTW? You're in the right place.

How to translate ?

* `Join the project on poeditor.com <https://poeditor.com/join/project?hash=aeeef61cdad663825bfe49bb7cbccb30>`_
* Select elabftw
* Add a language (or select an existing one)
* Start translating
* Ignore things like `<strong>, <br>, %s, %d` and keep the ponctuation like it was!

Translations are updated before a release.

Contributing to the code
------------------------

Environment installation
````````````````````````
* Fork `the repository <https://github.com/elabftw/elabftw>`_ on GitHub
* From your fork page, clone it on your machine (let's say it's in `/home/user/elabftw`)
* :ref:`Install eLabFTW <install>` normally (with **elabctl**) but don't start it
* Git clone the docker image repo and build a dev image:

.. code-block:: bash

    git clone -b dev https://github.com/elabftw/elabimg
    cd elabimg
    docker build -t elabftw/dev .

* Edit the docker-compose configuration file `/etc/elabftw.yml`
* Change the `volumes:` line so it points to the `elabftw` folder, not `elabftw/uploads`. So the line should look like : /home/user/elabftw:/elabftw
* Change the line `image:` and replace it with `elabftw/dev`
* In the elabftw folder install composer dependencies: `composer install`
* Optionaly change the port binding from 443 to something else (example: 9999:443)
* Start the containers:

.. code-block:: bash

   sudo elabctl start

* You now should have a running local eLabFTW, and changes made to the code will be immediatly visible

Making a pull request
`````````````````````
#. Before working on a feature, it's a good idea to open an issue first to discuss its implementation
#. Create a branch from **hypernext**
#. Work on a feature
#. Make a pull request on GitHub to include it in hypernext

Code organization
`````````````````
* Real accessible pages are in the root directory (experiments.php, database.php, login.php, etc…)
* The rest is in app/
* app/models will contain classes with CRUD (Create, Read, Update, Destroy)
* app/views will contain classes to generate and display HTML
* app/classes will contain services or utility classes
* a new class will be loaded automagically thanks to the use of PSR-4 with composer (namespace Elabftw\\Elabftw)
* app/controllers will contain pages that send actions to models (like destroy something), and generally output json for an ajax request, or redirect the user.

Ðependencies
````````````
* PHP dependencies are managed through `Composer <https://getcomposer.org/>`_
* JavaScript dependencies are managed through `Bower <https://bower.io/>`_

  Install them with:

.. code-block:: bash

    # php dependencies
    composer install
    # javascript dependencies
    bower install jquery jquery-ui bootstrap colorpicker fancybox jeditable jquery.complexify.js keymaster moment fullcalendar tinymce tinymce-mention dropzone file-saver.js

i18n
````
* for internationalization, we use gettext
* i18n related things are in the `locale` folder
* the script `locale/genpo.sh` is used to merge the french .po file from extracted strings
* if you add a string shown to the user, it needs to be gettexted _('like this')

Miscellaneous
`````````````
* if you make a change to the SQL stucture, you need to put add an update function in `app/classes/Update.php` and also modify `install/elabftw.sql` accordingly
* instead of adding your functions to `app/functions.inc.php`, create a proper class
* you can use the constant ELAB_ROOT (which ends with a /) to have a full path
* comment your code wisely
* your code must follow `the PSR standards <https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md>`_
* add a plugin to your editor to show trailing whitespaces in red
* add a plugin to your editor to show PSR-1 errors
* remove BOM
* if you want to work on the documentation, clone the `elabdoc repo <https://github.com/elabftw/elabdoc>`_

Grunt
`````

Since version 1.1.7, elabftw uses `grunt <http://gruntjs.com/>`_ to minify and concatenate files (JS and CSS), among other things.

* Install grunt with :

.. code-block:: bash

    $ npm install grunt grunt-contrib-uglify grunt-contrib-watch grunt-contrib-cssmin grunt-shell
    $ sudo npm install -g grunt-cli

Regenerate assets (JS/CSS)
``````````````````````````

.. code-block:: bash

    $ grunt # will minify and concatenate JS and CSS
    $ grunt css # will minify CSS

Tests
`````
Get the version 1.9.8 of `PhantomJS <https://bitbucket.org/ariya/phantomjs/downloads>`_. There is a bug in the most recent version, so grab 1.9.8.

.. code-block:: bash

    # start phantomjs
    $ ./phantomjs --ignore-ssl-errors=true --webdriver=4444
    $ grunt unit # will run the unit tests
    $ grunt test # will run the unit and acceptance tests

For code coverage you need to enable the xdebug PHP extension and run `grunt coverage`.

API Documentation
`````````````````

To generate a PHP Docblock documentation:

.. code-block:: bash

    $ grunt api

Then, point your browser to the `_api/index.html`.

You can have a look at the errors report to check that you commented all new functions properly.

Make a gif
``````````

* make a capture with xvidcap, it outputs .xwd

* convert .xwd to gif:

.. code-block:: bash

    $ convert -define registry:temporary-path=/path/tmp -limit memory 2G \*.xwd out.gif
    # or another way to do it, this will force to write all to disk
    $ export MAGICK_TMPDIR=/path/to/disk/with/space
    $ convert -limit memory 0 -limit map 0 \*.xwd out.gif

* generate a palette with ffmpeg:

.. code-block:: bash

    $ ffmpeg -i out.gif -vf fps=10,scale=600:-1:flags=lanczos,palettegen palette.png

* make a lighter gif:

.. code-block:: bash

    $ ffmpeg -i out.gif -i palette.png -filter_complex "fps=10,scale=320:-1:flags=lanczos[x];[x][1:v]paletteuse" out-final.gif

* upload to original one to gfycat and the smaller one to imgur
