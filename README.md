Sexyfox
=======

Sexyfox copies over your firefox profile to a tmpfs (/tmp is assumed to be one) so that disk writes (which firefox seems to do a lot of) are done in RAM. It then copies it back after firefox closes.

Is that a good idea? I honestly can't remember. I did the experiment a few years ago and I'm doing it again to see if it does make a difference. I have an old copy of the script I used and I'm pushing it to see...

How it works
------------

1. The profile directory is backed up (see "Profile Safety" below)
2. The profile directory is copied over to the tmpfs
3. The profile directory is made into a symlink to the tmpfs
4. firefox is launched
5. as soon as firefox is closed, the profile symlink is removed and replaced with the profile directory in the tmpfs

Profile Safety
--------------

As I just mentioned above, sexyfox copies your firefox profile into RAM. This means that your history, bookmarks, extension data, and other preferences will remain volatile so long as firefox is running. This means that an improper shutdown or battery loss is going to result in losing your firefox profile.

To prevent this from being a complete disaster, of course, the following steps are taken:

1. A backup of the profile folder is copied to profilename.previous before running firefox.
2. sexyfox periodically rsyncs the profile in the tmpdir to profilename.current (5 mins)
3. as soon as firefox is closed, the profile symlink is removed and replaced with the profile in the tmpfs

This means, of course, that your home directory will at any point in time have 2-3 copies of the mozilla profile directory.

If ever a power interruption occurs before firefox is closed, the profile will be invalid. In which case either remove the symlink (~/.mozilla/randomrandomrandom.profilename) and replace it with either the .current (latest sync) or the .previous (state before the sync) directory.

Usage
-----

Just run sexyfox instead of firefox.

If you have multiple named profiles, you can create symlinks to sexyfox named sexyfox-profilename, and each will launch a different firefox profile. sexyfox, by itself, will launch the default profile. For example, sexyfox-two will launch a firefox profile named "two".
