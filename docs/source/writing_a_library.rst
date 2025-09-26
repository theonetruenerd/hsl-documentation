Writing a Library
===================================

HSL libraries contain functions written in HSL which can be accessed from within the Venus Graphical Method Editor.
These libraries can be used to provide complex functionality which would otherwise take up a lot of space in the method editor,
provide functionality available in HSL which is not available in Venus, or to allow integration with COM automation.

An HSL library consists of three main (groups of) files: the library file itself (.hsl), the help file(s) (.chm) and the
library icon (.bmp). Whilst the help files and icon are very helpful, they are technically optional.

HSL Library File
----------------

As mentioned in :ref:`fundamentals`, an HSL library is usually split into two halves; an empty declaration/implementation
and the actual function implementation. These can be split into two individual files, or technically the empty declaration
can be ignored, which is seen in some libraries, but here we will follow best practices and for the purpose of this documentation
we will aim to use this format.

The structure of an HSL library should begin with a commented out description of the file:

    .. code-block::

        //----------------------------------------------------------------------------------------
        //
        // Library Name:		MyFlankingNamespace::MyLibraryName
        // Description:			A description of my library
        // Author:			    tarunchapman
        // Date Created:		2025-09-26
        // Major ID:			My library major id in hexadecimal
        //
        // Library Version:		v1.0
        //
        // Changelog:
        //			v1.0	-	2025-09-26	-	Library Created
        //
        //----------------------------------------------------------------------------------------

