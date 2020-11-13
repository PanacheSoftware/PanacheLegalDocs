Build the project
=================

The main GitHub repository for the Panache Legal Platform can be found `here <https://github.com/PanacheSoftware/PanacheLegalPlatform/>`_.

Panache Legal is built using .NET 5 and the C# language.  It includes a main solution file that you can open in the `Visual Studio <https://visualstudio.microsoft.com/>`_, but editing and buildiing the code could also be done using the cross platform Visual Studio Code editor, or another code editor of your choice.

Prerequisites
^^^^^^^^^^^^^

The only prerequisites are:

1. `Install <https://www.microsoft.com/net/download/core#/current>`_ the latest .NET 5 SDK for your platform.
2. Install `Git <https://git-scm.com/>`_

Within the `UI Core <https://github.com/PanacheSoftware/PanacheLegalPlatform/tree/main/src/Web/PanacheSoftware.UI.Core>`_ project `Node <https://nodejs.org/en/>`_ and `Gulp <https://gulpjs.com/>`_ are used to import various open source javascript libraries (via `NPM <https://www.npmjs.com/>`_), as well as compile some css files.  Although not strictly neccesary, as all compiled files and required libraries are included in GitHub, if you want to make changes in this area you should install Node and Gulp and use the provided `package.json <https://github.com/PanacheSoftware/PanacheLegalPlatform/blob/main/src/Web/PanacheSoftware.UI.Core/package.json>`_ file to update node modules.

Build
^^^^^

This should be as simple as opening the `solution <https://github.com/PanacheSoftware/PanacheLegalPlatform/blob/main/src/PanacheLegalPlatform.sln>`_ in Visual Studio and running or building the project, or alternatively navigating to the 'src' directory and running::

    dotnet build .\PanacheLegalPlatform.sln


