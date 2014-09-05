ABOUT File Specification
========================


ABOUT files are a simple way to document the provenance (origin and
license) and other important or interesting information about a software
component. 

An ABOUT file is a small text file stored in the codebase side-by-
side with the software component files or archives that it documents. No
modification of the documented software is needed.

The ABOUT format is plain text with field name/value pairs separated by a
colon. It is designed first to be easy to read and create by hand by 
humans, rather than machines.

The format is well-defined and structured just enough to make it easy to 
process with software as well. It contains enough information to fulfill key 
license requirements such as creating attribution notices or 
collecting redistributable source code or collecting an inventory of components.

ABOUT files are typically checked-in a version control system with the code 
they document. 


Quick start
-----------

A simple and valid ABOUT file named httpd.ABOUT may look like this::

    about_resource: httpd-2.4.3.tar.gz
    name: Apache HTTP Server
    version: 2.4.3

    homepage: http://httpd.apache.org

    download: http://archive.apache.org/dist/httpd/httpd-2.4.3.tar.gz

    license: apache-2.0
    license_file: httpd.LICENSE
    notice_file: NOTICE
    copyright: Copyright (c) 2012 The Apache Software Foundation.


The meaning of this ABOUT file is:

- The file `httpd-2.4.3.tar.gz` is stored in the same directory as 
  the ABOUT file `httpd.ABOUT` that documents it.
- The name of this component is `Apache HTTP Server` with version
  `2.4.3`.
- The homepage for this component is `http://httpd.apache.org`
- The file `httpd-2.4.3.tar.gz` was originally downloaded from 
  `http://archive.apache.org/dist/httpd/httpd-2.4.3.tar.gz`
- In the same directory, `httpd.LICENSE` and `NOTICE` are text files that
  contain respectively the license text and the notice text for this
  component.


File Specification
------------------

An ABOUT file is a text file with lines of colon-separated name:value pairs.
This format is loosely based on the Email header field format as specified 
in `RFC5322/RFC822 <http://tools.ietf.org/html/rfc5322>`_ .
There are some differences though such as UTF-8 support.


Naming ABOUT files
~~~~~~~~~~~~~~~~~~

An ABOUT file has an ".ABOUT" extension. 
A file name can contain only these US-ASCII characters:

- letters from A to Z
- digits from 0 to 9
- the "_" underscore, "-" dash and "." period signs.

The case of a file name and extension is not significant. 
On case-sensitive file systems (such as on Linux), a tool must report an 
error if two ABOUT files stored in the same directory have the same lowercase
file name. This is to ensure that ABOUT files can be used across different 
file systems and operating systems. The convention is to use a lowercase 
file name and an uppercase ABOUT extension as in `httpd.ABOUT`.


ABOUT file content
~~~~~~~~~~~~~~~~~~

An ABOUT file is a UTF-8 encoded text file. Lines contain field names/values
pairs. The standard line ending is the LF character but the line ending
characters can be any of LF, CR or CR/LF. Tools must normalize line endings
to LF. Empty lines and lines containing only white spaces that are not part 
of a field value and must be ignored.


Fields specification
--------------------

Field name
~~~~~~~~~~

A field name can contain only these US-ASCII characters:

- the first character must be a letter from A to Z
- the other characters can be any of these:
    - digits from 0 to 9
    - letters from A to Z
    - the "_" underscore sign.

Field names are not case sensitive. For example, "Homepage" and "HOMEPAGE"
represent the same field name.

A field name must start at the beginning of a new line. It can be followed by
one or more spaces that must be ignored.

Standard fields have their name defined in this specification. 
All other are custom fields.


Field Value
-----------

The field value is separated from a field name by a colon ":". Spaces before 
and after the colon must be ignored. 

A field value is composed of one or more lines of text. When a field value
contains more than one line of text, additional continuation lines must start
with at least one space. In this case, the first space of a continuation line
is ignored and is not part if the field value.
Trailing spaces in a value must be ignored.

For example, this description field value spans multiple lines::

    description: This is a long description for a
     software component that spans multiple lines with arbitrary line
     breaks.

