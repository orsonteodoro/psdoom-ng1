AUX_DIST_GEN =                          \
        $(ac_aux_dir)/install-sh        \
        $(ac_aux_dir)/missing

CMAKE_FILES=                                   \
        CMakeLists.txt                         \
        cmake/FindSDL2.cmake                   \
        cmake/FindSDL2_mixer.cmake             \
        cmake/FindSDL2_net.cmake               \
        cmake/FindFluidSynth.cmake             \
        cmake/Findm.cmake                      \
        cmake/FindSampleRate.cmake             \
        cmake/config.h.cin

DOC_FILES=                              \
        COPYING.md                      \
        README.md                       \
        README.Music.md                 \
        NEWS.md                         \
        PHILOSOPHY.md                   \
        ChangeLog

EXTRA_DIST=                             \
        $(AUX_DIST_GEN)                 \
        $(CMAKE_FILES)                  \
        $(DOC_FILES)                    \
        NOT-BUGS.md                     \
        README.Strife.md                \
        .lvimrc                         \
        HACKING.md                      \
        TODO.md                         \
        rpm.spec                        \
        win32/win_opendir.c             \
        win32/win_opendir.h

PSDO0M_DOC_FILES=                       \
        CHANGELOG.psdoom-ng             \
        COPYING.psdoom-ng               \
        CREDITS.psdoom-ng               \
        README.psdoom-ng                \
        TODO.psdoom-ng

psdoomngdocsdir = ${docdir}/../psdoom-ng
psdoomngdocs_DATA = $(DOC_FILES) $(PSDO0M_DOC_FILES)

if ENABLE_ALL_GAMES
doomdocsdir = ${docdir}/../${PROGRAM_PREFIX}doom
doomdocs_DATA = $(DOC_FILES) NOT-BUGS.md

hereticdocsdir = ${docdir}/../${PROGRAM_PREFIX}heretic
hereticdocs_DATA = $(DOC_FILES)

hexendocsdir = ${docdir}/../${PROGRAM_PREFIX}hexen
hexendocs_DATA = $(DOC_FILES)

strifedocsdir = ${docdir}/../${PROGRAM_PREFIX}strife
strifedocs_DATA = $(DOC_FILES) README.Strife.md
endif

MAINTAINERCLEANFILES =  $(AUX_DIST_GEN)

SUBDIRS=textscreen opl pcsound data src man

DIST_SUBDIRS=pkg $(SUBDIRS)

if HAVE_DOC

INSTALL : man/INSTALL.template man/simplecpp
	$(srcdir)/man/simplecpp -DDOOM -DHERETIC -DHEXEN -DSTRIFE \
            -DLONG_GAME_NAME="@PACKAGE_SHORTNAME@ Doom" \
            -DLONG_EXE_NAME="@PROGRAM_PREFIX@doom" \
	                < $(srcdir)/man/INSTALL.template > $@

endif

psdoomngcfgdir = ${prefix}/share/psdoom-ng
psdoomngcfg_DATA = psdoom-ng.conf.example

psdoomngwrapperdir = ${prefix}/bin
psdoomngwrapper_PROGRAMS = psd

psd : psd.in
	cp psd.in $@
	chmod +x $@

if ENABLE_CLOUDFOUNDRY
psdoomngcfdir = ${prefix}/local/bin
psdoomngcf_PROGRAMS = psdoom-ng-cf-ctl

psdoom-ng-cf-ctl : contrib/psdoom-ng-cf-ctl.in
	cp contrib/psdoom-ng-cf-ctl.in $@
	chmod +x $@
endif
