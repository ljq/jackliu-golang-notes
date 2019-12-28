# fmt.Printf () format placeholder

### Format character ```fmt.Printf ("% v ", variable)` ``

```
# [General placeholder]
    v The default format for values.
    % + v add field name (such as structure)
    % # v Go syntax representation of the corresponding value
    % T Go syntax representation of the type of the corresponding value
    %% literally a percent sign, not a placeholder for a value


# 【Boolean value】
    % t true or false


# [Integer value]
    % b binary representation
    % c the character represented by the corresponding Unicode code point
    % d decimal representation
    % o octal representation
    % q single-quoted character literal, safely escaped by Go syntax
    % x in hexadecimal notation a-f
    % X Hexadecimal representation, uppercase A-F
    % U Unicode format: U + 1234, equivalent to "U +% 04X"


# [Floating point and complex numbers]
    % b Scientific notation with no fractional part and exponent power of two,
       Consistent with the 'b' conversion format in strconv.FormatFloat. E.g. -123456p-78
    % e scientific notation, for example -1234.456e + 78
    % E scientific notation, for example -1234.456E + 78
    % f has a decimal point without an exponent, such as 123.456
    % g selects% e or% f as appropriate to produce a more compact (no trailing 0) output
    % G selects% E or% f as appropriate to produce a more compact (no trailing 0) output


# [Slice representation of strings and bytes]
    % s String or slice uninterpreted byte
    % q double-quoted string, safely escaped by Go syntax
    % x hex, lowercase, two characters per byte
    % X hex, uppercase, two characters per byte


# [Pointer]
    % p hex representation, prefix 0x
    There is no 'u' tag here. If integers are of unsigned type, they will be printed as unsigned.


# [Precision and alignment]
    [Minimum output width] [. Accuracy] [length] type
    "% -md": Left-justified, if m is less than the actual, it is output as actual.
    "% m.ns": output m bits, take n characters from the string (from the left), pad with spaces to the left, m = n when n> m or m is omitted
    "% m.nf": output width alignment of output floating point numbers, where m is the width and n is the right digit of the decimal point


# [Width and accuracy]
    The indicator (`f ') is an example:
    % f: default width and precision
    % mf width m, default accuracy
    % .nf default width, precision n
    % m.nf width m, precision n
    % m.f width m, precision 0
    If a floating-point indicator (`f ') is encountered, it represents the number of digits in the decimal part.
    If a floating-point indicator (`e ',` E', `g ',` G') is encountered, it indicates the number of significant digits
    If the precision is set to '*', the precision value will be extracted from the parameter
    Integer indicators (`d ',` i', `b ',` o', `x ',` X', `u ')
    For string% s or floating point type% f, the precision can truncate the length of the data
    "% -md": Left-justified, if m is less than the actual, it is output as actual.
    "% m.ns": output m bits, take n characters from the string (from the left), pad with spaces to the left, and m = n when n> m or m is omitted

# [Display parameter placeholder]
    % [2] d,% [1] d Take% d as an example: first output the second value, and then output the first value.


# [Special format identifier]
    + The sign of the total printed value; for% q (% + q), only ASCII-encoded characters are guaranteed to be output.
    -Left justified
    # Alternate format:
              Add leading 0 (% # o) to octal,
                  Add leading 0x (% # x) or 0X (% # X) to hexadecimal, remove leading 0x for% p (% # p);
              For% q, if strconv.CanBackquote returns true,
                  Will print the original (i.e. backquoted) string;
              If it is a printable character,% U (% # U) will write the character
                  Unicode encoding (such as the character x will be printed as U + 0078 'x').
    '' (Space) to leave blank (% d) for the omitted sign
          When printing strings or slices in hexadecimal (% x,% X), separate spaces between bytes
    0 pads leading zeros instead of spaces; for numbers, this moves padding after the sign

```