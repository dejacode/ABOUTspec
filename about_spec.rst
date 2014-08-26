ABOUT File Specification
========================


Purpose
-------

An ABOUT file provides a simple way to document the provenance (origin and
license) and other important or interesting information about a software
component. An ABOUT file is a small text file stored in the codebase side-by-
side with the software component file or archive that it documents. No
modification of the documented software is needed.

The ABOUT format is plain text with field name/value pairs separated by a
colon. It is easy to read and create by hand and is designed first for
humans, rather than machines. The format is well-defined and structured just
enough to make it easy to process with software as well. It contains enough
information to fulfill key license requirements such as creating credits or
attribution notices, collecting redistributable source code or an inventory
of components.


Getting Started
---------------

A simple and valid ABOUT file named httpd.ABOUT may look like this::

    about_resource: httpd-2.4.3.tar.gz
    name: Apache HTTP Server
    version: 2.4.3

    home_url: http://httpd.apache.org

    download_url: http://archive.apache.org/dist/httpd/httpd-2.4.3.tar.gz

    license: apache-2.0
    license_file: httpd.LICENSE
    notice_file: NOTICE
    copyright: Copyright (c) 2012 The Apache Software Foundation.


The meaning of this ABOUT file is:

-   The file "httpd-2.4.3.tar.gz" is stored in the same directory and
    side-by-side with the ABOUT file "httpd.ABOUT" that documents it.
-   The name of this component is "Apache HTTP Server" with version
    "2.4.3".
-   The home URL for this component is http://httpd.apache.org
-   The file "httpd-2.4.3.tar.gz" was originally downloaded from 
    http://archive.apache.org/dist/httpd/httpd-2.4.3.tar.gz
-   In the same directory, "httpd.LICENSE" and "NOTICE" are files that
    contain respectively the license text and the notice text for this
    component.


Specification
-------------

An ABOUT file is a text file with lines of colon-separated name:value pairs. 
This format is loosely based on the Email header field
format as specified in `RFC5322/RFC822 <http://tools.ietf.org/html/rfc5322>`_ 
There are some differences though: for instance the text must be encoded as 
UTF-8.

ABOUT file name
~~~~~~~~~~~~~~~

An ABOUT file has an ".ABOUT" extension. 
A file name can contain only these US-ASCII characters:

-   letters from A to Z
-   digits from 0 to 9
-   the "_" underscore, "-" dash and "." period signs.

The case of a file name and extension is not significant. 
On case-sensitive file systems (such as on Linux), a tool must report an 
error if two ABOUT files stored in the same directory have the same lowercase
file name. This is to ensure that ABOUT files can be used across different 
file systems and operating systems. The convention is to use a lowercase 
file name and an uppercase ABOUT extension.


Lines of text
~~~~~~~~~~~~~

An ABOUT file contains lines of UTF-8 text. Lines contain field names/values
pairs. The standard line ending is the LF character. The line ending
characters can be any LF, CR or CR/LF and tools must normalize line endings
to LF when processing an ABOUT file. Empty lines and lines containing only
white spaces that are not part of a field value continuation are ignored.
Empty lines are commonly used to improve the readability of an ABOUT file.


Field name
~~~~~~~~~~

A field name must start with a uppercase and lowercase US-ASCII letter from A to
Z. A field name can contain only these US-ASCII characters:

-   the first character must be a letter from A to Z

The other characters can be any of these:

-   digits from 0 to 9
-   letters from A to Z
-   the "_" underscore sign.

Field names are not case sensitive. For example, "HOME_URL" and "Home_url"
represent the same field name.

A field name must start at the beginning of a new line. It can be followed by
one or more spaces that must be ignored.


Field value
~~~~~~~~~~~

The field value is separated from a field name by a colon ":" . The colon ":"
can be followed by one or more spaces that must be ignored. 
Trailing spaces are also ignored.

A field value is composed of one or more lines of text. When a field value
contains more than one line of text, additional continuation lines must start
with at least one space. In this case, the first space of a continuation line
is ignored and is not part if the field value.

When a field contains multiple lines of text, it may be specified either
as a single value (for instance a description spanning multiple text lines) 
or as a list of values (for instance multiple license file paths)

