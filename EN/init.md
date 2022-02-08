# init()

### init() function feature

* init() before the main package executes
* init() executes automatically and cannot display the call
* multiple init() functions can be defined in the same Go file and executed sequentially
* different files in the same package, alphabetically and numerically sort file names by string, then call init() in each file in order.
* import:
    * character **_**(underscore) operation essentially introduces the package and doesn't use the inner function directly, just calls init() initialization
    * call init() in the package in import order
    * package init() is called automatically when it is referenced
    * the package has a dependency, and the call order is the last to be dependent and the first to be initialized
    * package is imported by multiple other packages and can only be initialized once