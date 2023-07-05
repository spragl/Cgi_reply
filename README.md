# NAME

SPRAGL::Cgi\_reply - Simple HTTP replies.

# VERSION

1.10

# SYNOPSIS

    use SPRAGL::Cgi_reply;

    my %df = map {(/^(\S+).+\s(\d+)\%/)} grep {/\d\%/} qx[ df ];
    reply_json \%df;

# DESCRIPTION

Simple module for sending simple HTTP replies. Geared towards CGI scripts.

CGI is simple and quick to code for, even though it is not so performant or fashionable. It nevertheless is handy when making quick and dirty web services that are not going to see a lot of load. HTTP Routing is handled by the file system. Adding or removing functionality is easy and orthogonal, like playing with Lego bricks.

The reply methods in SPRAGL::Cgi\_reply will exit when they have been called. The exit is based on "die". So it is catchable, END blocks will execute, et cetera.

# FUNCTIONS AND VARIABLES

Loaded by default:
[fail](#fail-c),
[redirect](#redirect-u),
[reply](#reply-s),
[reply\_html](#reply_html-s),
[reply\_json](#reply_json-hr),
[cexec](#cexec)

Loaded on demand:
[%status\_code](#status_code)

- fail( $c )

    Replies with the given return code plus the standard return message attached to that, and then exits. Can be given a second parameter, a string, to replace the standard return message with. As in:

        fail 404 , 'Lost in Space.'; # Instead of just fail 404;

- redirect( $u )

    Replies with a 302 redirect to the given URI, and then exits.

- reply( $s )

    Replies with the given string as plain/text, and then exits.

- reply\_html( $s )

    Replies with the given string as HTML, and then exits.

- reply\_json( $hr )

    Replies with the given hashref transformed into JSON, and then exits.

- csystem( $c )

    CGI system. Runs the given command and returns the output. Your script will wait for it to complete.

- cexec ...

    CGI exec. Executes a system command, and sends a reply of your choice, in one go. Works like exec ought to in a CGI context. Calling it looks like this:

        cexec('mysqldump orders > orders_backup.sql')->reply('Database backup started.');

    Or like this:

        cexec('sudo postfix stop && postfix start')->redirect('status.html');

    You must naturally be very careful about what system commands it is possible to run from your webserver.

- %status\_code

    A hash that maps return codes to standard return messages.

    Only loaded on demand.

## OPTIONAL NAMED PARAMETERS

Optional named parameters can be given in the reply calls. If the name is "redirect" the reply will be like calling redirect. If the name is anything else, a header with that name and value will be inserted in the reply.

Examples:

    reply $x.' seconds to go!' , refresh => 5; # Inserts a "Refresh: 5" header.

    fail 503 , 'We are down at the moment, please try again later' , 'Retry-After' => $t;

    fail 308 , redirect => 'https://perlmaven.com/'; # Redirecting with another code than 302.

# DEPENDENCIES

File::Spec
JSON

# KNOWN ISSUES

No known issues.

# TODO

# SEE ALSO

[SPRAGL::Cgi\_read](https://metacpan.org/pod/SPRAGL::Cgi_read)
[CGI](https://metacpan.org/pod/CGI)

# LICENSE & COPYRIGHT

(c) 2022-2023 Bjrn Hee
Licensed under the Apache License, version 2.0
https://www.apache.org/licenses/LICENSE-2.0.txt
