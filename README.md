The reason why 1d and 2d are working in your MVEL rule expression while 0d is causing an error lies in how MVEL interprets these numeric literals with the d suffix. Here's a breakdown of why this might be happening:

MVEL Numeric Literal Interpretation:
In MVEL, numeric literals with a d suffix (1d, 2d, etc.) are typically interpreted as double literals, similar to Java syntax.
When MVEL encounters 1d, it interprets it as a double value of 1.0.
Similarly, 2d is interpreted as 2.0.
Parsing Behavior:
MVEL's parsing behavior for numeric literals can vary based on the context and the specific version of MVEL you are using.
While 1d and 2d are interpreted as valid double literals without causing parsing errors, 0d is causing an issue likely due to MVEL treating it as an octal (base 8) number due to the 0 prefix.
Difference with 0d:
The specific issue with 0d could be related to how MVEL interprets the combination of 0 (which can be a valid octal prefix) followed by the d suffix.
The error message [Error: badly formatted number: For input string: "d" under radix 8] suggests that MVEL is trying to parse 0d as an octal number (0 followed by d under base 8), which is not valid and results in a parsing error.
