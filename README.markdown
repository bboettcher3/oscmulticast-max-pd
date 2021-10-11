# oscmulticast external for MaxMSP and Pure Data

An external object for sending and receiving Open Sound Control over multicast. Help patches are included for documentation.

Windows support for the Pure Data external is not yet complete.

On OSX you can build the externals using the XCode project. Windows users should use the instructions below.

## Building the oscmulticast on Windows

### Dependencies

First, you'll need to clone the [max sdk][max-sdk].

[max-sdk]: https://github.com/Cycling74/max-sdk

Cmake is also required to generate visual studio solutions, and can be installed [here][cmake]. Add it to the environment path when prompted for terminal access later on.

[cmake]: https://cmake.org/download/

oscmulticast depends on version 0.30 of liblo or later.
Please clone the [LibLo repository][liblo] and consult its documentation to build for Windows.

[liblo]: https://github.com/radarsat1/liblo

Finally, you'll need Visual Studio 2017 or 2019, which you can grab [here][visual_studio]. Be sure to install the C++ developer tools when installing if you don't already have them.

[visual_studio]: https://visualstudio.microsoft.com/vs/

### Configuring

Now open a terminal in this project's root folder.

Create a build directory and cd into it

    mkdir ./build
    cd build

Modify the CMakeLists.txt file in the root folder, replacing the paths near the top with your local paths.

Comment out line 4 in <your-max-sdk-root>/source/max-sdk-base/max-pretarget.cmake:

    #string(REGEX REPLACE "(.*)/" "" THIS_FOLDER_NAME "${CMAKE_CURRENT_SOURCE_DIR}")

Commenting this line out corrects the external's name when exported.

Run the following to generate a solution, replacing your version's details:

    cmake -G "Visual Studio 16 2019" ..

### Building

By now, you should have a Visual Studio solution in the ./build directory. Open the .sln and build the external project(s) of your choosing.

Once built, copy the Max help files for each external to the output directory so that they're visible in Max. 

### Running

Open Max and add the folder your external is built in with the `Options -> File Preferences` interface. Now you should be able to create your externals in a patcher.
