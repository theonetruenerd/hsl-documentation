HSL Fundamentals
================

Hamilton Standard Language (HSL) is written in text format, and is organized into statements, blocks consisting of related sets of statements, functions, one method, and comments. Within a statement, use variables, immediate data such as strings and numbers, and expressions.
In many ways it is a C-like language, albeit with some quirks.

HSL can be used either to write methods or to write libraries of functions/constants/variables. Each of these are structured differently,
and will be gone through in more detail individually. To get started on writing a library, see :ref:`Writing a Library` <writing_a_library>`.
To get started on writing a method, see :ref:`Writing a Method <writing_a_method>`.

HSL has many :ref:`reserved keywords <reserved_keywords>` and :ref:`predefined constants <predefined_constants>`, which can be used
in any HSL file and cannot be reassigned. HSL also supports a range of :ref:`operators`.

HSL has two main scopes, global (accessible throughout the entire program), and local (accessible only within the function it is declared in).

Conventions
-----------

HSL has several programming conventions which are not strictly required but should be followed for best practices. These conventions
are used in all examples in this documentation, but may not be found in all libraries depending on who has written them.
They are listed here for clarity.

- Parameters being fed into functions should follow the following naming convention:
x_y_z, where x is whether the parameter is an input, output, or both (i, o, or io), y is the type of input (var for variable,
arr for array, seq for sequence), and z is the name of the parameter in camelCase. For variables or arrays of variables, z should begin with the
variable type - i.e. int for integer, str for string, bln for boolean, hex for hexadecimal, and flt for float. This means, for example
an input volume as a float should look like this: i_var_fltInputVolume. The name assigned to the parameter will also
be what it is referenced as in the corresponding Venus function, so should aim to be as self-explanatory as possible.
- Help files should be in the form of .chm files, created via any HTML help editor software. Microsoft HTML Help Workshop can be used,
though it is not easy to find. Other usable software include things such as :link:`https://helpbuilder.west-wind.com/ <WestWind Help Builder>`.