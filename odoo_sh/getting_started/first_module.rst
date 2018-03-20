:banner: banners/odoo-sh.jpg

==================================
Your first module
==================================

Overview
========

This chapter helps you to create your first Odoo module and deploy it on Odoo.sh.
Basic use of git and Github is explained.

In this tutorial, *~/src* is assumed the directory where are located the Git repositories related to your Odoo projects.
Feel free to replace it by the directory of your choice.

Create the development branch
=============================

Clone your Github repository on your computer:

.. code-block:: bash

  $ mkdir ~/src; cd ~/src
  $ git clone https://github.com/<user>/<repository>.git

Replace:

* <user> with your Github user or organization name,
* <repository> with repository name.

Create a new branch:

.. code-block:: bash

  $ git checkout -b feature-1 master

Replace:

* *feature-1* by the name of your choice,
* *master* by the name of your production branch. If you just created your project, and do not have a production branch,
  you can leave *master*.


Create the module structure
===========================

Scaffolding the module
----------------------

While not necessary, scaffolding avoids the tedium of setting the basic Odoo module structure.
It nevertheless requires *odoo-bin*, and therefore the
`installation of Odoo <https://www.odoo.com/documentation/11.0/setup/install.html#source-install>`_ on your computer.

Use *odoo-bin scaffold* to generate the module structure in your repository:

.. code-block:: bash

  $ ./odoo-bin scaffold my_module ~/src/<repository>/

Replace:

* <repository> with your repository name.

This will generate the below structure:

::

  my_module
  ├── __init__.py
  ├── __manifest__.py
  ├── controllers
  │   ├── __init__.py
  │   └── controllers.py
  ├── demo
  │   └── demo.xml
  ├── models
  │   ├── __init__.py
  │   └── models.py
  ├── security
  │   └── ir.model.access.csv
  └── views
      ├── templates.xml
      └── views.xml

.. Warning::

  Do not use special characters other than the underscore ( _ ) for your module name, not even an hyphen ( - ).
  This name is used for the Python classes of your module,
  and having classes name with special characters other than the underscore is not valid in Python.

Uncomment the content of the files:

* *models/models.py*,
  an example of model with its fields,
* *views/views.xml*,
  a tree and a form view, with the menus opening them,
* *demo/demo.xml*,
  demo records for the above example model,
* *controllers/controllers.py*,
  an example of controller implementing some routes,
* *views/templates.xml*,
  two example qweb views used by the above controller routes,
* *__manifest__.py*,
  the manifest of your module, including for instance its title, description and data files to load.
  You just need to uncomment the access control list data file:

  .. code-block:: xml

    # 'security/ir.model.access.csv',

Manually
--------

If you want to create your module structure manually,
you can follow `Build an Odoo module <https://www.odoo.com/documentation/11.0/howtos/backend.html>`_ to understand
the structure of a module and the content of each file.

Push the development branch
===========================

Stage the changes to be commited

.. code-block:: bash

  $ git add my_module

Replace:

* *my_module* with the name you gave to your module

Commit your changes

.. code-block:: bash

  $ git commit -m "My first module"

Replace:

* *My first module* with the comment of your choice to describe your changes

Push your changes to your remote repository

.. code-block:: bash

  $ git push -u origin feature-1

Replace:

* *feature-1* by the name of your branch

You need to specify *-u origin feature-1* at the first push only.
From that point, when you want to push your next changes, you can simply use

.. code-block:: bash

  $ git push

Test your module
================

Your branch should appear in your development branches in your project.

.. image:: ./media/firstmodule-test-branch.png
  :align: center

You can click on the branch in the right panel to access its history.

.. image:: ./media/firstmodule-test-branch-history.png
  :align: center

You can see here the changes you just pushed, including the comment you set.
Once the database ready, you can access it by clicking the *Connect* button.

.. image:: ./media/firstmodule-test-database.png
  :align: center

If your Odoo.sh project is configured to install your module automatically,
you will directly see it amongst the database apps. Otherwise, it will be available in the apps to install.

You can then play around with your module, create new records and test your features and buttons.


Test with the production data
=============================

You need to have a production database for this step. You can create it if you do not have it yet.

Once you tested your module in a development build with the demo data and believe it is ready,
you can test it with the production data using a staging branch.

You can either:

* Make your development branch a staging branch, by drag and dropping it onto the *staging* section title.

  .. image:: ./media/firstmodule-test-devtostaging.png
    :align: center

* Merge it in an existing staging branch, by drag and dropping it onto the given staging branch.

  .. image:: ./media/firstmodule-test-devinstaging.png
    :align: center

This will create a new staging build, which will duplicate the production database and make it run with a server
updated with your latest changes of your branch.

.. image:: ./media/firstmodule-test-mergedinstaging.png
  :align: center

Once the database ready, you can access it using the *Connect* button.

Install your module
-------------------

Your module will not be installed automatically, you have to install it from the apps menu.
Indeed, the purpose of the staging build is to test the behavior of your changes as it would be on your production,
and on your production you would not like your module to be installed automatically, but on demand.

Your module may not appear directly in your apps to install either, you need to update your apps list first:

* activate the developer mode from the Settings,

  .. image:: ./media/firstmodule-test-developermode.png
    :align: center

* in the apps menu, click the *Update Apps List* button,
* in the dialog that appears, click the *Update* button.

  .. image:: ./media/firstmodule-test-updateappslist.png
    :align: center

Your module will then appears in the list of available apps.

.. image:: ./media/firstmodule-test-mymoduleinapps.png
  :align: center

Deploy in production
====================

Once you tested your module in a staging branch with your production data,
and believe it is ready for production, you can merge your branch in the production branch.

Drag and drop your staging branch on the production branch.

.. image:: ./media/firstmodule-test-mergeinproduction.png
  :align: center

This will merge the latest changes of your staging branch in the production branch,
and update your production server with these latest changes.

.. image:: ./media/firstmodule-test-mergedinproduction.png
  :align: center

Once the database ready, you can access it using the *Connect* button.

Install your module
-------------------

Your module will not be installed automatically,
you have to install it manually as explained in the
`above section about installing your module in staging databases <#install-your-module>`_.

Add a change
============

This section explains how to deploy a change in an existing module by adding a new field in a model of your module.