In this example the value of the description field spans multiple lines::

    description: This is a long description for a
     software component that spans multiple lines with arbitrary line
     breaks.

Fields are mandatory or optional
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

As defined in this specification, a field can be mandatory. Tools must report
an error for missing mandatory fields. Mandatory fields include:

-   about_resource: Used to reference the resources (files or
    directories) documented by an ABOUT file.
-   name: Name of the documented Component.


Custom fields
~~~~~~~~~~~~~

A custom field is a field with a name not defined in this specification.
Custom fields must be collected and supported by tools. Their meaning is not
specified. Tools should report a warning for custom fields.


Fields validation
~~~~~~~~~~~~~~~~~

When processing an ABOUT file, tools must report a warning or error if a
field is invalid. A field can be invalid for several reasons, such as invalid
field name syntax or invalid content. Tools should report additional
validation error details. The validation should ensure that field
names and values are correct according to this specification.
For certain fields, specific validations may apply such as checksum
verification, URL validation, path resolution and verification, and so forth.


Fields order and precedence
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The field order is not significant. If there are multiple occurrences of a
field name, only the last occurrence is considered as the value for this field.
Previous occurrences of this field must be ignored. A tool must issue a 
warning when a field name occurs more than once in an ABOUT file.


Field referencing files
~~~~~~~~~~~~~~~~~~~~~~~

Some fields may reference a file path such as a license text. 
In this case the field name is suffixed with "_file" and the field
value must be a path pointing to a text file. This path must be a POSIX path 
relative to the path of the ABOUT file parent directory. 
Files content must be UTF-8-encoded text. To reference multiple
files, set each file path on a new continuation line.

For example, the license text is often stored in a file named COPYING::

    license_file: COPYING


In this example, the license file is stored in a doc directory, one directory
above the ABOUT file directory, using a relative path::

    license_file: ../docs/ruby.LICENSE

Field referencing URLs
~~~~~~~~~~~~~~~~~~~~~~

A field may reference URLs such as a homepage or a download URL. In
this case the field name is suffixed with "_url" and the field must contain
a valid absolute URL. To reference multiple URLs, set each URLs on a new
continuation line. For example, a download URL is referenced this way::

    download_url: http://www.kernel.org/pub/linux/kernel/v3.0/linux-3.4.20.tar.bz2

Flag fields
~~~~~~~~~~~

Flag fields have a "true" or "false" value. Yes, y, true and t are True in
any lower or upper case combination. No, n, false and f are False in any
lower or upper case combination.


Referencing the resources (files or directories) documented by an ABOUT file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The about_resource field is used to reference one or more files or
directories documented in an ABOUT file. This field contains one value per
line. Each value is a file path relative to the location of the ABOUT
file parent directory. Paths must use POSIX paths "/" forward slash as 
path separators.

For example, a file named django.ABOUT contains the following field to
document the django-1.2.3.tar.gz archive stored in the same directory::

    about_resource: django-1.2.3.tar.gz

In this example, the ABOUT file documents a whole sub-directory and a single
file::

    about_resource: downloads/linux-kernel-2.6.23/
     downloads/include/linux/kernel.h

Use a "." period to reference the current whole directory::

    about_resource: .

All paths are interpreted relative to the ABOUT file location therefore, /
also references the current directory::

    about_resource: /

Origin fields
~~~~~~~~~~~~~
These fields document the origin of a Component:

-   version: Component version. A component usually has a version, such
    as a revision number or hash from a version control system (for a
    snapshot checked out from VCS such as Subversion or Git). If not
    available, the version should be the date the component was provisioned,
    in an ISO date format such as 'YYYY-MM-DD'.
-   download_url: A direct URL to download the original file or archive
    documented by this ABOUT file. One line per value.
-   home_url: URL to the homepage for this component. One line per value.
-   owner: The name of the primary organization(s) or person(s) that owns
    or provides the component. One line per value.
-   author: Name of the organization(s) or person(s) that authored the
    component. One line per value.
-   contact: Contact information (such as an email address or physical
    address) for the component owner.


Licensing fields
~~~~~~~~~~~~~~~~
These fields document the license of a Component:

