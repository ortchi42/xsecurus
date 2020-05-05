
Debian
====================
This directory contains files used to package securusd/securus-qt
for Debian-based Linux systems. If you compile securusd/securus-qt yourself, there are some useful files here.

## securus: URI support ##


securus-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install securus-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your securusqt binary to `/usr/bin`
and the `../../share/pixmaps/securus128.png` to `/usr/share/pixmaps`

securus-qt.protocol (KDE)

