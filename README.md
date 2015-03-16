# Autotools-style configure script wrapper around CMake

People building software on Linux or BSD generally expect to be able
to `./configure && make && make install`, but projects using CMake
don't provide a `configure` script.

This script is just a simple wrapper around `cmake` which accepts
autotools-style arguments (such as --prefix=...) and translates them
into a cmake invocation.

It it as permissively licensed as I can make it (CC0 is basically an
attempt to put something in the public domain), so please feel free to
copy it into your project—users accustomed to autotools will thank
you!  You only need the `configure` script—you shouldn't have to worry
about the LICENSE or README files.
