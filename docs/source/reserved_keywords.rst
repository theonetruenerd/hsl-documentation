Reserved Keywords
====================

The following keywords are reserved words and can only be used with their pre-defined meaning:

-  abort

-  break

-  char

-  const

-  device

-  debug

-  define

-  dialog

-  echo

-  else

-  endif

-  error

-  event

-  event

-  file

-  float

-  for

-  function

-  global

-  goto

-  if

-  ifdef

-  ifndef

-  include

-  lock

-  long

-  loop

-  method

-  namespace

-  next

-  object

-  once

-  onerror

-  pause

-  pragma

-  private

-  return

-  resume

-  sequence

-  short

-  static

-  struct

-  string

-  synchronized

-  timer

-  unlock

-  variable

-  void

-  while

-  "__filename__"

Abort
---------------------
Terminates the program abnormally, by controlled aborting of the execution

Break
---------------------
A break statement can only be found within an iteration statement (e.g. while, for, loop). The program jumps to directly
after the iteration, ending that statement early

Char
---------------------
A single character. Strings are composed of chains of chars.

Const
---------------------
Used during variable/object declaration (e.g. const variable myVariable;). Specifies that the object or variable can't
be modified.

Device
---------------------
Defines a device (e.g. ML_STAR). Can be used with no initialiser (device ML_STAR;), with an initialiser of the layout
file (device ML_STAR("MyLayout.lay");), or with three initialisers (device ML_STAR("MyLayout.lay","ML_STAR",hslTrue);).
When coding new methods or libraries which use devices, always use three initialisers. The third initialiser should always
be hslTrue; it currently does nothing but is reserved for future use.

The deck layout file is searched for in:

- The directory that the method/library file is in

- The Method folder

- The Library folder

- Any directories in the PATH environment variable

Debug
---------------------
Obsolete and should not be used. In old versions it controlled debug information, and could be defined as any integer
between 0 and 5. Each level of debug contained all lower levels.

- 0: No debug information

- 1: Debug information of memory management

- 2: Debug information of symbol table management

- 3: Debug information of syntax tree evaluation during execution

- 4: Debug information of reduce steps during parsing

- 5: Debug information of tokens produced during scanning

Define
--------------------
Control directive, so used with a # in front of it. Format is #define id constant. Used to define the indicated name (id)
as the appropriate value (constant) within the global scope. This name is then predefined and cannot be reassigned via
an l-value.

Dialog
--------------------
Initialises a dialog object with the value entered in brackets as its title.

Echo
--------------------
Obsolete and should not be used. In old versions in controlled output listing and could be either 0 or 1.

- 0: No output listing

- 1: Output listing

Output stream is either the output stream set within the parser, or _stdout_ if no output stream has been set.

Else
--------------------
Used after an "if" statement to determine what happens if the condition is not met.

Endif
--------------------
Control directive, so used with a # in front of it. Closes an #ifdef or #ifndef directive.

Error
--------------------
Represents runtime errors and cannot be instantiated.

Event
--------------------
Initialises the event object with the value entered as its name.

File
--------------------
Initialises the file object with the value entered as connection string to its data source. Connection string indicates
the information used to establish a connection to a data source. The default provider is OLE DB Provider for Microsoft
Jet. The default connection string used is "Provider=Microsoft.Jet.OLEDB.4.0;Data Source=databaseName;properties;".
In this, databaseName is the file path for the data source, and properties can be "Extended Properties=Excel 8.0" to open
.xls files and "Extended Properties=Text" for any text files (.txt, .csv, .tab, .asc). Empty will be for Microsoft Jet
Databases (.mdb)

Float
-------------------
A decimal.

For
-------------------
An iteration statement. Conditional loop, which will happen for each item in a set condition. Format is:

for(v=0;v<2;v++) {
    ...
}

Which means v starts at 0, it repeats while v is less than 2, and after each loop v will increase by 1.

Function
-------------------
Defines a function which can be referenced and called in the method or by other functions. Can be prefixed by any of the
following declarators:

- Static

- Private

- Const

- Global

- Synchronized

Functions take formal parameters as arguments (e.g. function FunctionName(variable variableOne, variable& variableTwo))
In this case, the & symbol after the word variable allows the function to edit the value of the variable and the method
will use the new value once the function has resolved; without the & symbol the function can edit the variable but once
the function is finished the value will revert to the original value.

Global
------------------
Scope declarator. When used the named object is shared with the entire system, outside of the namespace it is declared in.
This is useful sharing objects among tasks in a workflow. If a named global object is declared repeatedly all declarations
are used as aliases for the same instance of the object.

