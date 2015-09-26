.

**Version 2.X is currently under development on GitHub (as part of my laurea thesis ( [VersionTwo](https://github.com/Darelbi/Infectorpp) )**

.

Infector++ is a lightweight IoC Container for doing dependency injection, it was designed to be **easy to use and hard to misuse**.

**Note for VisualStudio users**: due to a VisualStudio bug, "std::unique\_ptr" can't be used properly with Infectorpp. I hope they fix that soon, in the meanwhile, VS users could still inject quietly "std::shared\_ptr".

Thanks!:)

# Features: #
  * **Exception safety** ([STRONG GUARANTEE](http://en.wikipedia.org/wiki/Exception_safety))
  * **Virtually remove memory leaks**
  * Lightweight and simple
  * No compilation (few headers only)
  * Clean API
  * **Typesafe**
  * **Non-intrusive** (constructor injection without code generation)
  * Lazy instantiation (everything is created only when needed)
  * C++11 (requires [GCC 4.6 or greater](http://gameprog.it/articles/90/c-11-getting-started-on-windows#.U95T7aNBm7g), Clang 3.0 or greater, VisualStudio 2013 Update 4 CTP)
  * No macro obscure magic
  * No string processing

![http://i.imgur.com/kFfswdg.png](http://i.imgur.com/kFfswdg.png)


# Basic Tutorial #

Given the following class diagram (your existing code design):
![http://i.imgur.com/47CB7mn.png](http://i.imgur.com/47CB7mn.png)

You list your composition needs:
  * **BedRoom** implements _IRoom_
  * **ComfortableBed** implements _IBed_
  * **BedRoom** has 1 (and so depend on) _IBed_

Since **BedRoom** depends on a _IBed_ and inherits _IRoom_, the class will look like this
```
    class BedRoom: public virtual IRoom{
        std::unique_ptr<IBed> myBed; //unique pointer, there's a different bed for each room
    public:
        BedRoom( std::unique_ptr<IBed> bed )
            : myBed( std::move(bed) ) // take explicit ownership
            {  }
    };
```

In your `main()` you create your IoC Container,
```
    Infector::Container ioc;
```

then bind implementations to their interfaces,
```
    ioc.bindAs<BedRoom,        IRoom> ();  //bind BedRoom as IRoom
    ioc.bindAs<ComfortableBed, IBed>();    //bind ComfortableBed as IBed
```

and finally wire a constructor for concrete types
```
    ioc.wire<ComfortableBed    >(); //ComfortableBed constructor has no arguments
    ioc.wire<BedRoom,      IBed>(); //BedRoom takes 1 IBed as argument
```

# Istantiating objects #
Just simple as calling `build` on the IoC Container,
```
    auto myRoom = ioc.build<IRoom>();
```

The istantiated room is a **BedRoom**, it has a _IBed_ (the Comfortable one). The type "hided" by _auto_ is
```
    std::unique_ptr<IRoom> myRoom; 
```
and no call to "std::move" is necessary because move constructor in this particular case is automatically called.

# Advanced Tutorial #
[FurtherReadings](https://code.google.com/p/infectorpp/wiki/EaseOfUse)

# Many thanks to #
  * The creator of this lightweight [IoC container for Unity](https://github.com/sebas77/Lightweight-IoC-Container-for-Unity3D), in particular his articles about the topic are very usefull:
    1. [Lightweight IoC Container for Unity3D - part 1](http://blog.sebaslab.com/ioc-container-for-unity3d-part-1/)
    1. [Lightweight IoC Container for Unity3D - part 2](http://blog.sebaslab.com/ioc-container-for-unity3d-part-2/)
Infectorpp was heavily inspired by that Container.


---


