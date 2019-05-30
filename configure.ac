#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.
AC_PREREQ([2.69])
AC_INIT([mobiusft], [1.9], [aguiar.eao@gmail.com])
AC_CONFIG_SRCDIR([src/mobius/unittest.cc])
AM_INIT_AUTOMAKE([foreign -Wall -Werror dist-bzip2 dist-xz dist-zip no-dist-gzip])
AC_CONFIG_HEADERS([config.h])
AM_PROG_AR
LT_INIT
AC_CONFIG_MACRO_DIR([m4])

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks for programs, headers and libraries
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AC_PROG_CXX
AC_PROG_CC
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AC_PROG_MKDIR_P
AC_PROG_LIBTOOL
AC_PROG_RANLIB
AX_CXX_COMPILE_STDCXX_11
AX_CXXFLAGS_WARN_ALL
AC_C_INLINE
AC_C_BIGENDIAN

AC_CHECK_HEADERS([fcntl.h stdlib.h string.h sys/ioctl.h unistd.h])
AC_CHECK_HEADER_STDBOOL
AC_STRUCT_TM
AC_HEADER_TIME

AC_FUNC_ERROR_AT_LINE
AC_FUNC_MALLOC
AC_CHECK_FUNCS([floor memset mkdir regcomp])
AC_CHECK_FUNCS([strchr strtoul strtoull])

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks datatypes
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AC_TYPE_UID_T
AC_TYPE_OFF_T
AC_TYPE_MODE_T
AC_TYPE_SIZE_T
AC_TYPE_SSIZE_T
AC_TYPE_INT8_T
AC_TYPE_INT16_T
AC_TYPE_UINT8_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_INT32_T
AC_TYPE_INT64_T
AC_CHECK_TYPES([ptrdiff_t])

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks for Python
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AM_PATH_PYTHON(2.6)
PY_PREFIX=`$PYTHON -c 'import sys ; print sys.prefix'`
PY_LIBS="-lpython$PYTHON_VERSION"
PY_CXXFLAGS="-I$PY_PREFIX/include/python$PYTHON_VERSION"
pkgpyexecdir=$pyexecdir/mobius
pkgpythondir=$pyexecdir/mobius
AC_SUBST([PY_LIBS])
AC_SUBST([PY_CXXFLAGS])

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks for pygtk
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AC_MSG_CHECKING(for pygtk 2.0 or newer)
OUT=`$PYTHON -c "import pygtk" 2>/dev/null`

if test $? -eq 0; then
  AC_MSG_RESULT(yes)
