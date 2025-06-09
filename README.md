psdoom-ng
=========

psdoom-ng is a First Person Shooter operating system process killer based on psDooM and Chocolate Doom.


Compile and usage
-----------------

Quick guide: 

 1. Install all dependencies: gcc, make, libsdl, sdl_mixer, sdl_net, bash.
 2. `cd trunk`
 3. `./configure --help` #review your prefix, other paths, and extra options
 4. `make`
 5. `make install`
 6. `man psdoom-ng` #read up on how to set it up the wrapper script psd
 4. Get a copy of Doom, to copy the file Doom2.wad.
 3. Action! `psd` or `psdoom-ng`

Find more information in:
 * [man psdoom-ng](https://github.com/orsonteodoro/psdoom-ng/blob/master/extras/psdoom-ng.pdf)
 * [trunk/INSTALL](https://github.com/orsonteodoro/psdoom-ng/blob/1.6.0/trunk/INSTALL)
 * [trunk/CMDLINE](https://github.com/orsonteodoro/psdoom-ng/blob/1.6.0/trunk/CMDLINE)
 * [trunk/README.psdoom-ng](https://github.com/orsonteodoro/psdoom-ng/blob/master/trunk/README.psdoom-ng) 

Requirements
------------

Linux requirements:

| Package       | version   | Required or optional? | Build time or runtime? | Purpose                                                                                                                                              |
| ----          | ----      | ----                  | ----                   | ----                                                                                                                                                 |
| autotools     |           | Required              | Build time             | For build system (e.g. autoconf, make)                                                                                                               |
| Python        | 3.x       | Required              | Build time             | For documentation generation                                                                                                                         |
| Bash          |           | Required              | Build time and runtime | For the psd wrapper script                                                                                                                           |
| GCC           |           | Required              | Build time             | For building the program                                                                                                                             |
| Clang         |           | Optional              | Build time             | An alternative to GCC                                                                                                                                |
| procps        |           | Required              | Runtime                | To list processes with ps                                                                                                                            |
| SDL2          | >= 2.0.14 | Required              | Runtime                | To display on tty with KMS, X11, or Wayland                                                                                                          |
| util-linux    |           | Required              | Runtime                | To kill processes                                                                                                                                    |
| FluidSynth    | >= 2.2.0  | Optional              | Runtime                | To play music based on MUS or MIDI format                                                                                                            |
| FLAC          | >= 1.4.3  | Optional              | Runtime                | To play FLAC based music packs                                                                                                                       |
| libpng        | >= 1.2.50 | Optional              | Runtime                | For taking screenshots                                                                                                                               |
| libsamplerate | >= 0.1.8  | Optional              | Runtime                | To play sound effects consistently on modern soundcards                                                                                              |
| SDL_mixer     | >= 2.0.2  | Optional              | Runtime                | To play music and sound simultaneously                                                                                                               |
| zenity        | >= 4      | Optional              | Runtime                | For config settings security review if using saved configs in `/home/${USER}/.psdoom-ng/psdoom-ng.conf` or `/etc/psdoom-ng.conf` and Wayland or X11  |

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
The custom wads referred in psDooM readme can be found at contrib/psdoom-2000.05.03-data.tar.gz

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
