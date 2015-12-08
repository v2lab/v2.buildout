Buildout for v2 website
=======================

This website is located here: http://v2.nl

Installation
------------

The server uses Debian 8, Jessie.
Install the following debian packages::

    sudo apt-get install gcc git python-dev libtiff5-dev libjpeg62-turbo-dev zlib1g-dev libfreetype6-dev

Set symlinks for PIL::

    sudo ln -s /usr/lib/x86_64-linux-gnu/libjpeg.so /usr/lib
    sudo ln -s /usr/lib/x86_64-linux-gnu/libfreetype.so /usr/lib
    sudo ln -s /usr/lib/x86_64-linux-gnu/libz.so /usr/lib

This website uses buildout, run in the following way::

    cp buildout.cfg.dev buildout.cfg
    python bootstrap.py
    bin/buildout

Patch
-----

Due to a Plone upgrade old ABTreeFolders are not sortable anymore.
Events is an ABTreeFolder. This patch, combined with disabling javascript
in the browser allows the Events to be sorted.
See also this bug report: https://dev.plone.org/ticket/12255::

  *** Plone-4.0.1-py2.7.egg/Products/CMFPlone/PloneFolder.py	2014-08-06 19:08:20.560550391 +0200
  --- PloneFolder.py	2014-08-06 12:18:55.302665806 +0200
  *************** class OrderedContainer(Folder):
  *** 127,132 ****
  --- 127,133 ----
            """Get the ids of only cmf objects (used for moveObjectsByDelta)."""
            ttool = getToolByName(self, 'portal_types')
            cmf_meta_types = [ti.Metatype() for ti in ttool.listTypeInfo()]
  +         cmf_meta_types.append('ATBTreeFolder')
            return [obj['id'] for obj in objs if obj['meta_type'] in cmf_meta_types]

        security.declareProtected(ModifyPortalContent, 'getObjectPosition')
