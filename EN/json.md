# JSON

### json.Marshal ()
Default conversion rule:  
* 1. Boolean is still Boolean after conversion to JSON, such as true-> true
* 2. Floating point and integer types are converted to regular numbers in JSON, such as 1.23-> 1.23
* 3. The string will be converted to UTF-8 encoding and output as a string of Unicode character set. Special characters such as <will be escaped as \ u003c
* 4. Arrays and slices are converted to arrays in JSON. The [] byte class is converted to a base64 encoded string, and the zero value of slice is converted to null.
* 5. The structure will be converted into a JSON object, and only the exportable fields that begin with a capital letter in the structure will be converted and output, and these exportable fields will be used as the string index of the JSON object.
* 6. When converting a map type data structure, the type of the data must be map [string] T (T can be any data type supported by the encoding / json package)


___


### json.Unmarshal ()
Note: If a field in the JSON does not exist in the Go target type, the json.Unmarshal () function will discard the field during decoding.  
Unknown type, follow the rules:
* 1.Boolean values ​​in JSON will be converted to bool in Go
* 2. The value will be converted to float64 in Go
* 3. String type is still string type
* 4.JSON array will be converted to [] interface {} type
* 5.JSON object will be converted to map [string] interface {}
* 6.null value will be converted to nil