
Debian
====================
This directory contains files used to package aeriumxd/aeriumx-qt
for Debian-based Linux systems. If you compile aeriumxd/aeriumx-qt yourself, there are some useful files here.

## aeriumx: URI support ##


aeriumx-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install aeriumx-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your aeriumxqt binary to `/usr/bin`
and the `../../share/pixmaps/aeriumx128.png` to `/usr/share/pixmaps`

aeriumx-qt.protocol (KDE)

