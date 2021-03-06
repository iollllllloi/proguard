Class files essentially define classes, their fields, and their methods. A lot
of essential and non-essential data are attached to these classes, fields, and
methods as *attributes*. For instance, attributes can contain bytecode, source
file names, line number tables, etc.

ProGuard's obfuscation step removes attributes that are generally not
necessary for executing the code. With the
[`-keepattributes`](usage.md#keepattributes) option, you can specify a filter
for attributes that you do want to keep, for instance if your code accesses
them through reflection, or if you want to preserve some compilation or
debugging information. The filter works like any [filter](usage.md#filters) in
ProGuard.

The following wildcards are supported:

| Wildcard | Meaning
|-----|----------------------------------------------------
| `?` | matches any single character in an attribute name.
| `*` | matches any part of an attribute name.

An attribute name that is preceded by an exclamation mark '**!**' is
*excluded* from further attempts to match with *subsequent* attribute names in
the filter. Make sure to specify filters correctly, since they are not checked
for potential typos.

For example, the following setting preserves the optional attributes that are
typically necessary when processing code that is intended to be used as a
library:

    -keepattributes Exceptions,InnerClasses,Signature,Deprecated,
                    SourceFile,LineNumberTable,*Annotation*,EnclosingMethod

The Java bytecode specifications currently specify the following list of
attributes.

## Optional attributes

ProGuard's obfuscation step by default discards the following optional
attributes. You can keep them with the
[`-keepattributes`](usage.md#keepattributes) option.

`SourceFile`
: Specifies the name of the source file from which the class file was
  compiled. If present, this name is reported in stack traces.

`SourceDir`<div>(J++ extension)</div>
: Specifies the name of the source directory from which the class file was
  compiled.

`Record` <div>(Java 14 or higher)</div>
: Specifies the components of a record class. Code may access this information
  by reflection.

`InnerClasses`
: Specifies the relationship between a class and its inner classes and outer
  classes. Other than this and the naming convention with a '\$' separator
  between the names of inner classes and outer classes, inner classes are just
  like ordinary classes. Compilers may need this information to find classes
  referenced in a compiled library. Code may access this information by
  reflection, for instance to derive the simple name of the class.

`PermittedSubclasses` <div>(Java 15 or higher)</div>
: Specifies the allowed extensions or implementations of sealed classes or
  interfaces.

`EnclosingMethod`<div>(Java 5 or higher)</div>
: Specifies the method in which the class was defined. Compilers may need this
  information to find classes referenced in a compiled library. Code may
  access this information by reflection, for instance to derive the simple
  name of the class.

`Deprecated`
: Indicates that the class, field, or method is deprecated.

`Synthetic`
: Indicates that the class, field, or method was generated by the compiler.

`Signature`<div>(Java 5 or higher)</div>
: Specifies the generic signature of the class, field, or method. Compilers
  may need this information to properly compile classes that use generic types
  from compiled libraries. Code may access this signature by reflection.

`MethodParameters`<div>(Java 8 or higher)</div>
: Specifies the names and access flags of the parameters of the method. Code
  may access this information by reflection.

`Exceptions`
: Specifies the exceptions that a method may throw. Compilers may use this
  information to enforce catching them.

`LineNumberTable`
: Specifies the line numbers of the method. If present, these line numbers are
  reported in stack traces.

`LocalVariableTable`
: Specifies the names and types of local variables of the method. If present,
  some IDEs may use this information for helping with auto-completion.

`LocalVariableTypeTable`<div>(Java 5 or higher)</div>
: Specifies the names and generic types of local variables of the method. If
  present, some IDEs may use this information for helping with
  auto-completion.

`RuntimeVisibleAnnotations`<div>(Java 5 or higher)</div>
: Specifies the annotations that are visible at run-time, for classes, fields,
  and methods. Compilers and annotation processors may use these annotations.
  Code may access them by reflection.

`RuntimeInvisibleAnnotations`<div>(Java 5 or higher)</div>

: Specifies the annotations that are visible at compile-time, for classes,
  fields, and methods. Compilers and annotation processors may use these
  annotations.

`RuntimeVisibleParameterAnnotations`<div>(Java 5 or higher)</div>

: Specifies the annotations that are visible at run-time, for method
  parameters. Compilers and annotation processors may use these annotations.
  Code may access them by reflection.

`RuntimeInvisibleParameterAnnotations`<div>(Java 5 or higher)</div>

: Specifies the annotations that are visible at compile-time, for method
  parameters. Compilers and annotation processors may use these annotations.

`RuntimeVisibleTypeAnnotations`<div>(Java 8 or higher)</div>
: Specifies the annotations that are visible at run-time, for generic types,
  instructions, etc. Compilers and annotation processors may use these
  annotations. Code may access them by reflection.

`RuntimeInvisibleTypeAnnotations`<div>(Java 8 or higher)</div>

: Specifies the annotations that are visible at compile-time, for generic
  types, instructions, etc. Compilers and annotation processors may use these
  annotations.

`AnnotationDefault`<div>(Java 5 or higher)</div>
: Specifies a default value for an annotation.

## Essential attributes

ProGuard automatically keeps the following essential attributes, processing
them as necessary. We're listing them for the sake of completeness:

`ConstantValue`
: Specifies a constant integer, float, class, string, etc.

`Code`
: Specifies the actual bytecode of a method.

`StackMap`<div>(Java Micro Edition)</div>
: Provides preverification information. The Java Virtual Machine can use this
  information to speed up the verification step when loading a class.

`StackMapTable`<div>(Java 6 or higher)</div>
: Provides preverification information. The Java Virtual Machine can use this
  information to speed up the verification step when loading a class.

`BootstrapMethods`<div>(Java 7 or higher)</div>
: Specifies the methods to bootstrap dynamic method invocations.

`Module`<div>(Java 9 or higher)</div>
: Specifies the dependencies of a _module_.

`ModuleMainClass`<div>(Java 9 or higher)</div>
: Specifies the main class of a _module_.

`ModulePackages`<div>(Java 9 or higher)</div>
: Specifies the packages of a _module_.

`NestHost`<div>(Java 11 or higher)</div>
: Specifies the host class of a _nest_, for example an outer class.

`NestMembers`<div>(Java 11 or higher)</div>
: Specifies the members of a _nest_, for example the inner classes.
