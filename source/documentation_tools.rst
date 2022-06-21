.. _documentation_tools:

Documentation Tools
===================

Monocle documents are powered by `sphinx <https://www.sphinx-doc.org/en/master/index.html>`_ and `readthedocs.org <https://readthedocs.org/>`_ using `reStructuredText <https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html>`_. This section gives a detailed explaination on how to install tools to generate documentation for Monocle. And also describes how to contribute to the documents.

First time Install On a Windows system using WSL Ubuntu
-------------------------------------------------------

* `Enable WSL on windows. <https://www.windowscentral.com/install-windows-subsystem-linux-windows-10>`_
* `Install Ubuntu 18.04. <https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6?ranMID=24542&ranEAID=kXQk6*ivFEQ&ranSiteID=kXQk6.ivFEQ-lZ2r42Gr4Nn5NydU6mZgGQ&epi=kXQk6.ivFEQ-lZ2r42Gr4Nn5NydU6mZgGQ&irgwc=1&OCID=AID2200057_aff_7593_1243925&tduid=%28ir__co2im6bswokf62kg0wqgq9lhcm2xoqqbu6pa3poq00%29%287593%29%281243925%29%28kXQk6.ivFEQ-lZ2r42Gr4Nn5NydU6mZgGQ%29%28%29&irclickid=_co2im6bswokf62kg0wqgq9lhcm2xoqqbu6pa3poq00&activetab=pivot:overviewtab>`_
* Give appropriate password and configure Ubuntu. By running it.
* APT Update and upgrade on Ubuntu: ``$ sudo apt update; sudo apt upgrade``
* Install python on Ubuntu: ``$ sudo apt install python python-pip``
* Change directory to ``monocle`` folder: ``$ cd /mnt/c/Nordic/nRF5_SDK_17.0.2_d674dde/examples/monocle``
* Create a Python Virtual Environment for Sphinx: ``$ python -m virtualenv monocle_env``
* Activate Python Virtual Environment: ``$ source monocle_env/bin/activate``
* Install Sphinx: ``$ pip install sphinx``
* Install ReadTheDocs theme: ``$ pip install sphinx_rtd_theme``
* Change Directory to docs: ``$ cd docs``
* Install PDF Tools: ``$ sudo apt-get install latexmk texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended``

Generate documents
------------------

* Build the document to get HTML: ``$ make html``
* Build the document to get PDF:  ``$ make latexpdf`` 
* The documents will be built on ``build`` directory under ``docs`` folder.

Updating the document and rebuilding
------------------------------------

* All the document source code is present in ``source`` folder.
* ``index.rst`` file is the home file. The source files are written in `ReStructuredText <https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html>`_ format.
* After modifying the source code (Document source code), execute ``$ make html`` to generate the html documents.