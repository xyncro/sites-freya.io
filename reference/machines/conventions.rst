Conventions
===========

Freya machines follow a package naming convention to make it clear what is available and how it should be used. The convention for naming is as follows:

* **Freya.Machines.<Machine>[.<Extension>]**

Using the example of the Freya HTTP machine, this gives:

* **Freya.Machines.Http**

as the name of the core HTTP machine library. The HTTP machine also has extensions available to implement further web standards, or to integrate with other technologies/frameworks. Following the convention, this gives (for example):

* **Freya.Machines.Http.Cors**
* **Freya.Machines.Http.Patch**
