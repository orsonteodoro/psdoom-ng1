psdoom-ng
=========

psdoom-ng is a First Person Shooter operating system process killer based on psDooM and Chocolate Doom.

Requirements
------------

Linux requirements:

| Package       | version   | Required or optional? | Build time or run time? | Purpose                                                                                                                                              |
| ----          | ----      | ----                  | ----                    | ----                                                                                                                                                 |
| autoconf      |           | Required              | Build time              | For autotools build system                                                                                                                           |
| automake      | >= 1.8.0  | Required              | Build time              | For autotools build system                                                                                                                           |
| coreutils     |           | Required              | Build time              | For setting file permissions                                                                                                                         |
| CMake         |           | Optional              | Build time              | An alternative to autotools build system                                                                                                             |
| grep          |           | Required              | Build time              | For build scripts                                                                                                                                    |
| Bash          |           | Required              | Build time and run time | For the psd wrapper script                                                                                                                           |
| binutils      |           | Required              | Build time              | For linking programs                                                                                                                                 |
| Clang         |           | Optional              | Build time              | An alternative to GCC                                                                                                                                |
| GCC           |           | Required              | Build time              | For building the program                                                                                                                             |
| FLAC          | >= 1.4.3  | Optional              | Run time                | To play FLAC based music packs                                                                                                                       |
| FluidSynth    | >= 2.2.0  | Optional              | Run time                | To play music based on MUS or MIDI format                                                                                                            |
| make          |           | Required              | Build time              | For autotools build system                                                                                                                           |
| man-db        |           | Optional              | Run time                | To view documentation with `man psdoom-ng`                                                                                                           |
| libpng        | >= 1.2.50 | Optional              | Run time                | To save screenshots in PNG format                                                                                                                    |
| libsamplerate | >= 0.1.8  | Optional              | Run time                | To play sound effects consistently on modern soundcards                                                                                              |
| libvorbis     | >= 1.3.7  | Optional              | Run time                | To play Ogg Vorbis based music packs                                                                                                                 |
| pkg-config    |           | Required              | Build time              | For package autodetection and getting package flags info                                                                                             |
| procps        |           | Required              | Run time                | To list processes with ps                                                                                                                            |
| Python        | 3.x       | Required              | Build time              | For documentation generation                                                                                                                         |
| SDL 2         | >= 2.0.14 | Required              | Run time                | To display on KMS on TTY, X11, or Wayland                                                                                                            |
| SDL_mixer     | >= 2.0.2  | Optional              | Run time                | To play background music and sound effects simultaneously                                                                                            |
| util-linux    |           | Required              | Run time                | To kill processes                                                                                                                                    |
| zenity        | >= 3.44   | Optional              | Run time                | For config settings security review if using saved configs in `/home/${USER}/.psdoom-ng/psdoom-ng.conf` or `/etc/psdoom-ng.conf` and Wayland or X11  |

Compile and usage
-----------------

Quick guide with autotools:

 1. Install all dependencies
 2. `cd trunk`
 3. `./configure --help` #review your prefix, other paths, and extra options
 4. `make`
 5. `make install`
 6. `man psdoom-ng` #read up on how to set it up the wrapper script psd
 7. Get a copy of Doom, to copy the file Doom2.wad.
 8. Action! Run with `psd` or `psdoom-ng`.  psd is preferred to auto load settings.

Quick guide with cmake:

 1. Install all dependencies
 2. `cd trunk`
 3. `cmake -B build -S . -DCMAKE_INSTALL_PREFIX=/usr -DENABLE_SDL2_MIXER=ON <source-dir>` # For features, see option() in CMakeLists.txt
 4. `cd build`
 4. `ninja -v`
 5. `ninja -v install`
 6. `man psdoom-ng` #read up on how to set it up the wrapper script psd
 7. Get a copy of Doom, to copy the file Doom2.wad.
 8. Action! Run with `psd` or `psdoom-ng`.  psd is preferred to auto load settings.

Find more information in:
 * [man psdoom-ng](https://github.com/orsonteodoro/psdoom-ng1/blob/psdoom-ng/psdoom-ng.pdf)
 * [trunk/INSTALL](https://github.com/orsonteodoro/psdoom-ng/blob/1.6.0/trunk/INSTALL)
 * [trunk/CMDLINE](https://github.com/orsonteodoro/psdoom-ng/blob/1.6.0/trunk/CMDLINE)
 * [trunk/README.psdoom-ng](https://github.com/orsonteodoro/psdoom-ng1/blob/psdoom-ng/README.psdoom-ng) 

Gentoo Linux
------------

You can find the ebuild at https://github.com/orsonteodoro/oiledmachine-overlay

Mac OS X
---------

Now with support for Mac OS X!

It is recommended use brew to install the depenedencies.

Support for external process source
-----------------------------------

You can use external commands as interface to retrieve, renice and kill process.

This makes it easy to adapt the tool to your needs, or even integrate it with external
services (AWS, heroku, vmware, etc).

For that, you only need to override these environment variables:

 * PSDOOM_NG_PS_CMD List the processes. The command must print one space separated 
   line per process with the format: `<user> <pid> <processname> <is_daemon=[1|0]>`
```bash
    keymon 29 web4 1
    keymon 30 web3 1
    keymon 31 adis3 1
    keymon 32 core15 1
    keymon 20 core2 1
```

 * PSDOOM_NG_RENICE_CMD Command to renice the process. Will get the pid as argument

 * PSDOOM_NG_KILL_CMD Command to kill the process. Will get the pid as argument

For example, in contrib you can find a script that interacts with cloudfoundry:

```bash
    cd trunk
    PSDOOM_NG_PS_CMD="./contrib/psdoom-cf-ctl ps" \
    PSDOOM_NG_RENICE_CMD="true" \
    PSDOOM_NG_KILL_CMD="./contrib/psdoom-cf-ctl kill" \
    ./src/psdoom
```

NOTE: psdoom does a synchronous call to the external commands (mono-thread). If your
command takes too long, you will feel hipcuts in the game. Try to make your commands
respond really fast! 

The example script can be installed by adding --enable-cloudfoundry on configure and is installed in /usr/local/portage/psdoom-ng-cf-ctl

Custom map
----------

The custom wads referred in psDooM readme can be found at [contrib/psdoom-2000.05.03-data.tar.gz](https://github.com/orsonteodoro/psdoom-ng1/raw/refs/heads/psdoom-ng/contrib/psdoom-2000.05.03-data.tar.gz)

Contributors
------------

 Dennis Chao came up with the original idea and wrote much of the mod.

 David Koppenhofer was the previous maintainer of the mod psDooM.

 Simon Howard wrote Chocolate Doom which is the current game engine used.

 Hector Rivas Gandara added support for external sources and cloud services.

 Jesse Speilman added support for Mac OS X.

 Orson Teodoro was responsible for making psDooM mod work on Chocolate Doom.

Contributing
------------

    Fork it ( https://github.com/orsonteodoro/psdoom-ng1/fork )
    Create your feature branch (git checkout -b my-new-feature)
    Commit your changes (git commit -am 'Add some feature') or (for multiline commit message: git commit -F- <<EOF \n blah blah blah \n EOF)
    Push to the branch (git push origin my-new-feature)
    Create new Pull Request

License
-------

 psDooM was based on GNU General Public License 2.0.

 Chocolate Doom was based on GNU General Public License 2.0.

 You can view more about the GPL-2 at http://www.gnu.org/licenses/old-licenses/gpl-2.0.en.html
