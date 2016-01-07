<a target="_blank" href="https://github.com/rttrorg/rttr/releases/latest">![Version][badge.version]</a> <a target="_blank" href="https://travis-ci.org/rttrorg/rttr">![Travis status][badge.Travis]</a> <a target="_blank" href="https://ci.appveyor.com/project/acki-m/rttr">![Appveyor status][badge.Appveyor]</a> <a target="_blank" href="https://coveralls.io/github/rttrorg/rttr">![Coverage Status][badge.Coveralls]</a> <a target="_blank" href="https://raw.githubusercontent.com/rttrorg/rttr/master/LICENSE.txt">![License][badge.license]</a>

RTTR
====
> C++ Reflection Library

RTTR stands for Run Time Type Reflection. 
It describes the ability of a computer program to introspect and modify an object at runtime. It is also the name of the library itself, which is written in C++ and released as open source library.
You can find more information on: <a target="_blank" href="http://www.rttr.org">www.rttr.org</a>

----------

How to Use
----------

####Manual registration
```cpp
#include <rttr/registration>
using namespace rttr;

struct MyStruct { MyStruct() {}; void func(double) {}; int data; };

RTTR_REGISTRATION
{
    registration::class_<MyStruct>("MyStruct")
         .constructor<>()
         .property("data", &MyStruct::data)
         .method("func", &MyStruct::func);
}
```
####Iterate over members
```cpp
type t = type::get<MyStruct>();
for (auto& prop : t.get_properties())
    std::cout << "name: " << prop.get_name();

for (auto& meth : t.get_methods())
    std::cout << "name: " << meth.get_name();
```

####Constructing types
```cpp
type t = type::get_by_name("MyStruct");
variant var = t.create();    // will invoke the previously registered ctor

constructor ctor = t.get_constructor();  // 2nd way with the constructor class
var = ctor.invoke();
std::cout << var.get_type().get_name(); // prints 'MyStruct'
```

####Set/get properties
```cpp
MyStruct obj;

property prop = type::get(obj).get_property("data");
prop.set_value(obj, 23);

variant var_prop = prop.get_value(obj);
std::cout << var_prop.to_int(); // prints '23'
```

####Invoke Methods:
```cpp
MyStruct obj;

method meth = type::get(obj).get_method("func");
meth.invoke(obj, 42.0);

variant var = type::get(obj).create();
meth.invoke(var, 42.0);
```

Features
---------
- reflect constructors, methods, data member or enums
- classes; with *single*-, *multiple*- and *virtual*-inheritance
- constructors (arbitrary argument count)
- methods (*virtual*, *abstract*, *overloaded*, arbitrary argument count)
- arrays (incl. raw-arrays; arbitrary dimension count)
- ability to invoke properties and methods of classes from any arbitrary class level
- no header pollution; the reflection information is created in the cpp file to minimize compile time when modifying the data
- working with custom types without the need of having the declaration of the type available at compile time (useful for plugins)
- possibility to add additional *metadata* to all reflection objects
- possibility to add *default arguments* to methods or constructors
- adjust registration behaviour through *policies*
- minimal macro usage
- **no** additional 3rd party dependencies are needed
- **no** rtti required; contains a faster and across shared libraries working replacement
- **no** exceptions (this feature come with cost and is also regularly disabled on consoles)
- **no** external compiler or tool needed, only standard ISO C++11

Portability
-----------
Tested and compiled with:
- Microsoft Visual Studio 2013 & 2015
- GCC 4.8.1
- MinGW 4.8.1

License
-------
RTTR is released under the terms of the [MIT](LICENSE.txt) license,
so it is free to use in your free or commercial projects.

Installation
------------
The installation guide can be found [here][rttr.install].


Get Started:
------------
Take a look at the [documentation][rttr.docs] or start with the [tutorial][rttr.tutorial].

<!-- Links -->
[badge.version]: https://badge.fury.io/gh/rttrorg%2Frttr.svg
[badge.Travis]: https://travis-ci.org/rttrorg/rttr.svg?branch=master
[badge.Appveyor]: https://ci.appveyor.com/api/projects/status/github/rttrorg/rttr?svg=true&branch=master
[badge.Coveralls]: https://coveralls.io/repos/rttrorg/rttr/badge.svg?branch=add-coveralls-ci&service=github
[badge.License]: https://img.shields.io/badge/license-MIT-blue.svg
[rttr.docs]: http://www.rttr.org/doc/rttr-0-9-5/classes.html
[rttr.install]: http://www.rttr.org/doc/rttr-0-9-5/building_install_page.html
[rttr.tutorial]: http://www.rttr.org/doc/rttr-0-9-5/tutorial_page.html
