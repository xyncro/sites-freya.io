Versioning
==========

Freya follows a `semantic versioning <http://semver.org>`_ approach to versioning, with some key points, due to the multi-library nature of the Freya stack.

The overall version of Freya (and the version used to define versions of documentation, etc.) is the version of the Freya meta-package -- the package which has dependencies that make up a default Freya stack. The version of this meta-package may be increased driven by changes to the dependency set.

In effect, this means that a breaking change to a dependency will likely require a major version increase of that dependency. The next release of the Freya meta-package which includes the breaking change version will therefore also likely receive a major version increment.

In this way the version of the Freya meta-package signifies the version of a Freya stack which is known and designed to work well, and it may be many versions ahead of some of the versions of the packages that it depends upon -- especially when some of the core packages are very stable.

As from Freya 3.0 (including release candidate builds) the versioning of Freya packages will be less significant from a "feature announcement" perspective, but will become a simple semantic versioning tool.

Upgrading
---------

Upgrading to newer versions of Freya may still result in breaking changes (major version changes will signify this possibility). Guidance for working through these changes can be found in the following section:

* :doc:`upgrading` -- overview and version specific guidance on managing change in Freya versions.

.. toctree::
   :glob:
   :hidden:
   :maxdepth: 1

   *
