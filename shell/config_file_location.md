# Config file location

Generally speaking `/etc` is used by OS for its config files.
`/usr/local/etc` for config files by users and installed software.

`/usr/local` is usually the place for config files for applications built from source.
i.e. I install most of my packages using something like apt, but if I download a newer version of something or a piece of software not part of my distribution, I would build it from source and put everything into the `/usr/local` hierarchy.

This allows for separation from the rest of the distribution.

If you're developing a piece of software for others, you should design it so that it can be installed anywhere people want, but it should default to the regular [FHS](https://www.pathname.com/fhs/pub/fhs-2.3.html) specified system directories when they specify the prefix to be /usr (/etc, /usr/bin, etc.)

i.e. `/usr/local` is for your personal use, it shouldn't be the only place to install your software.

Have a good read of the FHS(File System Hierarchy) standard, and use the standard Linux tools to allow your source to be built and installed anywhere so that package builders for the various distributions can configure them as required for their distribution, and users can put it into `/usr/local` if they desire or the regular system directories if they wish.
