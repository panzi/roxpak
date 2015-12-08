roxpak
======

Basic usage:

	roxpak.py list <archive>                 - list contens of .pak archive
	roxpak.py unpack <archive>               - extract .pak archive
	roxpak.py mount <archive> <mount-point>  - mount archive as read-only file system

The `mount` command depends on the [llfuse](https://code.google.com/p/python-llfuse/)
Python package. If it's not available the rest is still working.

This script is compatible with Python 2.7 and 3 (tested with 2.7.5 and 3.3.2).

File Format
-----------

Enough of the file format is reverse engineered to dump the packed files, but
there are some fields that I don't know what they do.

File names are stored in some strange way where they use 3 bytes per character
where the first byte is always 1 and the second always 0. The third byte is the
actual chracter in ASCII.

Encoding: Little Endian


	┌──────────────────────────────┐
	│                              │
	│ Header                       │
	│                              │
	│    number of entries         │
	│                              │
	│ ┌──────────────────────────┐ │
	│ │                          │ │
	│ │ Entry                    │ │
	│ │                          │ │
	│ │    ???                   │ │
	│ │    file name lenght      │ │
	│ │    file name             │ │
	│ │    ???                   │ │
	│ │    file size             │ │
	│ │    file data             │ │
	│ │    NIL                   │ │
	│ │                          │ │
	│ └──────────────────────────┘ │
	│                              │
	│ ...                          │
	│                              │
	└──────────────────────────────┘

### Archive

	Offset  Size  Type        Description
	     0     4  uint32_t    number of entries (N)
		 1   N/A  Entry[N]    entries

### Entry

	    Offset  Size  Type        Description
	         0     4  ???         ???
	         4     4  uint32_t    file name lenght (N)
	         8     2  ???         always two zero bytes
	        10     N  byte[N*3]   file name
	  10+N*3+1     4  uint32_t    file size (S)
      10+N*3+5     S  uint8_t[S]  file data
	10+N*3+5+S     1  ???         always zero

Related Projects
----------------

 * [psypak](https://github.com/panzi/psypak): unpack, list and mount Psychonauts .pkg archives
 * [bgebf](https://github.com/panzi/bgebf): unpack, list and mount Beyond Good and Evil .bf archives
 * [unvpk](https://bitbucket.org/panzi/unvpk): extract, list, check and mount Valve .vpk archives
 * [u4pak](https://github.com/panzi/u4pak): unpack, list and mount Unreal Engine 4 .pak archives
 * [t2fbq](https://github.com/panzi/t2fbq): unpack, list and mount Trine 2 .fbq archives
 * [fezpak](https://github.com/panzi/fezpak): pack, unpack, list and mount FEZ .pak archives

BSD License
-----------
Copyright (c) 2015 Mathias Panzenböck

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