else
  AC_MSG_RESULT(no)
  AC_MSG_ERROR([PyGTK 2.0 or newer required. See http://www.pygtk.org])
fi

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks for pycairo
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AC_MSG_CHECKING(for pycairo)
OUT=`$PYTHON -c "import cairo" 2>/dev/null`

if test $? -eq 0; then
  AC_MSG_RESULT(yes)
else
  AC_MSG_RESULT(no)
  AC_MSG_ERROR([PyCairo not found. See http://cairographics.org/pycairo])
fi

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks zlib
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks if we should support libz.
AC_ARG_WITH(zlib,
    [AS_HELP_STRING([--with-zlib=DIR],[specify prefix directory for installed zlib package.])],
    [with_zlib=${withval}],
    [with_zlib=yes])

if test "x${with_zlib}" = "xno"; then
  have_zlib=no

elif test "x${with_zlib}" = "xyes"; then
  have_zlib=yes

else
  have_zlib=yes
  ZLIB_CXXFLAGS="-I${with_zlib}/include"

  if test -d ${with_zlib}/lib64; then
    ZLIB_LDFLAGS="-L${with_zlib}/lib64"

  elif test -d ${with_zlib}/lib; then
    ZLIB_LDFLAGS="-L${with_zlib}/lib"
  
  else
    AC_MSG_WARN([no /lib dir found at directory "${with_zlib}"])
  fi
fi

# checks if zlib is really available
if test "x${have_zlib}" = "xyes"; then
  OLD_CPPFLAGS=$CPPFLAGS
  OLD_LDFLAGS=$LDFLAGS
  CPPFLAGS="$ZLIB_CXXFLAGS $CPPFLAGS"
  LDFLAGS="$ZLIB_LDFLAGS $LDFLAGS"

  AC_SEARCH_LIBS([inflateEnd], [z], [have_zlib=yes], [have_zlib=no])
  AC_CHECK_HEADERS([zlib.h], [], [have_zlib=no])

  if test "x${have_zlib}" = "xyes"; then
    MSG_SUPPORT_ZLIB=yes
    AC_SUBST([ZLIB_CXXFLAGS])
    AC_SUBST([ZLIB_LDFLAGS])
  else
    MSG_SUPPORT_ZLIB="no (zlib not found. See www.zlib.net)"
    CPPFLAGS=$OLD_CPPFLAGS
    LDFLAGS=$OLD_LDFLAGS
  fi
else
  MSG_SUPPORT_ZLIB=no
fi

AM_CONDITIONAL([HAVE_LIBZ], [test x${have_zlib} = xyes])
  
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks for libtsk
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

# checks if we should support libtsk.
AC_ARG_WITH(libtsk,
    [AS_HELP_STRING([--with-libtsk=DIR],[specify prefix directory for installed libtsk package.])],
    [with_libtsk=${withval}],
    [with_libtsk=yes])

if test "x${with_libtsk}" = "xno"; then
  have_libtsk=no

elif test "x${with_libtsk}" = "xyes"; then
  have_libtsk=yes

else
  have_libtsk=yes
  LIBTSK_CXXFLAGS="-I${with_libtsk}/include"

  if test -d ${with_libtsk}/lib64; then
    LIBTSK_LDFLAGS="-L${with_libtsk}/lib64"

  elif test -d ${with_libtsk}/lib; then
    LIBTSK_LDFLAGS="-L${with_libtsk}/lib"
  
  else
    AC_MSG_WARN([no /lib dir found at directory "${with_libtsk}"])
  fi
fi

# checks if libtsk is really available
if test "x${have_libtsk}" = "xyes"; then
  OLD_CPPFLAGS=$CPPFLAGS
  OLD_LDFLAGS=$LDFLAGS
  CPPFLAGS="$LIBTSK_CXXFLAGS $CPPFLAGS"
  LDFLAGS="$LIBTSK_LDFLAGS $LDFLAGS"

  AC_SEARCH_LIBS([tsk_version_get_str], [tsk], [have_libtsk=yes], [have_libtsk=no])
  AC_CHECK_HEADERS([tsk/libtsk.h], [], [have_libtsk=no])

  if test "x${have_libtsk}" = "xyes"; then
    MSG_SUPPORT_LIBTSK=yes
    AC_SUBST([LIBTSK_CXXFLAGS])
    AC_SUBST([LIBTSK_LDFLAGS])
  else
    MSG_SUPPORT_LIBTSK="no (libtsk not found. See www.sleuthkit.org)"
    CPPFLAGS=$OLD_CPPFLAGS
    LDFLAGS=$OLD_LDFLAGS
  fi
else
  MSG_SUPPORT_LIBTSK=no
fi

AM_CONDITIONAL([HAVE_LIBTSK], [test x${have_libtsk} = xyes])

if test "x${have_libtsk}" != "xyes"; then
  AC_MSG_ERROR([LIBTSK development files not found. See https://www.sleuthkit.org/sleuthkit])
fi

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks libudev
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks if we should support libudev.
AC_ARG_WITH(libudev,
    [AS_HELP_STRING([--with-libudev=DIR],[specify prefix directory for installed libudev package.])],
    [with_libudev=${withval}],
    [with_libudev=yes])

if test "x${with_libudev}" = "xno"; then
  have_libudev=no

elif test "x${with_libudev}" = "xyes"; then
  have_libudev=yes

else
  have_libudev=yes
  LIBUDEV_CXXFLAGS="-I${with_libudev}/include"

  if test -d ${with_libudev}/lib64; then
    LIBUDEV_LDFLAGS="-L${with_libudev}/lib64"

  elif test -d ${with_libudev}/lib; then
    LIBUDEV_LDFLAGS="-L${with_libudev}/lib"
  
  else
    AC_MSG_WARN([no /lib dir found at directory "${with_libudev}"])
  fi
fi

# checks if libudev is really available
if test "x${have_libudev}" = "xyes"; then
  OLD_CPPFLAGS=$CPPFLAGS
  OLD_LDFLAGS=$LDFLAGS
  CPPFLAGS="$LIBUDEV_CXXFLAGS $CPPFLAGS"
  LDFLAGS="$LIBUDEV_LDFLAGS $LDFLAGS"

  AC_SEARCH_LIBS([udev_device_unref], [udev], [have_libudev=yes], [have_libudev=no])
  AC_CHECK_HEADERS([libudev.h], [], [have_libudev=no])

  if test "x${have_libudev}" = "xyes"; then
    MSG_SUPPORT_LIBUDEV=yes
    AC_SUBST([LIBUDEV_CXXFLAGS])
    AC_SUBST([LIBUDEV_LDFLAGS])
  else
    MSG_SUPPORT_LIBUDEV="no (libudev not found. See www.kernel.org.)"
    CPPFLAGS=$OLD_CPPFLAGS
    LDFLAGS=$OLD_LDFLAGS
  fi
else
  MSG_SUPPORT_LIBUDEV=no
fi

AM_CONDITIONAL([HAVE_LIBUDEV], [test x${have_libudev} = xyes])

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks sqlite3
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AX_LIB_SQLITE3

if test "x${SQLITE3_VERSION}" = "x"; then
  MSG_SUPPORT_SQLITE3=no
  AC_MSG_ERROR([SQLITE3 development files not found. See https://sqlite3.org])
else
  MSG_SUPPORT_SQLITE3=yes
fi

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# checks arg --enable-python-api
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AC_ARG_ENABLE(python_api,
        [AS_HELP_STRING([--enable-python-api],[build Python binding to the C++ API (default=yes)])],
        [enable_python_api=${enableval}],
        [enable_python_api=yes])

MSG_SUPPORT_PYTHON_API=${enable_python_api}
AM_CONDITIONAL([HAVE_PYTHON_API], [test x${enable_python_api} = xyes])

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# generates files
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AC_CONFIG_FILES([Makefile
                 mobius_bin.sh
                 src/Makefile
                 src/mobius/Makefile
                 src/mobius/core/Makefile
                 src/mobius/core/application.cc
                 src/mobius/crypt/Makefile
                 src/mobius/database/Makefile
                 src/mobius/datetime/Makefile
                 src/mobius/decoder/Makefile
                 src/mobius/disk/Makefile
                 src/mobius/encoder/Makefile
                 src/mobius/filesystem/Makefile
                 src/mobius/filesystem/tsk/Makefile
                 src/mobius/forensics/Makefile
                 src/mobius/forensics/registry/Makefile
                 src/mobius/imagefile/Makefile
                 src/mobius/imagefile/ewf/Makefile
                 src/mobius/imagefile/msr/Makefile
                 src/mobius/imagefile/split/Makefile
                 src/mobius/io/Makefile
                 src/mobius/model/Makefile
                 src/mobius/partition/Makefile
                 src/mobius/system/Makefile
                 src/mobius/turing/Makefile
                 src/python/Makefile
                 src/python/mobius/Makefile
                 src/python/mobius/core/Makefile
                 src/python/mobius/crypt/Makefile
                 src/python/mobius/database/Makefile
                 src/python/mobius/decoder/Makefile
                 src/python/mobius/disk/Makefile
                 src/python/mobius/filesystem/Makefile
                 src/python/mobius/forensics/Makefile
                 src/python/mobius/forensics/registry/Makefile
                 src/python/mobius/imagefile/Makefile
                 src/python/mobius/io/Makefile
                 src/python/mobius/model/Makefile
                 src/python/mobius/partition/Makefile
                 src/python/mobius/system/Makefile
                 src/python/mobius/turing/Makefile
                 src/python/pymobius/Makefile
                 src/python/pymobius/forensics/Makefile
                 src/python/pymobius/forensics/registry/Makefile
                 src/python/pymobius/p2p/Makefile
                 src/python/pymobius/p2p/ares/Makefile
                 src/python/pymobius/p2p/bittorrent/Makefile
                 src/python/pymobius/p2p/emule/Makefile
                 src/python/pymobius/p2p/emuletorrent/Makefile
                 src/python/pymobius/p2p/shareaza/Makefile
                 src/python/pymobius/turing/Makefile
                 src/python/examples/Makefile
                 src/tools/Makefile
                 src/unittest/Makefile
                 src/unittest/cpp/Makefile
                 src/unittest/cpp/benchmark/Makefile
                 src/unittest/python/Makefile
                 data/Makefile
                 doc/Makefile
                 doc/tutorial/Makefile
                 doc/developers_guide/Makefile
                 extensions/Makefile
                 ])
AC_OUTPUT

# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
# prints a summary
# =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
AC_MSG_NOTICE([
   Python API.................: $MSG_SUPPORT_PYTHON_API
   mobius::database...........: $MSG_SUPPORT_SQLITE3
   mobius::imagefile::ewf.....: $MSG_SUPPORT_ZLIB
   mobius::tsk................: $MSG_SUPPORT_LIBTSK
   mobius::system::device.....: $MSG_SUPPORT_LIBUDEV
   mobius::zlib functions.....: $MSG_SUPPORT_ZLIB
]);
