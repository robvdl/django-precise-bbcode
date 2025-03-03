=====================
django-precise-bbcode
=====================

.. image:: https://readthedocs.org/projects/django-precise-bbcode/badge/?version=stable
  :target: http://django-precise-bbcode.readthedocs.org/en/stable/
  :alt: Documentation Status

.. image:: https://img.shields.io/pypi/l/django-precise-bbcode.svg
  :target: https://pypi.python.org/pypi/django-precise-bbcode/
  :alt: License

.. image:: http://img.shields.io/pypi/v/django-precise-bbcode.svg
  :target: https://pypi.python.org/pypi/django-precise-bbcode/
  :alt: Latest Version

.. image:: https://github.com/ellmetha/django-precise-bbcode/workflows/CI/badge.svg?branch=develop
  :target: https://github.com/ellmetha/django-precise-bbcode/actions
  :alt: Build status

.. image:: https://img.shields.io/codecov/c/github/ellmetha/django-precise-bbcode.svg
  :target: https://codecov.io/github/ellmetha/django-precise-bbcode
  :alt: Codecov status

|
*Django-precise-bbcode* is a Django application providing a way to create textual contents based on BBCodes.

  BBCode is a special implementation of HTML. BBCode itself is similar in style to HTML, tags are enclosed in square brackets [ and ] rather than < and > and it offers greater control over what and how something is displayed.

This application includes a BBCode compiler aimed to render any BBCode content to HTML and allows the use of BBCodes tags in models, forms and admin forms. The BBCode parser comes with built-in tags (the default ones ; ``b``, ``u``, etc) and allows the use of smilies, custom BBCode placeholders and custom BBCode tags. These can be added in two different ways:

* Custom tags can be defined in the Django administration panel and stored into the database ; doing this allows any non-technical admin to add BBCode tags by defining the HTML replacement string associated with each tag
* Tags can also be manually registered to be used by the parser by defining a tag class aimed to render a given bbcode tag and its content to the corresponding HTML markup

.. contents:: Table of Contents
    :local:


Documentation
-------------

Online browsable documentation is available at https://django-precise-bbcode.readthedocs.org.


Requirements
------------

* Python 3.6+
* Django 3.2+
* PIL or Pillow (required for smiley tags)

Installation
------------

Just run:

::

  pip install django-precise-bbcode

Once installed you can configure your project to use *django-precise-bbcode* with the following steps.

Add ``precise_bbcode`` to ``INSTALLED_APPS`` in your project's settings module:

.. code-block:: python

  INSTALLED_APPS = (
      # other apps
      'precise_bbcode',
  )

Then install the models:

.. code-block:: shell

  python manage.py migrate

Usage
-----

Rendering bbcodes
*****************

*Django-precise-bbcode* comes with a BBCode parser that allows you to transform a textual content containing BBCode tags to the corresponding HTML markup. To do this, simply import the ``get_parser`` shortcut and use the ``render`` method of the BBCode parser::

  >>> from precise_bbcode.bbcode import get_parser
  >>> parser = get_parser()
  >>> parser.render('[b]Hello [u]world![/u][/b]')
  '<strong>Hello <u>world!</u></strong>'

*It's that easy!*

As you may need to render bbcodes inside one of your Django template, this parser can be used as a template filter or as a template tag after loading ``bbcode_tags``::

  {% load bbcode_tags %}
  {% bbcode entry.bbcode_content %}
  {{ "[b]Write some bbcodes![/b]"|bbcode }}

The BBCode content included in the ``entry.bbcode_content``  field will be converted to HTML and displayed. The last statement will output ``<strong>Write some bbcodes!</strong>``.

Storing bbcodes
***************

While you can use the Django built-in ``models.TextField`` to add your BBCode contents to your models, a common need is to store both the BBCode content and the corresponding HTML markup in the database. To address this *django-precise-bbcode* provides a ``BBCodeTextField``.

.. code-block:: python

  from django.db import models
  from precise_bbcode.fields import BBCodeTextField

  class Post(models.Model):
      content = BBCodeTextField()

This field will store both the BBCode content and the correspondign HTML markup. The HTML content of such a field can then be displayed in any template by using its ``rendered`` attribute:

::

  {{ post.content.rendered }}

And more...
***********

Head over to the `documentation <https://django-precise-bbcode.readthedocs.org>`_ for all the details on how to use the BBCode parser and how to define custom BBcode tags, placeholders and smilies.

Authors
-------

Morgan Aubert (`@ellmetha <https://github.com/ellmetha>`_) and contributors_

.. _contributors: https://github.com/ellmetha/django-precise-bbcode/contributors

License
-------

BSD. See ``LICENSE`` for more details.