-   license: Short identifier for a license, such as a DejaCode license
    key or an SPDX license key. One line per value.
-   license_name: Common name for a license, such as GNU General Public
    License, version 2.
-   license_file: License text file. One line per value.
-   copyright: Copyright statement for the component.
-   notice_file: File containing legal notice or credits. One line per
    value.
-   license_url: URL to the license text for the component. One line per
    value.
-   redistribute: Set this flag to yes if the component license requires
    source code redistribution. Defaults to No.
-   attribute: Set this flag to yes if the component license requires
    publishing an attribution or credit notice. Defaults to No.
-   track_change: Set this flag to yes if the component license requires
    tracking changes made to a the component. Defaults to No.
-   modified: Set this flag to yes if the component code has been
    modified. Defaults to No.
-   changelog_file: Changelog file for the component. One line per value.


Miscellaneous fields
~~~~~~~~~~~~~~~~~~~~

-   spec_version: The version of the ABOUT file format specification used
    for this file as a hint to readers and tools in order to support future
    versions of this specification.
-   description: Component description.
-   notes: Notes and comments about the documented component or license.


Documenting files stored in a version control system (VCS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These fields provide a simple way to reference files stored in a version
control system. There are many VCS tools such as Git, Mercurial, Subversion,
CVS, etc. Accurate addressing of a file or directory revision in each tool in
a uniform way may not be possible. Some tools may require access control via
user/password or certificate and this information should not be stored in an
ABOUT file. These fields allow handle the diversity of ways that VCS tools 
reference files and directories under version control:

-   vcs_tool: VCS tool such as git, svn, cvs, etc.
-   vcs_repository: A repository URL or identifier to point to a
    repository such as the URL of a Subversion or Git repository.
-   vcs_revision: Revision identifier such as a revision hash or version
    number.
-   vcs_path: Path used by the VCS tool to point to a file, directory or
    module inside a repository.
-   vcs_tag: Version tag used by the VCS tool.
-   vcs_branch: Branch name used by the VCS tool.

Some examples for using the vcs extension fields include::

    vcs_tool: git
    vcs_repository: git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git
    vcs_path: tools/lib/traceevent
    vcs_revision: b59958d90b3e75a3b66cd311661535f94f5be4d1

or::

    vcs_tool: svn
    vcs_repository: http://svn.code.sf.net/p/inkscape/code/inkscape_project/
    vcs_path: trunk/inkscape_planet/
    vcs_revision: 22886

checksum
~~~~~~~~

A tool can optionally verify the integrity of file(s) documented by an ABOUT
file with checksums.  This field support checksums (such as SHA1 and MD5)
used to verify integrity of about_resource files. 
The checksum value is prefixed with a checksum algorithm name such as "md5:",
"sha1:", "sha256:". The checksum algorithms and formats are as
defined in the `GNU Coreutils <http://www.gnu.org/software/coreutils/>`_ tools 
md5sum, sha1sum and sha256sum commands. 
When about_resource points to
multiple paths, each checksum line correspond to the matching 
about_resource line. Directories cannot have a checksum.

For example::

    checksum: md5:f30b9c173b1f19cf42ffa44f78e4b96c

Changes
~~~~~~~~~~~~~~~~~~~

-   2014.08.25: Simplification, removing several fields. Support for
    multiple values on multiple lines for certain fields.
-   2013.02.19: Add Changes section. Remove ability to reference files
    inside an archive.
-   2013.02.22: Multiple text clarifications throughout the
    specification.
-   2013.05.03: Renamed about_file to about_resource, and also made
    about_resource a mandatory field.
-   2013.05.13: Introduced flags and renamed organization-related fields
    to owner. Use directory instead of folder consistently in the spec. Use
    value instead of body consistently in the spec. Renamed the scm extension
    to vcs (version control system). Streamlined and clarified several
    sections of the spec. Explained how a field and a field_file relate to
    each other.
-   2013.06.18: removed fields that are not necessary: modified, usage.
    Added details on file content and line endings. Added redistribute and
    attribute flags. Added author related fields.
-   2013.08.08: Text clarifications and fixed typos.