As shown above, this should ideally contain the library name (including any flanking namespace, such as HSLExtensions or
TCExtensions, a description of the library, the name (and potentially contact details) of the author, the date the library
was created, the library major Id in hexadecimal (used for error codes; can be anything but try to begin somewhere higher than
200/0xC8 to avoid clashing with Hamilton-provided libraries. I will attempt to upload a list of all pre-existing major IDs
if possible), and a changelog with version numbers, dates, and descriptions of changes.

As these are comments, it is completely possible to write libraries which don't have these lines, however it is highly
recommended to have these for code legibility and debugging.

Libraries should then have a section which defines their unique identifier and checks whether it is already defined:

    .. code-block::

        #ifndef __MYLIBRARY_HSL__
        #define __MYLIBRARY_HSL__ 1

This first line checks whether the library has already been defined in the method it is being imported into, and only proceeds
if the library has not already been defined. This ensures libraries do not accidentally get imported twice. The second line
is what defines the library, which means any subsequent library imports will be skipped.

Next there should be a block for imports, allowing you to include other libraries, so that you can reference their functions
in your library.

    .. code-block::

        #ifndef __HSLMTHLIB_HSL__
            #include "HslMthLib.hsl"
        #endif

The first line checks whether the HslMthLib has already been defined in the method, and helps ensure it isn't imported twice.
To check what this should be, go into the .hsl file of the library which is being imported, and copy the word after the
"#define" keyword near the top of the library. This is usually just two underscores, the library name in full caps, an underscore,
the letters hsl, and then two more underscores, but this is not always the case so it is worth checking. The second, indented
line is what imports the library. It should be the word "#include" followed by the path to the .hsl file for the library
relative to the "Libraries" folder in the Hamilton directory - for most default libraries this will be their name, but if
a library is in a subfolder then the folder path is needed (e.g. TraceLevel is usually "ASWGlobal\\TraceLevel\\TraceLevel.hsl".
The "#endif" line just ends the opened "#ifndef" statement. This block should be repeated for each different library you wish
to import.

The next part of the library should be the empty implementation. Firstly, you need to wrap it in your namespace. If you
are making a group of related libraries, it is often useful to add a flanking namespace (e.g. HSLExtensions), followed by
your library namespace. If it is an isolated (and uniquely named) library then this isn't necessary.

    .. code-block::

        #ifndef HSL_RUNTIME
        namespace MyFlankingNamespace {
            namespace MyLibraryNamespace {

The first line is checking if HSL_RUNTIME is defined. This is a parser constant that is always defined at runtime but not
at edit time, and thus is what speeds up checking library syntax.
The library namespace is usually just the name of your library.

Within your library namespace, it is usually good practice to have a namespace for raising errors, within which should be
the error namespaces and a "raise error" function.

    .. code-block::

        namespace Error {

            static function RaiseRuntimeError(
                        variable majorID,
                        variable minorID,
                        variable specificID,
                        string errorDescription,
                        string functionName,
                        variable lineNumber) void
                    {
                        return;
                    }
            }
        }

This block is where we will store our error IDs and handle raising runtime errors in our library.

The next section should be where you define the implementation of your functions. Lets say you want to make a function
which takes a number and squares it, you have two main ways of doing this - you can either have an input parameter and an
output parameter (the number to be squared and the squared number), or just an input and have the return value of the function
be the squared number. There are pros and cons of each way of doing things; if you want to manipulate multiple data pieces
at once then it makes sense to have output parameters; if you want to modify the inputs without having to assign new variables
then having io variables makes sense, but often returning the value is the best way of doing things (as is the case in libraries
such as HslStrLib).

    .. code-block::

        function SquareNumber(variable i_var_intNumberToSquare) variable {return(0);}
        function SquareNumberAlternative(variable i_var_intNumberToSquare, variable& o_var_intSquaredNumber) void {return;}

In this block, we have defined the empty implementations for both ways of implementing this function. The first expects
a variable return value which will be the squared number; the second uses an output parameter. The "&" symbol after "variable"
is what tells the function it is allowed to modify the variable outside of itself, and is required for any function which
updates or outputs a parameter. The "variable" and "void" tags between the ")" and "{" are used to show what return type the
function is expecting.

If we wanted a function to be accessible for other functions within the library but not to be visible within the Graphical
Method Editor, we would give it the private scope, like so:

    .. code-block::

        private function MySupportingFunction() void {return;}

Once we have defined all our empty function implementations, we would then close that section of the library:

    .. code-block::

            } // end of library namespace
        } // end of flanking namespace
        #endif // End of "#ifndef HSL_RUNTIME

Next we have the block of our library which is used during actual runtime:

    .. code-block::

        #ifdef HSL_RUNTIME

        namespace MyFlankingNamespace {
            namespace MyLibraryNamespace {

Firstly, we would add our actual error namespace, with the error ids and raise error functions

    .. code-block::

        namespace Error {

            static const variable MajorID       (0xC8); // This should be the major ID of the library

We also need to define our minor and specific error ids. Minor error ids correspond with the individual function in our
library, and specific error ids correspond with the actual error occurring. This should all be within the Error namespace

    .. code-block::

        namespace MinorIDs {
            static const variable UnspecifiedFunctionId         (0x00)  // Always good to keep 0x00 free for unspecified errors
            static const variable SquareNumberId                (0x01)  // Repeat this for each function (private or public) in your library
            static const variable SquareNumberAlternativeId     (0x02)
        }

        namespace SpecificIDs {
            static const variable UnspecifiedErrorId    (0x00)  // Again, keep 0x00 free for unspecified errors
            static const variable InputNotInt            (0x01)  // Do this for each error you are throwing. Follow the error with a commented out line explaining what the error is, for easier debugging. In this case, description would be "input not an integer"
        }

Then you want to define your RaiseRuntimeError function:

    .. code-block::

        // --------------------------
        // Function: RaiseRuntimeError
        // Scope: Static
        // Description: Handles the generation of error codes and descriptions in the trace
        // Parameters:
        //	[i] majorID	-	The major error ID
        //	[i] minorID	-	The minor error ID
        //	[i] specificID	-	The specific error ID
        //	[i] errorDescription	-	The error description
        //	[i] functionName	-	The name of the function that raised the error
        //	[i] lineNumber	-	The line number of the function that raised the error
        // Returns: Void
        // --------------------------
        static function RaiseRuntimeError(
            variable majorID,
            variable minorID,
            variable specificID,
            string errorDescription,
            string functionName,
            variable lineNumber) void
        {
            // Defining function variables
            variable HxResult;
            variable description;

            // Generating error code
            HxResult = MthShiftLeft(minorID & 0x1F, 24) | MthShiftLeft(majorID & 0xFF, 16) | (specificID & 0xFFFF);
            // Defining error description;
            description = "MyLibrary.hsl ("+lineNumber+") : " + functionName + "() : " + errorDescription;
            err.SetDescription(description);
            // Raising error
            err.Raise(HxResult, err.GetDescription());
            // Returning void
            return;
        }

Whenever we wish to throw an error in any of our library functions, we will call this. This will raise an error in the
default way that Venus does, so integrates well with other systems interacting with "normal" runtime errors. It also allows
for consistency between libraries for error raising and makes things easier to debug. This function, when called, will abort
the method and throw an error in the trace, with the description including the library, the line number of the library file,
the major, minor and specific error IDs as defined in our namespaces, and our description of the error.

You will also see the documentation comment above the function; all functions should be documented this way, with their name,
scope, a description, a list of parameters, whether those parameters are [i], [o], or [io] parameters, a description of each
one, and what the function returns.

Once we have defined our error function, we can close the error namespace and move onto our actual functions:

    .. code-block::

        } // End of error namespace

        // Private functions

        // --------------------------
        // Function: MySupportingFunction
        // Scope: Private
        // Description: A supporting function not visible in the method editor
        // Parameters: None
        // Returns: Void
        // --------------------------
        private function MySupportingFunction() void
        {
            // We would put the code for our supporting function here
            return;
        }

Then we want to add our public functions (accessible in the method editor)

    .. code-block::

        // Public functions

        // --------------------------
        // Function: SquareNumber
        // Scope: Public
        // Description: Returns the square of the input number
        // Parameters:
        //	[i] i_var_intInputNumber    -   The number to be squared
        // Returns: Variable. The squared number
        // --------------------------
        function SquareNumber(variable i_var_intInputNumber) variable
        {
            variable outputNumber; // Before using any variable we need to define it.
            variable typeCheck; // Any variables declared in functions are local and not accessible outside the function

            // Lets say we want our function to only handle integers
            typeCheck = HSLUtilLib::IsInteger(i_var_intInputNumber)
            if (typeCheck == hslFalse)
            {
                // Here we call our RaiseRuntimeError function
                Error::RaiseRuntimeError(Error::MajorID,Error::MinorIDs::SquareNumberID,Error::SpecificIDs::InputNotInt,"Input not an integer","SquareNumber",GetLineNumber(););
            }

            outputNumber = i_var_intInputNumber * i_var_intInputNumber;

            return (outputNumber);
        }

        // --------------------------
        // Function: SquareNumberAlternative
        // Scope: Public
        // Description: Outputs the square of the input number
        // Parameters:
        //	[i] i_var_intInputNumber    -   The number to be squared
        //  [o] o_var_intSquaredNumber  -   The squared number
        // Returns: Void
        // --------------------------
        function SquareNumber(variable i_var_intInputNumber, variable& o_var_intSquaredNumber) void
        {
            variable typeCheck;

            // Lets say we want our function to only handle integers
            typeCheck = HSLUtilLib::IsInteger(i_var_intInputNumber)
            if (typeCheck == hslFalse)
            {
                // We can reuse the specific error id as it is the same cause, though often you will not be able to do this.
                // The minor error id corresponds to the specific function.
                Error::RaiseRuntimeError(Error::MajorID,Error::MinorIDs::SquareNumberAlternativeID,Error::SpecificIDs::InputNotInt,"Input not an integer","SquareNumberAlternative",GetLineNumber(););
            }

            o_var_intSquaredNumber = i_var_intInputNumber * i_var_intInputNumber;
            return;
        }

Assuming these are all the functions we need, we can now close off our namespaces and add our "#endif"s

    .. code-block::

            } // End of MyLibraryNamespace
        } // End of MyFlankingNamespace
        #endif // End of HSL_RUNTIME
        #endif // End of __MYLIBRARY_HSL__

And there you go! Your first library! This just goes through the basic structure. Continue to browse the documentation for
more detail on specific functions, or look at how to create help files and icons for your functions.