Bitcoin-qt: Qt4 GUI for Bitcoin
===============================

Build instructions
===================

Debian
-------

First, make sure that the required packages for Qt4 development of your
distribution are installed, for Debian and Ubuntu these are:

::

    apt-get install qt4-qmake libqt4-dev build-essential libboost-dev libboost-system-dev \
        libboost-filesystem-dev libboost-program-options-dev libboost-thread-dev \
        libssl-dev libdb4.8++-dev

then execute the following:

::

    qmake
    make

Alternatively, install Qt Creator and open the `bitcoin-qt.pro` file.

An executable named `bitcoin-qt` will be built.




Ubuntu
-------

1. Install git, make a ~/src directory and clone the memecoin-qt source code
apt-get install git
cd ~
mkdir src
cd src
git clone https://github.com/muddafudda/Memecoin	
sudo apt-get install git
cd ~
mkdir src
cd src
git clone https://github.com/muddafudda/Memecoin

2. Download the needed development files
sudo apt-get install build-essential libssl-dev \
libdb-dev libdb++-dev libboost-all-dev \
libqrencode-dev qt4-qmake libqtgui4 libqt4-dev	
sudo apt-get install build-essential libssl-dev \
libdb-dev libdb++-dev libboost-all-dev \
libqrencode-dev qt4-qmake libqtgui4 libqt4-dev

3. Fix memecoin-qt.pro

There is a little error in the memecoin-qt.pro file which forces make to use a very specific version of boost. Open ~/src/Memecoin/memecoin-qt.pro and change the line that says
LIBS += -lboost_system-mgw46-mt-sd-1_53 -lboost_filesystem-mgw46-mt-sd-1_53 -lboost_program_options-mgw46-mt-sd-1_53 -lboost_thread-mgw46-mt-sd-1_53
into
LIBS += -lboost_system -lboost_filesystem -lboost_program_options -lboost_thread

BOOST_LIB_SUFFIX=-mgw46-mt-sd-1_53
should become
BOOST_LIB_SUFFIX=

and deleting the following lines

isEmpty(BOOST_LIB_SUFFIX) {
macx:BOOST_LIB_SUFFIX = -mt
windows:BOOST_LIB_SUFFIX = -mgw44-mt-s-1_53
}

should help you fix errors like:

ddatamapper.o build/moc_transactiondesc.o build/moc_transactiondescdialog.o build/moc_bitcoinamountfield.o build/moc_transactionfilterproxy.o build/moc_transactionview.o build/moc_walletmodel.o build/moc_overviewpage.o build/moc_csvmodelwriter.o build/moc_sendcoinsentry.o build/moc_qvalidatedlineedit.o build/moc_qvaluecombobox.o build/moc_askpassphrasedialog.o build/moc_notificator.o build/moc_miningpage.o build/moc_rpcconsole.o build/qrc_bitcoin.o    -L/usr/lib/i386-linux-gnu -lboost_system -lboost_filesystem -lboost_program_options -lboost_thread -lrt -LC:/deps/boost/stage/lib -Lc:/deps/db/build_unix -Lc:/deps/ssl -lssl -lcrypto -ldb_cxx -lboost_system-mgw46-mt-sd-1_53 -lboost_filesystem-mgw46-mt-sd-1_53 -lboost_program_options-mgw46-mt-sd-1_53 -lboost_thread-mgw46-mt-sd-1_53 -lQtGui -lQtCore -lpthread
/usr/bin/ld: cannot find -lboost_system-mgw46-mt-sd-1_53
/usr/bin/ld: cannot find -lboost_filesystem-mgw46-mt-sd-1_53
/usr/bin/ld: cannot find -lboost_program_options-mgw46-mt-sd-1_53
/usr/bin/ld: cannot find -lboost_thread-mgw46-mt-sd-1_53
collect2: error: ld returned 1 exit status
make: *** [memecoin-qt] Error 1

4. Prepare the installation

cd ~/src/MemeCoin
qmake USE_UPNP=- USE_QRCODE=0 USE_IPV6=0

You can safely ignore following messages:
Project MESSAGE: Building without UPNP support
Project MESSAGE: Building with UPNP supportRemoved plural forms as the target language has less forms.
If this sounds wrong, possibly the target language is not set or recognized.

5. Build it

make

The last step might take a while, but once it’s finished, there should be an executable file called ‘memecoin-qt’ in ~/src/Memecoin/



Windows
--------

Windows build instructions:

- Download the `QT Windows SDK`_ and install it. You don't need the Symbian stuff, just the desktop Qt.