A field value may be specified as:

- a `single line string` such as a component name, 
- a `multiple line string` using continuation lines such as a description text, 
- a `path` pointing to a file or directory,
- a `URL`,
- a `text file path` such as a license text,
- a `flag` which is a `single line string` with a yes/no value,
- a `list` of strings, paths, text file paths or URLs such as a list of licenses.


Path value
~~~~~~~~~~

A field may point to path such as a license text.  A path:

- must be on a single line,
- must use POSIX paths separators (e.g. "/" forward slash),
- must be a path relative to the path of the ABOUT file parent directory.

For example, for a license text is stored in a file named COPYING use this::

    license_file: COPYING


Here the license file is stored in a doc directory, one directory
above the ABOUT file directory using a relative path::

    license_file: ../docs/ruby.LICENSE


Text file path value
~~~~~~~~~~~~~~~~~~~~

A path value can point to a text file path such as a license text. 
Text files content must be UTF-8-encoded text.


URL value
~~~~~~~~~

A field may contain a URL such as a homepage or a download URL: the value
must be a valid absolute URL. For example::

    download: http://www.kernel.org/pub/linux/kernel/v3.0/linux-3.4.20.tar.bz2


Flag value
~~~~~~~~~~

Flag have a "yes" or "no" value (aka. a boolean value). A flag can have the 
following values in any upper or lower case combination:
- Yes, y are equivalent to yes. 
- No, n are equivalent to no. 



List value
~~~~~~~~~~~~

A list value contains multiple string lines where each line is interpreted 
as a discrete item. Leading and trailing spaces are ignored.



Fields validation
~~~~~~~~~~~~~~~~~

When processing an ABOUT file, tools must report a warning or error if a
field is invalid. A field can be invalid for several reasons, such as invalid
field name syntax or invalid content. Tools should report additional
validation error details. The validation should ensure that field
names and values are correct according to this specification.
A field can be required or optional.  Tools must report an error for 
missing required fields. 
For certain fields, specific validations may apply such as checksum
verification, URL validation or path resolution and existence check.


Fields order and precedence
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The field order is not significant. If there are multiple occurrences of a
field name, only the last occurrence is considered as the value for this field.
Previous occurrences of this field must be ignored. A tool must issue a 
warning when a field name occurs more than once in an ABOUT file.



Fields definition
-----------------

Required fields 
~~~~~~~~~~~~~~~

- `about_resource`: Used to reference the resources (files or
  directories) documented by an ABOUT file. List of paths.  
- `name`: Name of the documented component. Single line string. 


`about_resource`
~~~~~~~~~~~~~~~~

The `about_resource` field is used to reference one or more files or
directories documented in an ABOUT file. This is a list of paths values. 

For example, a file named django.ABOUT contains this field to
document the django-1.2.3.tar.gz archive stored in the same directory::

    about_resource: django-1.2.3.tar.gz

This ABOUT file documents a whole sub-directory and a single file::

    about_resource: downloads/linux-kernel-2.6.23/
     downloads/include/linux/kernel.h

Use a "." (period) to reference the whole directory where the ABOUT file is 
stored::

    about_resource: .

All paths are interpreted relative to the ABOUT file location therefore, "/"
also references the current directory::

    about_resource: /


Origin fields
~~~~~~~~~~~~~
Use these fields to document a component origin:

- `version`: Component version (or a version control revision). If not 
  available, the version should be a download date. Single line string.
- `download`: Download URL for the original component. List of URLs.
- `homepage`: Homepage for this component. List of URLs.
- `owner`: Name of the primary organization(s) or person(s) that owns
  or provides the component. List of strings.
- `author`: Name of the organization(s) or person(s) that authored the
  component. List of strings.
- `contact`: Contact information (such as an email address or physical
  address) for the component owner. Multiple lines string.


Licensing fields
~~~~~~~~~~~~~~~~
Use these fields to document a component license:

- `license`: Short identifier for a license, such as a DejaCode license
  key or an SPDX license key. List of strings.
