The CssSelector Component
=========================

    The CssSelector component converts CSS selectors to `XPath`_ expressions.

Installation
------------

.. code-block:: terminal

    $ composer require symfony/css-selector

.. include:: /components/require_autoload.rst.inc

Usage
-----

.. seealso::

    This article explains how to use the CssSelector features as an independent
    component in any PHP application. Read the :ref:`Symfony Functional Tests <functional-tests>`
    article to learn about how to use it when creating Symfony tests.

Why Use CSS selectors?
~~~~~~~~~~~~~~~~~~~~~~

When you're parsing an HTML or an XML document, by far the most powerful
method is `XPath`_.

XPath expressions are incredibly flexible, so there is almost always an
XPath expression that will find the element you need. Unfortunately, they
can also become very complicated, and the learning curve is steep. Even common
operations (such as finding an element with a particular class) can require
long and unwieldy expressions.

Many developers -- particularly web developers -- are more comfortable
using CSS selectors to find elements. As well as working in stylesheets,
CSS selectors are used in JavaScript with the ``querySelectorAll()`` function
and in popular JavaScript libraries such as jQuery.

CSS selectors are less powerful than XPath, but far easier to write, read
and understand. Since they are less powerful, almost all CSS selectors can
be converted to an XPath equivalent. This XPath expression can then be used
with other functions and classes that use XPath to find elements in a
document.

The CssSelector Component
~~~~~~~~~~~~~~~~~~~~~~~~~

The component's only goal is to convert CSS selectors to their XPath
equivalents, using :method:`Symfony\\Component\\CssSelector\\CssSelectorConverter::toXPath`::

    use Symfony\Component\CssSelector\CssSelectorConverter;

    $converter = new CssSelectorConverter();
    var_dump($converter->toXPath('div.item > h4 > a'));

This gives the following output:

.. code-block:: text

    descendant-or-self::div[@class and contains(concat(' ',normalize-space(@class), ' '), ' item ')]/h4/a

You can use this expression with, for instance, :phpclass:`DOMXPath` or
:phpclass:`SimpleXMLElement` to find elements in a document.

.. tip::

    The :method:`Crawler::filter() <Symfony\\Component\\DomCrawler\\Crawler::filter>` method
    uses the CssSelector component to find elements based on a CSS selector
    string. See the :doc:`/components/dom_crawler` for more details.

Limitations of the CssSelector Component
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Not all CSS selectors can be converted to `XPath`_ equivalents.

There are several CSS selectors that only make sense in the context of a
web-browser.

* link-state selectors: ``:link``, ``:visited``, ``:target``
* selectors based on user action: ``:hover``, ``:focus``, ``:active``
* UI-state selectors: ``:invalid``, ``:indeterminate`` (however, ``:enabled``,
  ``:disabled``, ``:checked`` and ``:unchecked`` are available)

Pseudo-elements (``:before``, ``:after``, ``:first-line``,
``:first-letter``) are not supported because they select portions of text
rather than elements.

Pseudo-classes are partially supported:

* Not supported: ``*:first-of-type``, ``*:last-of-type``, ``*:nth-of-type`` and
  ``*:nth-last-of-type`` (all these work with an element name (e.g.
  ``li:first-of-type``) but not with the ``*`` selector).
* Supported: ``*:only-of-type``, ``*:scope``.

.. versionadded:: 6.3

    The support for ``*:scope`` was introduced in Symfony 6.3.

Learn more
----------

* :doc:`/testing`
* :doc:`/components/dom_crawler`

.. _`XPath`: https://en.wikipedia.org/wiki/XPath
