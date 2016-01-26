# configure-cmake

configure-cmake is an autotools-style configure script for CMake-based
projects.

People building software on Linux or BSD generally expect to be able
to `./configure && make && make install`, but projects using CMake
don't provide a `configure` script.  To make matters worse, the syntax
for invoking CMake is awkward and has issues with discoverability.

configure-cmake provides an easy way for people accustomed to
autotools-based projects to build your CMake-based project.  Just drop
the `configure` script into your project; it will accept most of the
standard autotools arguments (such as `--prefix` and `--libdir`), as
well as custom arguments you can specify, and map them to the correct
CMake definitions.

It it as permissively licensed as I can make it (CC0 is basically an
attempt to put something in the public domain), so please feel free to
copy it into your project—users accustomed to autotools will thank
you!  You only need the `configure` script—you shouldn't have to worry
about the LICENSE or README files.

## Customizing

You can add --enable-*, --disable-*, and --with-* parameters without
modifying the `configure` script itself.  Instead, you only need to
create a `.configure-custom.sh` file next to your `configure` script.
`configure` will then source `.configure-custom.sh` and use the
variables it finds to integrate the parameters.

The main variables you can set in `.configure-custom.sh` are:

* **ENABLE_VARS**
* **DISABLE_VARS**
* **WITH_VARS**

Each of these a space separated list of values.  Each value is a '|'
separated list of up to 3 values, though the first two are optional.
The syntax is:

```
name[|value[|transformed-name]]
```

The `name` will be be the name of the argument passed to
`configure`—for enable it will be `--enable-name`, for disable it will
be `--disable-name`, and for with it will be `--with-name`.

The optional `value` is the value passed to CMake, which defaults to
"yes".

The optional `transformed-name` is the key passed to CMake, which
defaults to `name` uppercased and all non-alpha-numeric characters
replaced with underscores.  For example:

```
ENABLE_VARS="foo|bar|BAZ"
```

Will create a `--enable-foo` parameter for people to pass to
`configure`.  If passed, `configure` will then pass `-DBAZ=bar` to
CMake.

Additionally, if you would like to supply a custom documentation
string for parameters (which is displayed in the output of
`./configure --help`), you can create an additional variable called
`ENABLE/DISABLE/WITH_transformed-name_DOC`.  Continuing or previous
example:

```
ENABLE_BAZ_DOC="enable integration with foo"
```

Will result in the output of `./configure --help` displaying

```
  --enable-foo            enable integration with foo
```

If you would like to see an example of how this works, you can use the
[`.configure-custom.sh` from Squash](https://github.com/quixdb/squash/blob/master/.configure-custom.sh).

Note that you can also use `--pass-thru -DBAZ=bar` to achieve the same
result.