GoTo
------------------
Used within error handling, always after an "onerror" keyword, and followed by either an id or a 0. If followed by an id,
when there is a runtime error the program jumps to the id and continues from there. If it is followed by a 0, any currently
enabled error handler is disabled for the current function or method.

If
------------------
Evaluates a conditional expression and performs the actions within the block only if the expression evaluates to true.

Ifdef
------------------
A control directive, so prefixed by a #. If the identifier immediately after the #ifdef is defined, the block of code until
the next #endif is compiled.

Ifndef
------------------
A control directive, so prefixed by a #. If the identifier immediately after the #ifndef is not defined, the block of code
until the next #endif is compiled.

Include
------------------
A control directive, so prefixed by a #. Inserts the file with the name following the #include into the input stream of
the parser, so can be referenced within that code. The file name can either be relative or absolute. If relative, the file
will be searched for in the following locations:

- The same directory as the file that contains the #include statement, as well as any subdirectories

- The Methods directory

- The Library directory

- Any directories listed in the PATH environment variable

Lock
-----------------
A control-of-flow language keyword. Encloses a block of hsl statements (until the next unlock) so that the group can be
executed without interruption. Lock-unlock loops can be nested.

Long
-----------------
An integer of arbitrary size

Loop
-----------------
A loop with counter which will run the specified number of times.

Method
-----------------
A non-library hsl file should have exactly one method (i.e. exactly one function characterized with the keyword method).
The method represents the start of the program, takes no arguments, and cannot be called from within the program. A
return function terminates the method.

Namespace
-----------------
A namespace declaration identifies and assigns a name to a declarative region. Is used to ensure names of functions and
variables do not clash with others with the same identifiers.

Next
-----------------
Used within onerror handling in the format "onerror resume next". Will continue the program from the next line after
the one which the error occurred.

Object
----------------
An object declaration introduces a new objcet and links this with a unique name and datatype. Cannot be initialised.

Once
----------------
Used immediately following #pragma. Specifies that the file in which the pragma resides is only included once outside of
any namespace. Used to not share a library among multiple tasks/processes.

Onerror
----------------
Controls the error handling of the program. Can either be followed by "goto" or "resume".

Pause
----------------
Suspends the program execution at the next position which is not inside a lock-unlock block.

Pragma
----------------
A control directive, so is preceded by a #. Can be followed by the "once" keyword (to specify that the file in which
the pragma resides is only included once per namespace), or by the "global" keyword (to specify that the file in which
the pragma resides is only included once _outside_ of any namespace. A library included in a global library is also
global.

Private
----------------
Scope declarator. When used as a prefix to a variable or function, means that the target cannot be accessed outside of
the block it is found in (e.g. namespace or function). It also means the function will not be visible in the Graphical
Method Editor.

Return
----------------
Terminates a function or method. Can either just terminate the method, in which case the format is just "return;",
or return a specific value, in which case the format is "return(x);", where x is the value being returned.

Resume
----------------
Used immediately before the "next" keyword to resume execution after an error handler is finished running. Will resume
with the statement from the line after the error (if in the same function as the error handler), or immediately after the
statement that last called the statement containing the error handler (if in a different function).

Sequence
----------------
A sequence-type object, representing a sequence of positions of labware. No initialiser is accepted.

Short
----------------
A 32-bit signed integer.

Static
----------------
A declarator. When declaring an object or function at global scope, applying the static keyword ensures its name is not
exported by the Analyzer.

Struct
----------------
A struct(ure) defines an object that consists of a sequence of named components with different types.

String
----------------
A sequence of chars.

Synchronized
----------------
Scope declarator used for code parallelism and multithreading. Declaring a function as synchronized allows a program to
have multiple threads simultaneously executing the same function via the fork command.

Timer
----------------
A timer object. Has two optional initialisers; name (timer name), and view name (timer view name used in timer display)

Unlock
----------------
Control-of-flow keyword which pairs with lock to ensure a series of hsl statements can be executed without interruption.

Variable
----------------
An integer, float, or string value.

Void
----------------
Used to represent that a function returns nothing

While
----------------
Conditional loop which performs the specified task for as long as its condition is marked as true

"__filename__"
----------------
Follows #include keyword, postfix of a cstring containing an extension. Inserts the file with the same name as the file
including the include statement, but with the extension cstring. Otherwise has the same behaviour as the #include
keyword.