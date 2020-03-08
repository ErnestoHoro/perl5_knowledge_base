# Perl 5 Knowledge Base
**Practical Extraction and Report Language**

Author: Ernesto Horo

Version: 5.16.2 or greater

## About
* although containg some very basic knowledge this document targets rather language switchers than programming beginners
* rather than to be printed out this file is meant to be searched upon with "CTRL + F" to receive multiple results for different contexts
* therefore it is not meant to be read sequentially

## General
* www.perl.org | www.perldoc.perl.org
* non strict interpreter language (byte code)
* *.pl/*.pm file name extensions (optional)
* case sensitive declarations
* by default all variables are global
* every variable type gets a seperate namespace # e.g. $foo != @foo
* there is a difference between runtime and compile time warnings
* subroutines or variables can be refered to as symbols
* suggested IDEs: Padre, Eclipse (EPIC), PyCharm (Perl), Kommodo IDE

### Version History
* [Version History (Wikipedia)](https://en.wikipedia.org/wiki/Perl_5_version_history)


| Version   | Noteable Changes                                                                                                                                                          |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 5.0.10    | custom pragmata<br>smartmatch features ~~ (removed later)<br>Log::Log4perl<br>say (borrowed from Perl 6)                                                                  |
| 5.0.8     | https://code.activestate.com/ppm/Log-Log4perl/<br>https://code.activestate.com/ppm/Log-Message/<br>https://developers.redhat.com/products/softwarecollections/overview/   |
| 5.0.6     | built-in pragmata (e.g. warnings)<br>mode parameter for filehandles (<, >, >>)<br>global var declaration # our keyword                                                    |
| 5.0.0     | subroutines should be prototyped -> not clear if this is true, read release notes                                                                                         |

### Abbreviations
| Term      | Description                           |
| --------- | ------------------------------------- |
| CPAN      | Comprehensive Perl Archive Network    |
| XS/SWIG   | C/C++-Library Interfaces              |
| POD       | Plain Old Text Documentation          |

## Basic Syntax

### General
| Character(s)  | Name              | Description                                                                                                           |
| ------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------- |
| ;             | semicolon         | instruction ending                                                                                                    |
| \#            | hash              | comment character                                                                                                     |
| =             | equals            | Perl::Pod documentation indicator (like pythons """)                                                                  |
| _             | whitespace        | is ignored (except quoted); use underscores for filenames                                                             |
| \             | backslash         | escape character                                                                                                      |
| ()            | parenthesis       | is optional for function arguments                                                                                    |
| {}            | curly brackets    | used to access hash variables ```print $data{'this_key'};``` <br> used to omit string interpolation for a variable    |
| ""            | double qoutes     | interpolating special charaters (e.g. \n)                                                                             |
| ''            | single qoutes     | plain string notation                                                                                                 |
| (...)         | barewords         | certain names will be threated as quoted strings ```$bareword = bareword; >> print $bareword;```                      |

### Operators
* operator precedence is mathmatical (e.g. * before +)

| Operator      | Description (short)       | Description                                                                                                                                   |
| ------------- | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| ::            | package qualifier         | explicitly refer to variables of another package <br> ```$Foo::another_scalar_var;``` <br> ```$::bar; equals $main::bar;```                   |
| .             | concate                   | ```$str = "hello" . "world"; >>"helloworld"```                                                                                                |
| <>            | diamond                   | special filehandle shortcut (note: usually <> and $_ are not connected) <br> ```while (<>) { print $_ }```                                    |
| ..            | range operator            | (see SPECIAL LITERALS -> Sequential Arrays)                                                                                                   |
| x             | string multiplier         | ```('-' x 3) >> '---';``` automatic arithmetic conversion ```("12" x "3") >> 36 (type int)```                                                 |
| ->            | dereference a method/var  | ```@top_links = @{$m->links};``` call method links from the object instance in $m, therefore returns the value from a reference point to a location; more generic ```$firstName = $object->getFirstName();``` or in e.g. Python first_name = object_instance.get_first_name(); |
| ++ --         | increment / decrement     | -                                                                                                                                             |
| **            | exponent                  | ```$a ** $b```                                                                                                                                |
| %             | modulus                   | casts to int first                                                                                                                            |
| ?             | if...else equivalent      | (see LOOPS)                                                                                                                                   |
| <=>           | numeric equation          | returns -1, 0 or 1                                                                                                                            |
| lt gt         | string wise <; >;         | -                                                                                                                                             |
| le ge         | string wise <=; >=;       | -                                                                                                                                             |
| eq ne         | string wise ==; !=;       | -                                                                                                                                             |
| cmp           | string wise equation      | returns -1, 0 or 1                                                                                                                            |
| *=            | (assignment operators)    | (implemented as expected)                                                                                                                     |

### Operators - Bitwise
| Operator              | Description                                                           |
| --------------------- | --------------------------------------------------------------------- |
| &                     | binary and                                                            |
| |                     | binary or                                                             |
| ^                     | binary xor                                                            |
| ~                     | binary ones (bit wise inversion)                                      |
| << >>                 | binary shift (left/right)                                             |
| 1;                    | short for ```return True;```                                          |

# Operators - Quotes
| Operator              | Description                                                           |
| --------------------- | --------------------------------------------------------------------- |
| q{}                   | enclose a string in single quotes # e.g. q{string} >> 'string'        |
| qq{}                  | enclose a string in double quotes                                     |
| qx{}                  | enclose a string in invert quotes # e.g. qx{string} >> `string`       |
| \`\`                  | is a system command call (cmd return value)                           |

## Arguments/Flags
| Element                       | Description                                                   |
| ---------------------         | ------------------------------------------------------------- |
| perl -v                       | version info                                                  |
| perl -V                       | extended info (i.a. version, platform parameters, @INC)       |
| perl -e _script_              | run application send in as program                            |
| perl -d                       | debugger                                                      |
| perl -w                       | warnings (run with this flag by default)                      |
| perl -W                       | enable all warnings                                           |
| perl -X                       | disable all warnings                                          |
| perl -T                       | taint checks enabled (script cwd is not added to @INC)        |
| perl -I _dir_                 | append colon separated directories to be added to @INC        |
| perl -M_module_               | command line switch (equals use <module>;)                    |
| perl -Mmodulino               | see MODULINO CONCEPT                                          |
| perl -Mmodulino -e1           | -                                                             |
| perl -Mmodulino -e"main()"    | -                                                             |
| perl -Mdiagnostics _script    | -                                                             |
| -A                            | -                                                             |
| -B                            | -                                                             |
| -b                            | -                                                             |
| -C                            | -                                                             |
| -c                            | -                                                             |
| -f                            | -                                                             |
| -g                            | -                                                             |
| -k                            | -                                                             |
| -l                            | -                                                             |
| -M                            | -                                                             |
| -O                            | -                                                             |
| -o                            | -                                                             |
| -p                            | -                                                             |
| -r                            | -                                                             |
| -R                            | -                                                             |
| -S                            | -                                                             |
| -s                            | -                                                             |
| -t                            | -                                                             |
| -u                            | -                                                             |
| -x                            | -                                                             |
| -z                            | -                                                             |

## Environment & Packages
| Element               | Description                                                                                           |
| --------------------- | ----------------------------------------------------------------------------------------------------- |
| #!/usr/bin/perl       | Shebang Standard Interpreter Path (UNIX)                                                              |
| #!/usr/bin/env perl   | Shebang Standard Interpreter Path (UNIX; searches in $PATH)                                           |
| #!/perl               | Shebang Standard Interpreter Reference (Windows)                                                      |
| @INC                  | include module (*.pm) search directories (e.g.) /usr/lib/perl5/5.18.0                                 |
| lib _path_            | include module (*.pm) search directories (see also @INC) <br> ```use lib "$FindBin::RealBin/lib";```  |
| @EXPORT               | contains a list of symbols to be exported into a callers namespace                                    |
| @EXPORT_OK            | equals @EXPORT (conditional requirement)                                                              |
| h2xs                  | perl module tree utility (see http://perldoc.perl.org/h2xs.html)                                      |
| @ISA                  | see object orientation                                                                                |
| PERLLIB               | -                                                                                                     |

## Built-in

### Built-in Functions & Pragmas

#### Pragmas
* all modules with an effect on runtime or compile time behavior
* built-in pragmas since Perl 5.6
* custom pragmas since Perl 5.10

| Functions & Pragmas   | Description (short)           | Description                                                                                                                                   |
| --------------------- | ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| require               | -                             | loads a module to current package <br> subroutine names must be fully qualified to be called <br> ```require Foo; Foo::bar();```              |
| use                   | wraps require                 | loads a module to current package <br> subroutine names can be directly called  <br> ```use Foo; bar();```                                    |
|                       | use _version_                 | set up Perl version requirement <br> ```use 5.6.0;```                                                                                         |
|                       | use _lib_                     | append module paths to @INC <br> ```use lib ('dir1/', '/dir2/');```                                                                           |
|                       | use _feature_                 | make specific feature available                                                                                                               |
|                       | use _encoding_                | recognize encoding of a script file <br> ```use utf8;```                                                                                      |
|                       | use diagnostics               | equivalent of perl -w (or use splain on command line as another substitute) <br> ```returns 'Illegal division by zero at app.pl line 4'```    |
|                       | use warnings                  | allows more detailed setup than -w flag (e.g. limit to {enclosing block} or file)                                                             |
|                       | use strict                    | enable mandatory variable declaration (limited to {enclosing block} or file)                                                                  |
| no                    | no _module/version/list_      | logical opposite to use                                                                                                                       |
| package               | -                             | (see object orientation)                                                                                                                      |
| my                    | private declaration           | declare a new variable bound to the scope of the block where it is declared (lexical scoping) <br> ```my ($first, $second, $third);```        |
|                       | local                         | assign local variables to globals while executing from the block it was declared <br> ```local $string; SubroutineCall(#using_$string);```    |
|                       | state                         | -                                                                                                                                             |
| our                   | global declaration (package)  | (Perl 5.6.0 or greater) <br> ```our ($first, $second);```                                                                                     |
| eval                  | evaluation block (try)        | return values are stored to $@ or $EVAL_ERROR <br> ```eval {__PACKAGE__->main()}; if ($@) { ... }```                                          |
| sigtrap               | signal handler interface      | -                                                                                                                                             |
| do                    | execute                       | execute value of EXPR <br> ```do another_script.pl;```                                                                                        |

### format | Formatted Data Ouput
* allows to set up a template to format data
 ```perl
 format FormatName =                     # format EMPLOYEE =                                                              
 # ===================================                                                                    
 fieldline                               # @<<<<<<<<<<<<<<<<<<<<<< @<< # left justified                                                             
 value_name, value_age                   # $name $age                                                             
 .                                       # =================================== # .                                                              
 invoke formatting # e.g. write EMPLOYEE
 ```

# Compilation (perlmod)
* use these features with care & sparsely

| Function      | Description                                                       |
| ------------- | ----------------------------------------------------------------- |
| AUTOLOAD      | optional default subroutine identifier                            |
| BEGIN { ... } | every begin block is executed first (post compilation >> True?)   |
| UNITCHECK     |                                                                   |
| CHECK         |                                                                   |
| INIT          |                                                                   |
| END { ... }   | executed after the application has ended <br> can function as a form of exception handling (e.g.) to do a clean-up, clean disconnect or destructor on exit <br> multiple END blocks can be placed and will be executed in reverse order |
| DESTROY       |                                                                   |
                            
## Time
| Function      | Description                                                       |
| ------------- | ----------------------------------------------------------------- |
| time          | return current time (e.g. "Thu Oct 13 04:54:34 1994") (EPOC?)     |
| localtime     | time() with optional formatting                                   |
| gmtime        | localtime() in Greenwich Mean Time (GMT)                          |
| strftime      | POSIX time formatting (similar to python)                         |
| sleep         | takes seconds as int                                              |
| utime         | change access and modification time of files                      |

## Mathmatical
| Function      | Description                                                       |
| ------------- | ----------------------------------------------------------------- |
| abs           | return absolute value                                             |
| sqrt          | return square root                                                |
| rand          | return random fractional number between 0 and x                   |
| srand         | return the random number seed for the rand operator               |

# Common Libaries
* for module path Example/Plot/FourD.pm write use Example::Plot::FourD to make it useable
| Module        | Submodule         | Description                                                                                           |
| ------------- | ----------------- | ----------------------------------------------------------------------------------------------------- |
| Data          | Data::Dumper      | converts data structures to strings in Perl syntax ```Dumper(@array);```                              |
| Devel         | Devel::NYTProf    | Perl source code profiler                                                                             |
| File          | File::Copy        | provides cp and mv                                                                                    |
| File          | File::Path        | provides constants to be used with various built-in functions                                         |
| FindBin       | FindBin::RealBin  | locates the full path to the script dir <br> ```use FindBin; my $dir = $FindBin::RealBin;```          |
| Getopt        | Getopt::Long      | extended processing of command line options (argument parsing)                                        |
| Log           |                   | -                                                                                                     |
| PerlIO        | PerlIO::EOL       | -                                                                                                     |
| Pod           |                   | Plain Old Documentation                                                                               |
| SOAP          | SOAP::Lite        | lightweight interface to the Simple Object Access Protocol (SOAP) both on client and server side      |
| Test          | Test::More        | -                                                                                                     |
| XML           | XML::LibXML       | recommended XML lib                                                                                   |
| XML           | XML::Simple       | XML API (usage is discouraged, use LibXML)                                                            |

                                                                                                            
# Special Libaries

## Fcntl (Use case?)
* provides constants to be used with various built-in functions
| Example                       | Description                                                   |
| ----------------------------- | ------------------------------------------------------------- |
| ```use Fcntl;```              | import standard fcntl.h constants                             |
| ```use Fcntl ":flock";```     | import LOCK_* constants                                       |
| ```use Fcntl ":seek";```      | import SEEK_CUR, SEEK_SET, SEEK_END                           |
| ```use Fcntl ":mode";```      | import S_* stat checking constants                            |
| ```use Fcntl ":Fcompat";```   | import F* constants                                           |

## Programm Structure & Flow

### Booleans / Conditional Statements
| Bareword      | Description                                                                   |
| ------------- | ----------------------------------------------------------------------------- |
| exists        | -                                                                             |
| defined       | -                                                                             |
| undef         | undefined                                                                     |
| not           | -                                                                             |

### Control Flow Statements
| Function      | Description                                                                   |
| ------------- | ----------------------------------------------------------------------------- |
| break         | -                                                                             |
| each          | -                                                                             |
| redo          | restart the loop (or labeled block) without another condition evalutation     |
| goto          | allow to goto label, expr or &name (!!!strongly discouraged!!!)               |
| next          | iterate a block prematurly (continue equivalent?)                             |
| last          | break equivalent ```last unless (defined @specific_data);```                  |
| exit          | evaluates optional expression and exits / calls END routine                   |
| die           | raise exception and exit if uncaught                                          |

### Subroutines
prototype
shift

## Object Orientation
| Function | Description |
|---|---|
| caller | |
| bless | |

## Data Structure
scalar
index
rindex
reverse                   reverse lists, arrays, chars (scalar context) and range operations; mandatory assignment
length
sort                      sorts lists, arrays, chars (scalar context) and range operations; mandatory assignment; e.g. @sorted = sort @unsorted; \-> Numerical: @sorted = sort { $a <=> $b } @_;

# BUILD-IN (Other)
ref()                       return reference type of an object
fork()                      clone current process (n.a. by default on windows)
wait()                      -
waitpid()                   -

### Type Related
undef
chr
int
hex
oct
ord

### Strings
| Function      | Description                                                                                                   |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| chop          | removes and returns the (very) last character ```chop($string);```                                            |
| chomp         | removes trailing newline corresponding to $INPUT_LINE_SEPARATOR ($/) and returns number of removed characters |
| lc            | returns string as lowercase                                                                                   |
| uc            | converts string to uppercase                                                                                  |
| lcfirst       | returns first character as lowercase                                                                          |
| ucfirst       | returns first character as uppercase                                                                          |
| join          | -                                                                                                             |

### Arrays
push
pop

### Hashes
keys

## Debugging
| Function  | Description                                                                   |
| --------- | ----------------------------------------------------------------------------- |
| dump      | dumps the currently executing Perl interpreter and script into a core dump    |

## IO

### General
| Function  | Description                                                   |
| --------- | ------------------------------------------------------------- |
| print     | write to output                                               |
| printf    | write to output, allows format-strings with %_special-char_   |
| say       | print() + newline by $\ (output record separator)             |
| system    | system command call (boolean return value)                    |
| `<cmd>`   | system command call (cmd return value)                        |
| exec      | system command call without returning                         |
| readpipe  | system command call, returns collected std-out                |
| kill      | send signal to terminate a pseudo-process                     |

### Files & Directories
| Function  | Description                                                   |
| --------- | ------------------------------------------------------------- |
| umask     | set default user mask (unix permission) for current process   |
| chmod     | change permissions                                            |
| chown     | change owner                                                  |
| mkdir     | create directory                                              |
| chdir     | change working directory                                      |
| rmdir     | remove directory                                              |
| chroot    | set directory to root path for current process scope          |
| unlink    | remove file                                                   |
| rename    | rename a file                                                 |
| link      | create hard link                                              |
| symlink   | create symbolic link                                          |
| readlink  | return value of a symbolic link                               |

### FILEHANDLE & DIRHANDLE
* standard Perl I/O wrappers

| Function  | Description                                                                       |
| --------- | --------------------------------------------------------------------------------- |
| readdir   | reads a DIRHANDLE created by opendir()                                            |
| open      | associates a data stream to a FILEHANDLE <br> ```open($DATA, "<file.txt");```     |
| readline  | read a FILEHANDLE created by open()                                               |
| seek      | sets FILEHANDLE's position (file pointer)                                         |
| tell      | return pointer position (if specified)                                            |
| select    | change active filehandle (change back to STDOUT asap)                             |
| close     | closes FILEHANDLE (induces flush)                                                 |
| opendir   | associates directory to DIRHANDLE                                                 |
| seekdir   | sets DIRHANDLE's position                                                         |
| closedir  | close DIRHANDLE                                                                   |
| read      | reads a block of information # e.g. read FILEHANDLE;                              |
| write     | -                                                                                 |
| getc      | getCharacter                                                                      |

### FILEHANDLE & DIRHANDLE (System)
* these functions bypass normal buffered IO (default)
* offer closer OS interaction while missing comfort functionality
* use as set of functions (don't mix with read, print, write, seek, tell, eof, etc.)

| Function  | Description                                                                       |
| --------- | --------------------------------------------------------------------------------- |
| syscall   | executes system command as syscall EXPR, LIST (of arguments)                      |
| sysopen   | wrapper of open(), associates file to FILEHANDLE                                  |
| sysread   | wrapper of read()                                                                 |
| sysseek   | wrapper of seek(), sets FILEHANDLE's position                                     |
| syswrite  | print() equivalent                                                                |

### File Test Operators
| Flag | Description                                                                            |
| ---- | -------------------------------------------------------------------------------------- |
| -r   | file is readable by effective uid/gid                                                  |
| -w   | file is writable by effective uid/gid                                                  |
| -x   | file is executable by effective uid/gid                                                |
| -o   | file is owned by effective uid                                                         |
| -R   | file is readable by real uid/gid                                                       |
| -W   | file is writable by real uid/gid                                                       |
| -X   | file is executable by real uid/gid                                                     |
| -O   | file is owned by real uid                                                              |
| -e   | file exists                                                                            |
| -z   | file has zero size (is empty)                                                          |
| -s   | file has nonzero size (returns size in bytes)                                          |
| -f   | file is a plain file                                                                   |
| -d   | file is a directory                                                                    |
| -l   | file is a symbolic link (false if symlinks aren't supported by the file system)        |
| -p   | file is a named pipe (FIFO), or Filehandle is a pipe                                   |
| -S   | file is a socket                                                                       |
| -b   | file is a block special file                                                           |
| -c   | file is a character special file                                                       |
| -t   | filehandle is opened to a tty                                                          |
| -u   | file has setuid bit set                                                                |
| -g   | file has setgid bit set                                                                |
| -k   | file has sticky bit set                                                                |
| -T   | file is an ASCII or UTF-8 text file (heuristic guess)                                  |
| -B   | file is a "binary" file (opposite of -T)                                               |
| -M   | script start time minus file modification time, in days                                |
| -A   | same for access time                                                                   |
| -C   | same for inode change time (Unix, may differ for other platforms)                      |


# IDENTIFIERS
$           scalar                      number (int, float), char/string or reference # e.g. $bigfloat = -1.2E-23;
            \-> Multiline Strings       use single quote just typing over multiple linesor use the "here" sytax
            \-> V-Strings               use vector strings to encode chars by their encoding index; the preceiding v can be ommited context dependent # v064; v102.111.111; 102.111.111
            \-> undef                   is_a valid scalar; euqals numeric 0
@           array                       zero based indexing (i=0); while declared with @ arrays are accessed with the $ sign # e.g. @ages = (25, 30, 40); >> print $ages[0]
            \-> qw Operator             use the qw operator to (e.g.) do a split # e.g. @array = qw/This is an array/; >> print $array[0] . "\n"; (prints This)
                                        qw!String!StringSting!; delimiter is set by the enclosing chars (in e.g. !)
            \-> Sequential Arrays       use as counting shortcut (.. range operator) # e.g. @var_8 = (1..8)
            \-> Max Index               e.g. $array2 = (1, 2, 3); >> $max_index = $#array; (prints 2) # undef if not existing
            \-> Push/Pop                return undef if argument is an empty array
                                        push @ARRAY, LIST # e.g. push(@coins, "Penny"); or push @INC, 'fully_qualified_path_to_module_wiht_our_REST.pm'; or push @records, \%hash_ref;
                                        pop @ARRAY
                                        shift @ARRAY; extracts the first element of an array, while at TOP-Level it equals shift(@ARGV) and within subroutines shift(@_) by default
                                        unshift @ARRAY, LIST
            \-> Select/Slice            selection slice # e.g. @list_of_elements[1,2,5] (returns items 1, 2 and 5)
            \-> Splice                  splice @ARRAY, OFFSET [ , LENGTH [ , LIST ] ]
                                        cut an array open and insert another list at a given index position # e.g. @nums = (1..20); >> splice(@nums, 5, 5, 21..25);
            \-> Split                   split [ PATTERN [ , EXPR [ , LIMIT ] ] ]
                                        use it to transform strings to arrays # e.g. $string = "this-is-a-text"; >> split('-', $string); (returns array ("this", "is", "a", "text"))
            \-> Join                    join EXPR, LIST
                                        contrary expression to split() # e.g. join('-', @array)
            \-> Sort                    sort [ SUBROUTINE ] LIST
                                        sorting arrays based on ASCII numeric expression; use a subroutine to apply a specific algorithm; allows different sorting algorithms # e.g. sort(@array)
            \-> Merge                   when printed arrays are automatically concatenated strings # e.g. @numbers = (1,3,(4,5,6)); print @numbers, "\n"; (prints 13456)
@_          array (special)             contains subroutine parameters # e.g. @_[0] equals package_name in constructors; special array created from passing arguments to subroutines
%           hash                        unordered key/value set # e.g. %data = ('Fritz', 45, 'Lisa', 30, 'Kumar', 40); or %data = ('Fritz' => 45, 'Lisa' => 30, 'Kumar' => 40);
                                        when preceded by hyphen, quotations can be omitted # e.g. %data = (-Fritz => 45, -Lisa => 30, -Kumar => 40);
            \-> Key                     declare and access via $data{'key'} # e.g. %data{'Fritz'} = 45; or %data{-Fritz} = 45;
            \-> Remove                  use delete function # e.g. delete $data{'Fritz};
            \-> Slice                   to extract slices just pass multiple keys # e.g. @data{-Fritz, -Lisa};
            \-> Keys                    use keys to get an array of keys # e.g. @keys = keys @data
            \-> Values                  use values to get an array of values # e.g. @values = values @data
            \-> Existence               non existant key/value pairs return undefined (+ runtime warning)
                                        use exists for checking # e.g. exists(@data{'Fritz'});
           
            
# Special Literals/Tokens
* no string interpolation
* to use full names (e.g. $OS_ERROR) put ```use English;``` at your programs top
* (see seperate file 'Perl Special Variable Types')

| Literal       | Literal (long)                    | Description                                                                                                                               |
| ------------- | --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| _             | (general)                         | indicate private usage # e.g. sub _sub_routine_name { ... }                                                                               |
| __DATA__      | -                                 | similar to end (defines the end of the perl code in any file)                                                                             |
| __FILE__      | -                                 | current filename                                                                                                                          |
| __LINE__      | -                                 | current line number                                                                                                                       |
| __PACKAGE__   | -                                 | current package; default=main; set with package <pkg_name>; see also special variable $0                                                  |
| __SUB__       | -                                 | returns a reference to the current subroutine (perl 5.1.6 use feature;)                                                                   |
| __END__       | -                                 | indicates logical end of your program; any following text is ignored by the compiler, but may be read via DATA filehandle                 |
| $$            | $PROCESS_ID / $PID                | current process id                                                                                                                        |
| $_            | default input / pattern search    | optional control variable (see LOOPS)                                                                                                     |
| $[            | default index variable            | allows to change the stardard array start index variable (not recommended)                                                                |
| $]            | $PERL_VERSION                     | -                                                                                                                                         |
| $!            | $OS_ERROR / $ERRNO                | as numeric reference contains error code; as string reference contains system error string                                                |
| $/            | -                                 | input record separarator (e.g. applied by chomp)                                                                                          |
| $?            | $CHILD_ERROR                      | TODO signal handling                                                                                                                      |
| $_pipe_       | $OUTPUT_AUTOFLUSH                 | default auto flush for every write or print = 0; set to !0 to force autoflush                                                             |
| $"            | $LIST_SEPARATOR                   | e.g. in 'print "The array is: " . join($", @array);' >> $" default is space                                                               |
| $0            | $PROGRAM_NAME                     | -                                                                                                                                         |
| $(            | $REAL_GROUP_ID / $GID             | -                                                                                                                                         |
| $)            | $EFFECTIVE_GROUP_ID / $EGID       | -                                                                                                                                         |
| $^W           | warnings variable                 | -                                                                                                                                         |
| $~            | $FORMAT_NAME                      | special variable of a FILEHANDLE (No? Missunderstanding of the tutorial?)                                                                 |
| $^            | $FORMAT_TOP_NAME                  | use to define a report header                                                                                                             |
| $-            | $FORMAT_LINES_LEFT                | use to define a report footer                                                                                                             |
| $%            | $FORMAT_PAGE_NUMBER               | use to define a pagination (if your report is longer than one page)                                                                       |
| $=            | $FORMAT_LINES_PER_PAGE            | use to set the number of lines per page                                                                                                   |
$^L ($FORMAT_FORMFEED)

# LOOPS
if...elsif...else           -
unless                      logical opposite to if (equals if not)
unless...else               -
unless...elsif..else        -
switch                      (only for late perl versions)
?                           (ternary operator) Exp1 ? Exp2 : Exp3; # if Exp1 is True Then Exp2 Else Exp3
                            use to replace if...else condition
$_                          is the special variable of iterables (e.g. while, foreach); gets the default for print at any point of iteration (therefore can be omitted);
                            in general it is not recommended to use it explicitly in real code # e.g. chomp() referes to this variable implicitly by default
                            exceptional cases can be the usage in the context of grep, map or any;
                            e.g. foreach ('hickory','dickory','doc') { print; } >> prints hickorydickorydoc (within 3 iterations)
for( ; ; ) { do; }          while True equivalent # terminate with Ctrl + C
foreach                     e.g. foreach my $current_item (@array) { ... } or refer to the default iteration variable $_
defined                     bool check on an expression (returns false if undef) # e.g. unless (defined $params) { ... } >> if params undef (explicitly), do something




SUBROUTINES
===========
Declaration/Defintion
---------------------
* default return value is the last expression evaluated
sub <name> {
   body;
}
                                        
Call/Invoke
-----------
name( args );               syntax for perl versions prior to 5.0 was &subroutine_name( args ); which is deprecated, but still functional (bypasses subroutine prototypes)
name( $scalar, @array );    if you pass a scalar with an array there will be merged to an extended list, therfore pass the @array type variables at last
name( hash )                if you pass a hash it will be translated to a list of key/value pairs
sub AUTOLOAD { ... }        any calls to undefined subroutines will raise a call of AUTOLOAD as a form of exception handling


TUTORIALSPOINT.COM/PERL - CODE EXAMPLES
=======================================
(1) "Here" Documents
$a = 10;
$var = <<"EOF";
This is the syntax for here document and it will continue
until it encounters a EOF in the first line.
This is case of double quote so variable value will be 
interpolated. For example value of a = $a
EOF
print "$var\n";
-----------------------------------
(2) "Here" Documents
$var = <<'EOF';
This is case of single quote so variable value will not be 
interpolated. For example value of a = $a
EOF
print "$var\n";
-----------------------------------
(3) Get Number of Elements (of an array)
@text = ('One', 'Two', 'Three')
$size = @text # $size now equals the number of elements in @text
-----------------------------------
(4) Get Size of an object / a variable
@array = (1,2,3);
print scalar @array, "\n";
-----------------------------------
(5) Get Number of Elements (of an hash)
@hash = ('One', 56, 'Two', 57, 'Three', 58)
$size = keys @hash
$size = values @hash


OTHER CODE EXAMPLES
===================
(1) Hello World
$perl -e 'print "Hello World\n"'
(2) Chomp IO
chomp(§line = <STDIN>);
chomp(@lines = <STDIN>);


# References | http://perldoc.perl.org/perlreftut.html
* store the location of another value
$scalarref = \$foo;
$arrayref  = \@ARGV; # see perlrun @docu for this special array
$hashref   = \%ENV;
$coderef   = \&handler;
$globref   = \*foo;
$arrayref  = [1, 2, ['a', 'b', 'c']];                       anonymous array reference
$hashref   = {'Fritz' => 'Steve', 'Albert' => 'John'};      anonymous hash reference
$coderef   = sub { print "Boink!\n" };                      anonymous subroutine reference
$cref      = \&AnyFunction;                                 anonymous function reference
ref()                                                       returns type of reference
                                                        

# Code Style Guidelines (Larry Wall + common)
* 4 spaces indent (no tabs)
* curly opening on keyword line with leading space or all curly braces on a seperate line
* one line block including curlies allowed
* no space before :
* space after each comma
* logical blank lines between chuncks
* uncuddled elses (no leading curly on keyword line)
* no space between function name and opening paranthesis
* break long lines after operator
* var_names_like_this
* subroutineNamesAsCamelCase
* PackageNamesLikeThis
* all FILEHANDLES uppercase
* use strict (all variables must be declared)
* omit return statement in subroutines
                                                            
# Code Style Guidelines (Private)
* one line block including curlies allowed (use sparsely)
* no surrounding whitespace within parenthesis ```if ($loop_count <= 10) { ... }```
* single letter variables are okay for loop iterators ```my $i;```
* class variables defined at the top of a *.pm file should be entirely uppercase
* inline comments start lower case, while punctuation is omitted
* $d[0]->[0] is to be prefered over ${$d[0]}[0]
* subroutine_names_like_this
* don't omit return statement in subroutines
                                                            
                                                                    
# Object Orientation
* objects, classes, methods (common)
* like in python, there are no private variables
package             namespace declaration keyword
bless()             returns an object reference
class               to create a class, first build a package # e.g. package MyClass; (valid until next package keyword)
@ISA                perl variable to govern method inheritance (is_a) # e.g. our @ISA = qw(Person); (inherits from Person)
(overloading)       (see https://www.tutorialspoint.com/perl/perl_object_oriented.htm)
DESTROY             (optional) destructor function

## Self
* $self needs to be initalized in each subroutine
```perl
sub doCoolStuff {
  # @_ = ($obj, $a, $b)
  my $self = shift; # $obj to $self
  my ($a, $b) = @_;
}
```

Call
----
$myinstance->myMethod("my_parameter");
equals
myMethod($myinstance, "my_parameter");
while
myMethod("my_parameter"); will not make the instance available with the called subroutine
                                                            
Example
-------
package Person;  # the package declaration is mandatory for class creation
sub new { # new is a naming convention for perl constructors
   my $class = shift;
   my $self = {
      _firstName => shift,
      _lastName  => shift,
      _ssn       => shift,
   };
   print "First Name is $self->{_firstName}\n";
   print "Last Name is $self->{_lastName}\n";
   print "SSN is $self->{_ssn}\n";
   bless $self, $class;
   return $self;
}
$object = new Person( "Mohammad", "Saleem", 23234345 );

# Automated Accessor/Mutator Generation
package my_package
__PACKAGE__(e.g. my_package)->mk_accessors(qw(ex_variable name));
$object_instance->ex_variable;  # get 'ex_variable'
$object_instance->ex_variable(val)  # set 'ex_variable'
$object_instance->get(ex_variable);  # get 'ex_variable' (conventional accessor)
$object_instance->set(ex_variable, val);  # get 'ex_variable' (conventional mutator)
                                                                     
# Modulino Concept
* allows files to work as executables and modules simultaneously
* basically you write your code in a default OO style wrapping everything into subroutines except: main() if not caller(); or if (!caller) { ... }
* caller returns the context of the current subroutine call ($package_name), which is undef for CLI executed perl code
* main() implements standalone functionality (*.pl equivalent), which is only executed if caller() returns undef
                                                                        
# Databases
* use DBI (Database Independent Interface) abstraction layer
* (see https://www.tutorialspoint.com/perl/perl_database_access.htm)

# Debugging, Exception-Handling & Logging
* use strict (var declaration is mandatory, bareword usage restricted)
* use warnings on debugging
* use diagnostics for a full explaination of an error

if statement                -                   open(DATA, $file) || die "Error: Couldn't open the file $!"; (or write full conditional statements if appropriate)
die                         (CORE::die)         prints a warning to STDERR and exits application; subroutine $SIG{__DIE__};
warn                        (CORE::warn)        prints a warning to STDERR # e.g. warn "Can't create directory."; subroutine $SIG{__WARN__};
carp                        -                   sort of a traceback equivalent to warn
                            \-> cluck           supercharged carp; can print a full stacktrace (all included modules)
                            \-> croak           die equivalent (reports one hierarchical level higher call)
                            \-> confess         die and cluck call (prints full stacktrace)
                            (Carp::Always)      additional CPAN module; use Carp::Always (all warnings as full traceback)

## Logging
# Easy mode if you like it simple ...
use Log::Log4perl qw(:easy);
Log::Log4perl->easy_init($ERROR);
 
DEBUG "This doesn't go anywhere";
ERROR "This gets logged";
 
# ... or standard mode for more features:
 
Log::Log4perl::init('/etc/log4perl.conf');
 
--or--
 
# Check config every 10 secs
Log::Log4perl::init_and_watch('/etc/log4perl.conf', 10);
 
--then--
 
$logger = Log::Log4perl->get_logger('house.bedrm.desk.topdrwr');
 
$logger->debug('this is a debug message');
$logger->info('this is an info message');
$logger->warn('etc');
$logger->error('..');
$logger->fatal('..');
 
#####/etc/log4perl.conf###############################
log4perl.logger.house              = WARN,  FileAppndr1
log4perl.logger.house.bedroom.desk = DEBUG, FileAppndr1
 
log4perl.appender.FileAppndr1      = Log::Log4perl::Appender::File
log4perl.appender.FileAppndr1.filename = desk.log
log4perl.appender.FileAppndr1.layout   = \
                        Log::Log4perl::Layout::SimpleLayout
######################################################
                                                                                    
# Other
* CodeStyle: automatic inline markings
* CodeStyle: alternate line length (wrapping)
* CodeSytle: braces aligned (better code selection)
                                            
# Regex
=~ (!~)                 match operator
/.../                   expressions are quoted in slashes; alphanumeric chars match
/$var/                  variable names get interpolated
/.../i                  case insensitive match                   
\                       special characters have to be escaped to be recognized as string; otherwise their interpolated as regex symbols:
                        /           expression definition operator
                        ?           matches 0 or 1 time
                                    e.g. x? >> one optional 'x'
                        *           matches 0 or more times
                                    e.g. x* >> one or more 'x' in a row
                                    e.g. .* >> one or more arbitrary chars
                        .           matches 1 single arbitrary
                        +           matches 1 or more times
                        ^           negate expression
                                    e.g. [^012] >> exclude matching 0, 1 or 2
                        |
                        ( )
                        [ ]         group/class
                                    e.g. [012] >> matches 0, 1 or 2
                                    e.g. [0-9] >> matches any number between 0 and 9;
                                    e.g. m[iu]t >> matches 'mit' and 'mut'
                                    e.g. [\n\r] >> matches defined linebreak
                                    e.g. [Pp]erl >> matches 'Perl' and 'perl'
                        { }         repetition range operator
                                    x{3,8} >> matches 'x' if counted between 3 and 8 times in a row
                        special shortcuts; introduced with a backslash:
                        \d          arbitrary digit; equals [0-9]
                        \D          (negated) arbitrary digit; equals [^0-9]
                        \w          arbitrary alphanumeric; equals [a-zA-Z0-9_]
                        \W          (negated) \w
                        \s          space or whitespace-characters; equals [\n\r\t\f]
                        \S          (negated) \s
                        anchors:
                        ^           beginning of text
                                    e.g. /^[12].*/ >> search string starts with 1 or 2
                        $           end of text
                                    e.g. /.*[34]$/ >> search string ends with 3 or 4
                        \b          word border; mark beginning and end with \b to define it as exempted
                        \B          word inner; mark beginning and end with \B to define it as embedded into other chars
(examples)              trim string (lr)            s/^\s+|\s+$/g
                        
# Todo
set clobber
