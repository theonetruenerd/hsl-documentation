Predefined Constants
==============================

HSL has various predefined keywords which can be used within methods and libraries without
needing to define them. They will be associated with the values shown in the table below

=====  =====  =======
Keyword      Value      Meaning
=====  =====  =======
hslTrue  1  True
hslFalse   0  False
hslInfinite  n/a   Infinite time-out value
hslOKOnly   0   Display OK button only
hslOKCancel   1   Display OK and Cancel button
hslAbortRetryIgnore   2   Display Abort, Retry and Ignore button
hslYesNoCancel   3   Display Yes, No and Cancel button
hslYesNo   4   Display Yes and No button
hslRetryCancel   5   Display Retry and Cancel button
hslDefButton1   0   The first button is the default button. hslDefButton1 is the default unless hslDefButton2 or hslDefButton3 is specified
hslDefButton2   256   The second button is the default button
hslDefButton3   512   The third button is the default button
hslError   16   Display Error Message icon
hslQuestion   32   Display Warning Query icon
hslExclamation   48   Display Warning Message icon
hslInformation   64   Display Information Message icon
hslOK   1   OK button was selected
hslCancel   2   Cancel button was selected
hslAbort   3   Abort button was selected
hslRetry   4   Retry button was selected
hslIgnore   5   Ignore button was selected
hslYes   6   Yes button was selected
hslNo   7   No button was selected
hslInteger   "i"   The input value is an integer
hslFloat   "f"   The input value is a float
hslString   "s"   The input value is a string
hslRead   "r"   Opens the file for reading. The file must exist
hslWrite   "w"   Opens an empty file for writing. If the file already exists its contents are deleted
hslAppend   "a"   Opens the file for writing at the end of the file. If the file does not exist, a new file is created
hslHide   1   Hides the window and activates another window
hslShow   2   Activates the window and displays it in its current size and position
hslShowMaximized   3   Activates the window and displays it as a maximized window
hslShowMinimized   4   Activates the window and displays it as a minimized window
hslSynchronous   1   The execution of the running HSL program is blocked until the program to execute terminates
hslAsynchronous   2   The execution of the running HSL program will not be blocked until the program to execute terminates
hslCSVDelimited   ","   Fields in the file data source are delimited by commas (default)
hslTabDelimited   "\t"   Fields in the file data source are delimited by tabs
hslFixedLength   ""   Fields in the text file are of fixed width
n/a   "*"   Fields in the file data source are delimited by asterisks. The asterisk can be substituted for any character except the double quotation mark (")
hslAsciiText   "\n"   Fields in the file data source are delimited by newline characters
hslCurrent   0   Starts at the current row (default)
hslFirst   1   Starts at the first row
hslLast   2   Starts at the last row
=====  =====  =======