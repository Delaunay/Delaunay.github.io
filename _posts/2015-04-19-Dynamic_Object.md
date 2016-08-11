---
layout: post
title:  "Dynamic Object Loading in C++"
date:   2015-08-15
categories: project cpp programming dll so shared library
---

I recently started to implement a plugin system for one of my applications.
I wanted to be able to hide most of the application's programming while enabling 
the application to run users' own code.

 Most of the examples I have seen were either simple tutorials or overly complicated
frameworks requiring a major change in my original design.

 This is what we are going to implement:

    -*- Code main.cpp

    #include <DynamicObject>

    int main()
    {
        DynamicObject<MyObjectBase> user_object("./libUserLib.so");

        DynamicObject<MyObjectBase> another_object = user_object.new_object();

        return user_object->function();
    }

As you can see DynamicObject manages the shared library loading, object creation/deletion 
and enables you to use DynamicObject as a simple pointer.

This is the minimal code the 'core' programmer needs to provide:

    -*- Code Base.h

    struct PLUGIN MyObjectBase
    {
        virtual ~MyObjectBase() = default;
        virtual PLUGIN int function() = 0;
    };

    // Prevent name mangling
    extern "C"{
        PLUGIN void* create_object();
        PLUGIN void destroy_object(void*);
    }

and this is the minimal code the plugin programmer needs to provide:

    -*- Code UserCode.cpp

    #include <DynamicObject/Plugin.h>
    #include "Base.h"


    struct MyObject : public Base
    {
        int function() {    return 1;   }
    }

    void* create_object()        { return new MyObject(); }
    void destroy_object(void* o) { delete (MyObject*) o;  }

PS: the plugin programmer doesn't have to include the "Plugin.h", nevertheless doing
 so will make sure that the appropriate measure has been taken so that the module can loaded flawlessly (see name mangling for more information).

He also does not have to inherit its specialized class from the base class but it is 
recommended as it allows compile-time checking of common errors.


# Implementation

SharedLibrary: load/unload a shared library is similar to a reference counting
pointer. The library is unloaded when the library is not referenced any more.

DynamicObject: Load a shared library and create the implemented object. It uses 
two functions "create_object" and "destroy_object" to allocate and deallocate 
the underlying object.

The default function's name can be modified, this is particularly
useful if the shared library implements multiple objects.

The DynamicObject can be created using an already existing SharedLibrary
object.

# Nota Bene

 If you are using references or pointers for an unresolved object you may need
 to specify PLUGIN_EXPORT before the function declaration.

    -*- Code
    class SpecialObject; // incomplete type

    struct MyObjectBase
    {
        PLUGIN_EXPORT virtual SpecialObject& function() = 0;
    }

    If you are interested in why you should read more about name mangling

# Compiler

You should make sure that the plugin and the core application have been compiled 
with the same compiler.

# [Source code][1]

### Change log
	
#### 26 jun 2016
* Replaced `DynamicObject` by `DynamicShared`
* Added `DynamicUnique`
* `DynanicShared` now uses `std::shared_ptr` with a custom deleter instead of my own reference counting pointer


[1]: https://github.com/Delaunay/DynamicLoading
