======================
Layout of Comm Central
======================

These directories are included in the comm-central repository:

build
  Miscellaneous files used by the build process.
calendar
  Source code of the Lightning extension and Google Calendar Provider extension.
chat
  Files for the chat component of Thunderbird. There is also related code in 
  ``mail/components/im``.
common
  An assortment of code we've uplifted from mozilla-central as it was removed from there.
db
  The mork database.
editor
  UI for the Compose window (Thunderbird/SeaMonkey) and the Composer part of SeaMonkey.
ldap
  The LDAP C SDK. Used for communicating with LDAP servers.
mail
  Thunderbird specific source code.
mailnews
  Source code specific to the Mail and Newsgroups part of Thunderbird and SeaMonkey.
other-licenses
  Code that is not under the Mozilla license. See http://www.mozilla.org/MPL/ for more info.
rdf
  The RDF file format.
suite
  SeaMonkey specific source code.
taskcluster
  Files needed for Thunderbird's build infrastructure.
testing
  Files needed for Thunderbird's test infrastructure.

..
  This document is adapted from
  https://developer.mozilla.org/en-US/docs/Mozilla/Thunderbird/comm-central
