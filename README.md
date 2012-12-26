TagPy
=====

TagPy is a Python crust (or a set of Python bindings) for Scott Wheeler's
TagLib [1](http://developer.kde.org/~wheeler/taglib.html). It builds upon
Boost.Python [2](http://www.boost.org/libs/python/doc/), a wrapper generation
library which is part of the Boost set of C++ libraries
[3](http://www.boost.org). It has its own web site
[4](http://mathema.tician.de/software/tagpy).

Just like TagLib, TagPy can:

- read and write ID3 tags of version 1 and 2, with many supported frame types
  for version 2 (in MPEG Layer 2 and MPEG Layer 3, FLAC and MPC),
- access Xiph Comments in Ogg Vorbis Files and Ogg Flac Files,
- access APE tags in Musepack and MP3 files.

All these have their own specific interfaces, but TagLib's generic tag
reading and writing mechanism is also supported. 

You can find examples in the test/ directory.

Andreas Kloeckner <inform@tiker.net>

Acknowledgments
===============

- Andreas Hemel <debian-bugs@daishan.de> sent a patch for a crash bug.
- Michal Čihař <nijel@debian.org> maintains the Debian package.
- Christoph Burgmer wrote the initial version of the new Python-only
  FileRef.
- Lars Wendler forwarded a patch from a Gentoo user.
- [hobophobe on github](https://github.com/hobophobe/tagpy) ported
  TagPy to Python 3.
- Keith Packard wrote a UCS4 to UTF8 routine that I shamelessly stole.

  Here's his copyright:

  > Copyright © 2000 Keith Packard
  >
  > Permission to use, copy, modify, distribute, and sell this software and its
  > documentation for any purpose is hereby granted without fee, provided that
  > the above copyright notice appear in all copies and that both that
  > copyright notice and this permission notice appear in supporting
  > documentation, and that the name of Keith Packard not be used in
  > advertising or publicity pertaining to distribution of the software without
  > specific, written prior permission.  Keith Packard makes no
  > representations about the suitability of this software for any purpose.  It
  > is provided "as is" without express or implied warranty.
  >
  > KEITH PACKARD DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE,
  > INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS, IN NO
  > EVENT SHALL KEITH PACKARD BE LIABLE FOR ANY SPECIAL, INDIRECT OR
  > CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE,
  > DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER
  > TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR
  > PERFORMANCE OF THIS SOFTWARE.


Building TagPy
==============

Step 0: Verifying that you have the right dependencies
------------------------------------------------------

TagPy works for me with

- TagLib 1.4
- Boost.Python 1.33
- gcc 4.0

I have reason to believe that slightly older versions of gcc and 
Boost.Python should be fine, but the 1.4 requirement for TagLib is
firm. Anything newer is also ok.

Step 1: Installing Boost.Python
-------------------------------

In Debian, it suffices to do "aptitude install libboost-python-dev".
The distribution is preconfigured for this case. You may skip to step
2.

For other distributions, check [this
howto](http://wiki.tiker.net/BoostInstallationHowto).

Step 2: Installing TagLib
-------------------------

In Debian, it suffices to do "aptitude install libtag1-dev".  The distribution
is preconfigured for this case. You may skip to step 3.

Install TagLib from its homepage, using the usual

    configure; make; make install

For details, you may consult the file `INSTALL' in the TagLib distribution.

Step 3: Installing TagPy
------------------------

If necessary, edit the file `setup.py', namely the section labelled
"USER CUSTOMIZABLE SECTION" to make sure the compiler will find your
installations of Boost and TagLib.

Then, run 

    python setup.py build

After a little wait, TagPy should finish building (if not, try and
go back to tweaking `setup.py', depending on the error message).

Finally, typing

    su -c "python setup.py install"

will complete the installation. 

Congratulations! You are now ready to use TagPy.

Using TagPy
===========

Using TagPy is as simple as this:

    >>> import tagpy
    >>> f = tagpy.FileRef("la.mp3")
    >>> f.tag().artist
    u'Andreas'

The `test/` directory contains a few more examples. 

In general, TagPy duplicates the TagLib API, with a few notable
exceptions:

- Namespaces (i.e. Python modules) are spelled in lower case.
  For example, `TagLib::Ogg::Vorbis` is now `taglib.ogg.vorbis`.

- Enumerations form their own scope and are not part of any
  enclosing class scope, if any.

  For example, the value "TagLib::String::UTF16BE" from the 
  enum `TagLib::String::Type` is now `tagpy.StringType.UTF16BE`.

- `TagLib::String` objects are mapped to and expected as Python
  unicode objects.

- `TagLib::ByteVector` objects are mapped to regular Python
  string objects.
