Introduction
============

This is attempt to create Live2D module for RenPy. This is WIP and not production version. Project has no ETA. This source code is invitation to developers that are willing to help with development of module.

Windows version is not ready yet.

This module works only with OpenGL renderer.

Usage example
=============

.. code:: renpy

    define h = Character("Hiyori")

    init python:
        from live2d.displayable import Live2DDisplayable
        from renpy.loader import transfn
    
        sprite_live2d = Live2DDisplayable()
    
    init:
        image sprite_live2d = sprite_live2d
    
    label start:
    
        show sprite_live2d
            
        $ live2d_model_hiyori = sprite_live2d.scene.create_model(transfn(u'live2d_resources/Hiyori/'), u'Hiyori.model3.json')
    
        $ live2d_model_hiyori.start_random_motion(group = u"Idle", priority = 3)
    
        h "You've created a new Ren'Py game."

        $ live2d_model_hiyori.start_motion(group = u"TapBody", no = 0, priority = 3)

        h "Once you add a story, pictures, and music, you can release it to the world!"

        return

Building
========

macOS
-----

Extensions for RenPy should be built with special Python version that is configured for RenPy:

1. Download `renpy-deps <https://github.com/renpy/renpy-deps>`_ repository.

2. Open 'build_python.sh' script and apply patch:

   .. code:: diff
   
       diff --git a/build_python.sh b/build_python.sh
       index ca76c4d..274d03e 100755
       --- a/build_python.sh
       +++ b/build_python.sh
       @@ -7,7 +7,7 @@ INSTALL=$PWD/install

        # The xes are required to prevent msys from interpreting these as
        # paths. (We use the system python to do this normalization.)
       -SOURCE=`python $SOURCE/norm_source.py "x$PWD" "x$SOURCE"`
       +SOURCE=`python2 $SOURCE/norm_source.py "x$PWD" "x$SOURCE"`

        export LD_LIBRARY_PATH="$INSTALL/lib"
        export DYLIB_LIBRARY_PATH="$INSTALL/lib"

3. Run script to create custom Python build.

4. Download Cython source code and install it via launching 'setup.py' script with fresh Python build:

   .. code:: shell
        
        %PATH_TO_CUSTOM_PYTHON_BUILD%/python setup.py install
        
Now you have special Python build that is suitable for building Live2D module for RenPy:

1. Download `Cubism Native SDK <https://live2d.github.io/index.html#native>`_ and replace 'CubismSDK/Core' folder of this library with 'Core' folder from downloaded SDK.

2. Open console and go to the folder where you placed content of this repository

3. Launch 

   .. code:: shell
   
      %PATH_TO_PYTHON_BUILD%/python setup.py build_ext --inplace
      
4. Module is ready. Now you could launch RenPy game.

Linux
-----

Should be similar to macOS but i could not confirm.

Windows
-------

1. Download and install `Microsoft Visual C++ Compiler for Python 2.7 <https://www.microsoft.com/en-us/download/details.aspx?id=44266>`_

2. Download and install x86 version of `Python 2.7.10  <https://www.python.org/ftp/python/2.7.10/python-2.7.10.msi>`_

3. Download `Cubism Native SDK <https://live2d.github.io/index.html#native>`_ and replace 'CubismSDK/Core' folder of this library with 'Core' folder from downloaded SDK.

4. Launch 'Visual C++ 2008 32-bit Command Prompt' from 'Start' menu and go to 'CubismSDK/Core/dll/windows/x86' folder.

5. Launch

    .. code:: shell
    
        dumpbin /EXPORTS Live2DCubismCore.dll > Live2DCubismCore.exports

6. Edit 'Live2DCubismCore.exports' to create 'Live2DCubismCore.def' file. Also you could get ready 'def' file `here <https://gist.github.com/asfdfdfd/e20835ed92bd245e258d8a1c1b2f77ac>`_ but it may be a bit outdated so i recommend to create it by yourself.

7. Launch
    
    .. code:: shell
    
        lib /def:Live2DCubismCore.def /out:Live2DCubismCore.lib

8. Go to root module folder and launch

    .. code:: shell
    
        %PATH_TO_PYTHON%\python.exe setup.py build_ext --inplace --compiler=msvc
        
9. Module is ready. Now you could launch RenPy game.
        
Installing
==========

Create 'live2d' folder inside 'game' folder of your project. And copy content of repository to this folder. Then follow 'Building' section. Prebuilt modules will be provided in the future.