- Download and extract the `dependencies archive`_  [#]_, or compile openssl, boost and dbcxx yourself.

- Copy the contents of the folder "deps" to "X:\\QtSDK\\mingw", replace X:\\ with the location where you installed the Qt SDK. Make sure that the contents of "deps\\include" end up in the current "include" directory.

- Open the .pro file in QT creator and build as normal (ctrl-B)

.. _`QT Windows SDK`: http://qt.nokia.com/downloads/sdk-windows-cpp
.. _`dependencies archive`: https://download.visucore.com/bitcoin/qtgui_deps_1.zip
.. [#] PGP signature: https://download.visucore.com/bitcoin/qtgui_deps_1.zip.sig (signed with RSA key ID `610945D0`_)
.. _`610945D0`: http://pgp.mit.edu:11371/pks/lookup?op=get&search=0x610945D0


Mac OS X
--------

- Download and install the `Qt Mac OS X SDK`_. It is recommended to also install Apple's Xcode with UNIX tools.

- Download and install `MacPorts`_.

- Execute the following commands in a terminal to get the dependencies:

::

	sudo port selfupdate
	sudo port install boost db48 miniupnpc

- Open the .pro file in Qt Creator and build as normal (cmd-B)

.. _`Qt Mac OS X SDK`: http://qt.nokia.com/downloads/sdk-mac-os-cpp
.. _`MacPorts`: http://www.macports.org/install.php


Build configuration options
============================

UPNnP port forwarding
---------------------

To use UPnP for port forwarding behind a NAT router (recommended, as more connections overall allow for a faster and more stable bitcoin experience), pass the following argument to qmake:

::

    qmake "USE_UPNP=1"

(in **Qt Creator**, you can find the setting for additional qmake arguments under "Projects" -> "Build Settings" -> "Build Steps", then click "Details" next to **qmake**)

This requires miniupnpc for UPnP port mapping.  It can be downloaded from
http://miniupnp.tuxfamily.org/files/.  UPnP support is not compiled in by default.

Set USE_UPNP to a different value to control this:

+------------+--------------------------------------------------------------------------+
| USE_UPNP=- | no UPnP support, miniupnpc not required;                                 |
+------------+--------------------------------------------------------------------------+
| USE_UPNP=0 | (the default) built with UPnP, support turned off by default at runtime; |
+------------+--------------------------------------------------------------------------+
| USE_UPNP=1 | build with UPnP support turned on by default at runtime.                 |
+------------+--------------------------------------------------------------------------+

Notification support for recent (k)ubuntu versions
---------------------------------------------------

To see desktop notifications on (k)ubuntu versions starting from 10.04, enable usage of the
FreeDesktop notification interface through DBUS using the following qmake option:

::

    qmake "USE_DBUS=1"

Generation of QR codes
-----------------------

libqrencode may be used to generate QRCode images for payment requests. 
It can be downloaded from http://fukuchi.org/works/qrencode/index.html.en, or installed via your package manager. Pass the USE_QRCODE 
flag to qmake to control this:

+--------------+--------------------------------------------------------------------------+
| USE_QRCODE=0 | (the default) No QRCode support - libarcode not required                 |
+--------------+--------------------------------------------------------------------------+
| USE_QRCODE=1 | QRCode support enabled                                                   |
+--------------+--------------------------------------------------------------------------+


Berkely DB version warning
==========================

A warning for people using the *static binary* version of Bitcoin on a Linux/UNIX-ish system (tl;dr: **Berkely DB databases are not forward compatible**).

The static binary version of Bitcoin is linked against libdb4.8 (see also `this Debian issue`_).

Now the nasty thing is that databases from 5.X are not compatible with 4.X.

If the globally installed development package of Berkely DB installed on your system is 5.X, any source you
build yourself will be linked against that. The first time you run with a 5.X version the database will be upgraded,
and 4.X cannot open the new format. This means that you cannot go back to the old statically linked version without
significant hassle!

.. _`this Debian issue`: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=621425

Ubuntu 11.10 warning
====================

Ubuntu 11.10 has a package called 'qt-at-spi' installed by default.  At the time of writing, having that package
installed causes bitcoin-qt to crash intermittently.  The issue has been reported as `launchpad bug 857790`_, but
isn't yet fixed.

Until the bug is fixed, you can remove the qt-at-spi package to work around the problem, though this will presumably
disable screen reader functionality for Qt apps:

::

    sudo apt-get remove qt-at-spi

.. _`launchpad bug 857790`: https://bugs.launchpad.net/ubuntu/+source/qt-at-spi/+bug/857790