- `license_name`: Common name for a license, such as GNU General Public
  License, version 2. List of strings.
- `license_file`: License text file. List of text file paths.
- `copyright`: Copyright statement for the component. Multiple lines string.
- `notice_file`: File containing legal notice or credits. List of text file paths.
- `license_url`: URL to the license text for the component. List of URLs.
- `redistribute`: Set to yes if the license requires source code redistribution. Flag.
- `attribute`: Set to yes if the license requires publishing an attribution or credit notice. Flag.
- `track_change`: Set to yes if the license requires tracking changes made to a the component. Flag.
- `modified`: Set to yes if the component code has been modified. Flag.
- `changelog_file`: Changelog file for the component. List of text file paths.


Miscellaneous fields
~~~~~~~~~~~~~~~~~~~~

- `description`: Component description. Multiple lines string.
- `notes`: Notes and comments about the documented component or license. Multiple lines string.
- `spec_version`: Version of the ABOUT specification used for this file. Single line string.


Custom fields
~~~~~~~~~~~~~

A custom field is a field with a name not defined in this specification.
Custom fields must be collected and supported by tools. Their meaning is not
specified. Tools should report a warning for custom fields.
The value of a custom field is a multiple lines string.


Documenting files stored in a version control system (VCS)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These fields allow to reference files and directories stored in a version
control system. There are many VCS tools such as Git, Mercurial, Subversion,
CVS, etc. Addressing of a file or directory revision is specific to each tool.

- `vcs_tool`: VCS tool such as git, svn, cvs, etc. Single line string.
- `vcs_repository`: A repository URL or identifier to point to a
  repository such as the URL of a Subversion or Git repository. Single line string.
- `vcs_revision`: Revision identifier such as a revision hash or version
  number. Single line string.
- `vcs_path`: Path used by the VCS tool to point to a file, directory or
  module inside a repository. Single line string.
- `vcs_tag`: Version tag used by the VCS tool. Single line string.
- `vcs_branch`: Branch name used by the VCS tool. Single line string.

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

Note that some tools may require access control via user/password or 
certificate and this information must not be stored in an ABOUT file. 


Checksum
~~~~~~~~

A tool can optionally verify the integrity of file(s) documented by an ABOUT
file with checksums.  The `checksum` field support checksums (such as SHA1 
and MD5) used to verify integrity of about_resource files. 
`checksum` is a list of strings. The checksum value is prefixed with a 
checksum algorithm name such as "md5:", "sha1:", "sha256:". 
The checksum algorithms and formats are as defined in the 
`GNU Coreutils <http://www.gnu.org/software/coreutils/>`_ tools md5sum, 
sha1sum and sha256sum commands. When `about_resource` points to multiple 
paths, each `checksum` entry corresponds to the matching `about_resource` 
entry. Directories cannot have a checksum.


For example::

    checksum: md5:f30b9c173b1f19cf42ffa44f78e4b96c


Changes
~~~~~~~~~~~~~~~~~~~
- 2014.09.05: Renamed home_url to homepage and download_url to download. 
  Improved the presentation of value types and streamlined the text.
  
- 2014.08.25: Simplification, removed several fields. Support for
  multiple values on multiple lines for certain fields.

- 2013.02.19: Add Changes section. Removed ability to reference files
  inside an archive.
  
- 2013.02.22: Multiple text clarifications throughout the specification.

- 2013.05.03: Renamed about_file to about_resource, and also made
  about_resource a mandatory field.
  
- 2013.05.13: Introduced flags and renamed organization-related fields
  to owner. Use directory instead of folder consistently in the spec. Use
  value instead of body consistently in the spec. Renamed the scm extension
  to vcs (version control system). Streamlined and clarified several
  sections of the spec. Explained how a field and a field_file relate to
  each other.

- 2013.06.18: removed fields that are not necessary: modified, usage.
  Added details on file content and line endings. Added redistribute and
  attribute flags. Added author related fields.

- 2013.08.08: Initial check-in. Text clarifications and fixed typos.
