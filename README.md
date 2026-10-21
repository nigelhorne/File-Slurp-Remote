# NAME

```
File::Slurp::Remote - read/write files on remote systems using ssh.
```

# SYNOPSIS

```perl
use File::Slurp::Remote;

write_remote_file($host, $file, @contents);

@lines = read_remote_file($host, $file);

$whole_thing = read_remote_file($host, $file);

# Log in as a different user on the remote system
$whole_thing = read_remote_file("$user\@$host", $file);
```

# DESCRIPTION

This is similar to [File::Slurp](https://metacpan.org/pod/File%3A%3ASlurp), but it reads and writes files on
remote systems using `ssh` to get there.

If the remote user name is not the same as the local one, pass the
host as `user@host`, just as you would to `scp`.

It uses ssh to get to the remote systems, through
[File::Slurp::Remote::SmartOpen](https://metacpan.org/pod/File%3A%3ASlurp%3A%3ARemote%3A%3ASmartOpen).  You can override the remote shell
command by redefining `$File::Slurp::Remote::SmartOpen::ssh`.

`$File::Slurp::Remote::scp` is still defined for backwards
compatibility, but it has never been used and setting it has no effect.

If the host is the local system (and no other user is given), the file
is read or written directly, without ssh.

Files whose names end in `.gz` or `.bz2` are decompressed as they are
read and compressed as they are written.

As well as the functions below, everything that [File::Slurp](https://metacpan.org/pod/File%3A%3ASlurp) exports
by default (`read_file`, `write_file` and so on) is exported.

# FUNCTIONS

## write\_remote\_file($host, $file, @contents)

Writes `@contents` to `$file` on `$host`, replacing anything already
there.  Returns nothing useful; it dies on failure.

## read\_remote\_file($host, $file)

In list context, returns the lines of `$file` on `$host`, each with
its line ending.  In scalar context, returns the whole file as one
string.

### API SPECIFICATION

#### Input

Both functions take positional arguments:

```perl
$host     string, required.  A hostname, or user@host to log in as a
          different user.  The user name ends at the last '@'.
          Neither the user nor the host may be empty or begin with '-'.
$file     string, required.  The path on the remote system.
@contents list of strings (write_remote_file only).  Written in order,
          with no separator added.
```

#### Output

```perl
read_remote_file, list context:   list of strings, one per line
read_remote_file, scalar context: string
write_remote_file:                nothing useful
```

# ERRORS

Both functions die if the host or user is invalid (see above), or if
the file cannot be opened, written or closed.  When ssh is used, a
failure on the remote side is reported when the file is closed, so
`write_remote_file` dies but `read_remote_file` may just return
nothing.

\# Unused: retained so that code which sets it doesn't break
our $scp = 'scp -q -o StrictHostKeyChecking=no -o BatchMode=yes -o PasswordAuthentication=no';

# LICENSE

Copyright (C) 2008-2007,2008-2010 David Sharnoff.
Copyright (C) 2007-2008 SearchMe Inc.
Copyright (C) 2011 Google Inc.
This package may be used and redistributed under the terms of either
the GPL 2 license.

# AUTHOR

David Muir Sharnoff

# SUPPORT

Module now maintained by
Nigel Horne, `<njh at nigelhorne.com>`
