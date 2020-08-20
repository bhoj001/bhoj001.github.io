---
layout: post
title: "Design pattern in python- part2 Creational Pattern"
description: "Design pattern - creational pattern"
# tags: [sample post, link post]
# link: http://mademistakes.com  
published: true
authors: 'Bhoj Bahadur Karki'
name: 'Bhoj Bahadur Karki'
share: true
---

## Creational Pattern
Lets look at some of the examples of crational pattern in this article. 

### Singleton desgin pattern:
This pattern restricts the creation of instance to one instance. This can be verified via following example:

{% highlight python %}

    class Singleton:
        __instance = None

        @staticmethod
        def get_instance():
            if Singleton.__instance is None:
                Singleton()
            return Singleton.__instance

        def __init__(self):
            # print(self)
            # print("type = ", type(self))
            if Singleton.__instance is not None:
                raise Exception("Singleton class !Can't create multiple objects!")

            else:
                Singleton.__instance = self


    s = Singleton()
    print(s)

    s = Singleton.get_instance()
    print(s)

    s = Singleton.get_instance()
    print(s)

    s = Singleton.get_instance()
    print(s)

{% endhighlight %}
Output:

{% highlight shell %}
<__main__.Singleton object at 0x7f78d1920c50>
<__main__.Singleton object at 0x7f78d1920c50>
<__main__.Singleton object at 0x7f78d1920c50>
<__main__.Singleton object at 0x7f78d1920c50>
{% endhighlight %}

### Builder design pattern:
Builder design pattern helps to create complex from simple object by separating construction and representation. Following example illustrates builder design pattern using python:

{% highlight python %}

    class Builder:
        """
        This is like an abstract class
        """

        def get_wheel(self): pass

        def get_body(self): pass

        def get_engine(self): pass


    class JeepBuilder(Builder):

        def get_wheel(self):
            wheel = Wheel()
            wheel.size = 34
            return wheel

        def get_engine(self):
            eng = Engine()
            eng.horsepower = 770
            return eng

        def get_body(self):
            body = Body()
            body.shape = "Range Rover"
            return body


    class Director:
        __builder = None

        def __init__(self, b):
            self.__builder = b

        def get_car(self):
            car = Car()

            car.set_body(self.__builder.get_body())
            # car.set_wheel(self.__builder.get_wheel())
            car.set_engine(self.__builder.get_engine())

            # set wheel
            i = 0
            while i < 4:
                wheel = self.__builder.get_wheel()
                car.set_wheel(wheel)
                i += 1

            return car


    # This one is our main product
    class Car:
        _body = None
        _engine = None
        _wheels = list()

        def set_body(self, body):
            self._body = body

        def set_engine(self, eng):
            self._engine = eng

        def set_wheel(self, wh):
            self._wheels.append(wh)

        def get_specification(self):
            print(f"body shape = {self._body.shape}")
            print(f"engine horsepower= {self._engine.horsepower}")
            print(f"wheel size = {self._wheels[0].size}")


    class Wheel:
        size = None


    class Body:
        shape = None  # e.g SUV


    class Engine:
        horsepower = None


    if __name__ == "__main__":
        print("jeep")
        builder = JeepBuilder()
        director = Director(builder)

        jeep = director.get_car()  # return an instance
        jeep.get_specification()
        print("thanks")

{% endhighlight %}

Output:

{% highlight shell %}
    jeep
    body shape = Range Rover
    engine horsepower= 770
    wheel size = 34
    thanks
{% endhighlight %}

### Factory design patter:
This pattern helps to create object without giving underlying details to client and communicating to those created object via common interfact. 
Following example illustrates factory design pattern:

{% highlight python %}
    class Button(object):
        html = "<button></button>"

        def get_html(self):
            return self.html


    class Image(Button):
        html = "<img></img>"


    class Input(Button):
        html = "<input></input>"


    class Card(Button):
        html = "<obj></obj>"


    class ButtonFactory:

        def create_button(self, typ):
            # Note we need to capitalize() because we are passing values in lower case
            return globals()[typ.capitalize()]()


    btn = ButtonFactory()
    input_list = ['image', 'input', 'card']
    for b in input_list:
        t = btn.create_button(b).get_html()
        print(t)
{% endhighlight %}

Output:

{% highlight python %}
    <img></img>
    <input></input>
    <obj></obj>
{% endhighlight %}


### Prototype design pattern:
Prototype design pattern helps to hide the complexity of the instances created by the class via cloning an object. Following example show prototype desing pattern:

{% highlight python %}

    import copy


    class Prototype:
        _type = None
        _value = None

        def get_type(self):
            return self._type

        def get_value(self):
            return self._value

        def clone(self):
            pass


    class Type1(Prototype):
        def __init__(self, value):
            self._type = "type1"
            self._value = value

        def clone(self):
            return copy.copy(self)


    class Type2(Prototype):
        def __init__(self, value):
            self._type = "type2"
            self._value = value

        def clone(self):
            return copy.copy(self)


    class ObjectFactory:
        __type1value1 = None
        __type1value2 = None
        __type2value1 = None
        __type2value2 = None

        @staticmethod
        def initialize():
            ObjectFactory.__type1value1 = Type1(1)
            ObjectFactory.__type1value2 = Type1(2)
            ObjectFactory.__type2value1 = Type2(1)
            ObjectFactory.__type2value2 = Type2(2)

        @staticmethod
        def get_type1_value1():
            return ObjectFactory.__type1value1.clone()

        @staticmethod
        def get_type1_value2():
            return ObjectFactory.__type1value2.clone()

        @staticmethod
        def get_type2_value1():
            return ObjectFactory.__type2value1.clone()

        @staticmethod
        def get_type2_value2():
            return ObjectFactory.__type2value2.clone()


    def main():
        print("starting main")

        # initialize and call method
        ObjectFactory.initialize()

        obj = ObjectFactory.get_type1_value1()
        print(f"type= {obj.get_type()}, value = {obj.get_value()}")

        obj = ObjectFactory.get_type1_value2()
        print(f"type= {obj.get_type()}, value = {obj.get_value()}")

        obj = ObjectFactory.get_type2_value1()
        print(f"type= {obj.get_type()}, value = {obj.get_value()}")

        obj = ObjectFactory.get_type2_value2()
        print(f"type= {obj.get_type()}, value = {obj.get_value()}")

        print("thanks")


    if __name__ == "__main__":
        main()


{% endhighlight %}

{% highlight bash %}
starting main
type= type1, value = 1
type= type1, value = 2
type= type2, value = 1
type= type2, value = 2
thanks
{% endhighlight  %}

In the next lession we will look at some structural design pattern.