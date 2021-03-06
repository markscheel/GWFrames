##
## NOTE: When compiling python, this Makefile is only used to call the
## python build process.  No variables set in this file affect python.
## If the python build is having trouble, look in setup.py to fix it.
##



C_INCLUDE_PATH:='.'
LIBRARY_PATH:='.'

## See if GSL_HOME is set; if so, use it
ifdef GSL_HOME
	C_INCLUDE_PATH:=${C_INCLUDE_PATH}:${GSL_HOME}/include
	LIBRARY_PATH:=${LIBRARY_PATH}:${GSL_HOME}/lib
endif

## See if FFTW3_HOME is set; if so, use it
ifdef FFTW3_HOME
	C_INCLUDE_PATH:=${C_INCLUDE_PATH}:${FFTW3_HOME}/include
	LIBRARY_PATH:=${LIBRARY_PATH}:${FFTW3_HOME}/lib
endif

.DEFAULT_GOAL := build_message
build_message:
	@echo "The default make target has changed. If you want to build and install"
	@echo "the code, you should run"
	@echo "  python setup.py install --user"
	@echo "to build and install into the default user-local directory, or"
	@echo "  python setup.py install"
	@echo "to build and install into the system (or virtualenv) directory."
	@echo "Remaining options in this Makefile are just for general reference in"
	@echo "the event that build complications arise and they may be helpful."

install :
	python setup.py install

install_user :
	python setup.py install --user

spinsfast :
	build=build/config.mk $(MAKE) -C spinsfast
	cp spinsfast/lib/spinsfast.so $(shell python -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())")/








############################################
## Flags for building only the c++ files: ##
############################################
# The compiler needs to be able to find the GSL (GNU Scientific
# Library) headers and libraries.  The following paths are the most
# common places for these to be installed.  If compilation doesn't
# work, correct these paths.
INCFLAGS = -I/opt/local/include -I/usr/local/include -Ispinsfast/include -IQuaternions -ISpacetimeAlgebra
LIBFLAGS = -L/opt/local/lib -L/usr/local/lib -Lspinsfast/lib
ifdef GSL_HOME
	INCFLAGS := -I${GSL_HOME}/include ${INCFLAGS}
	LIBFLAGS := -L${GSL_HOME}/lib ${LIBFLAGS}
endif
# Set compiler name and optimization flags here, if desired
C++ = g++
OPT = -O3 -Wall -Wno-deprecated
## DON'T USE -ffast-math in OPT


#############################################################################
## The following are pretty standard and probably won't need to be changed ##
#############################################################################

# Tell 'make' not to look for files with the following names
.PHONY : all cpp clean allclean realclean swig spinsfast SphericalFunctions

# If needed, we can also make object files to use in other C++ programs
cpp : Utilities.o Quaternions/Quaternions.o Waveforms.o PNWaveforms.o Scri.o SpacetimeAlgebra/SpacetimeAlgebra.o WaveformsAtAPointFT.o

# This is how to build those object files
%.o : %.cpp %.hpp Errors.hpp
	$(C++) $(OPT) -DCodeRevision=4 -c $(INCFLAGS) -DUSE_GSL $< -o $@

# The following are just handy targets for removing compiled stuff
clean :
	-/bin/rm -f *.o
	-/bin/rm -rf build spinsfast/python/build spinsfast/lib/* spinsfast/build/lib* spinsfast/build/temp.*
	-build=build/config.mk $(MAKE) -C spinsfast clean
allclean : clean
realclean : allclean

# This just runs SWIG, which can be handy for committing pre-swigged files
swig :
	swig -python -globals constants -c++ -builtin -outdir SWIG/ -DUSE_GSL -o SWIG/GWFrames_wrap.cpp SWIG/GWFrames.i
