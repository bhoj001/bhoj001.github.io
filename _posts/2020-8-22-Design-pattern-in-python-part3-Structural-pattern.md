---
layout: post
title: "Design pattern in python- part3 Structural Pattern"
description: "Design pattern - structural pattern"
tags: [python, design-pattern]
# link: http://mademistakes.com  
published: true
authors: 'Bhoj Bahadur Karki'
name: 'Bhoj Bahadur Karki'
share: true
---

## Structural Pattern
Structural desing pattern is concerned with class and object composition. They use inheritance to compose interfaces and declare ways to compose object to get the desired new functionality. Lets look at some of the examples of Structural pattern in this article. 

### Adaptor desgin pattern:
This pattern allows incompatible interfaces to work together by wrapping the interfaces around an already existing class. This can be verified via following example:

{% highlight python %}
    class EuropeanSocketInterface:
        def voltage(self): pass

        def live(self): pass

        def neutral(self): pass

        def earth(self): pass


    # adaptee

    class Socket(EuropeanSocketInterface):
        def voltage(self):
            return 230

        def live(self):
            return 1

        def neutral(self):
            return -1

        def earth(self):
            return 0

    class USASocketInterface:
        def voltage(self): pass

        def live(self): pass

        def neutral(self): pass

    # this is the adaptor pattern

    class Adaptor(USASocketInterface):
        def __init__(self, interface):
            self.__interface = interface

        def voltage(self):
            return 110

        def live(self):
            return self.__interface.live()

        def neutral(self):
            return self.__interface.neutral()

    # client
    class ClientElectricKettle:
        def __init__(self, obj):
            self.__power = obj

        def boil(self):
            if self.__power.voltage() > 110:
                print("Too hot")
            elif self.__power.live() == 1 and self.__power.neutral() == -1:
                print("Coffee time")
            else:
                print("No power")


    if __name__ == "__main__":

        socket = Socket()
        adaptor = Adaptor(socket)

        client = ClientElectricKettle(adaptor)
        client.boil()

        print("end")


{% endhighlight %}
Output:

{% highlight shell %}
    Coffee time
    end
{% endhighlight %}


### Facade desgin pattern:
This pattern provides simplified interfaces to a large body of interfaces.Let's look at an example below:

{% highlight python %}
    class _IgnitionSystem(object):

        @staticmethod
        def produce_spark():
            return True


    class _Engine(object):

        def __init__(self):
            self.revs_per_minute = 0

        def turnon(self):
            self.revs_per_minute = 2000

        def turnoff(self):
            self.revs_per_minute = 0


    class _FuelTank(object):

        def __init__(self, level=30):
            self._level = level

        @property
        def level(self):
            return self._level

        @level.setter
        def level(self, level):
            self._level = level


    class _DashBoardLight(object):

        def __init__(self, is_on=False):
            self._is_on = is_on

        def __str__(self):
            return self.__class__.__name__

        @property
        def is_on(self):
            return self._is_on

        @is_on.setter
        def is_on(self, status):
            self._is_on = status

        def status_check(self):
            if self._is_on:
                print("{}: ON".format(str(self)))
            else:
                print("{}: OFF".format(str(self)))


    class _HandBrakeLight(_DashBoardLight):
        pass


    class _FogLampLight(_DashBoardLight):
        pass


    class _Dashboard(object):

        def __init__(self):
            self.lights = {"handbreak": _HandBrakeLight(), "fog": _FogLampLight()}

        def show(self):
            for light in self.lights.values():
                light.status_check()


    # Facade
    class Car(object):

        def __init__(self):
            self.ignition_system = _IgnitionSystem()
            self.engine = _Engine()
            self.fuel_tank = _FuelTank()
            self.dashboard = _Dashboard()

        @property
        def km_per_litre(self):
            return 17.0

        def consume_fuel(self, km):
            litres = min(self.fuel_tank.level, km / self.km_per_litre)
            self.fuel_tank.level -= litres

        def start(self):
            print("\nStarting...")
            self.dashboard.show()
            if self.ignition_system.produce_spark():
                self.engine.turnon()
            else:
                print("Can't start. Faulty ignition system")

        def has_enough_fuel(self, km, km_per_litre):
            litres_needed = km / km_per_litre
            if self.fuel_tank.level > litres_needed:
                return True
            else:
                return False

        def drive(self, km=100):
            print("\n")
            if self.engine.revs_per_minute > 0:
                while self.has_enough_fuel(km, self.km_per_litre):
                    self.consume_fuel(km)
                    print("Drove {}km".format(km))
                    print("{:.2f}l of fuel still left".format(self.fuel_tank.level))
            else:
                print("Can't drive. The Engine is turned off!")

        def park(self):
            print("\nParking...")
            self.dashboard.lights["handbreak"].is_on = True
            self.dashboard.show()
            self.engine.turnoff()

        def switch_fog_lights(self, status):
            print("\nSwitching {} fog lights...".format(status))
            boolean = True if status == "ON" else False
            self.dashboard.lights["fog"].is_on = boolean
            self.dashboard.show()

        def fill_up_tank(self):
            print("\nFuel tank filled up!")
            self.fuel_tank.level = 100


    # the main function is the Client
    def main():
        car = Car()
        car.start()
        car.drive()
        car.switch_fog_lights("ON")
        car.switch_fog_lights("OFF")
        car.park()

        car.fill_up_tank()
        car.drive()
        car.start()
        car.drive()

    if __name__ == "__main__":
        main()

{% endhighlight %}
Output:

{% highlight shell %}
    Starting...
    _HandBrakeLight: OFF
    _FogLampLight: OFF


    Drove 100km
    24.12l of fuel still left
    Drove 100km
    18.24l of fuel still left
    Drove 100km
    12.35l of fuel still left
    Drove 100km
    6.47l of fuel still left
    Drove 100km
    0.59l of fuel still left

    Switching ON fog lights...
    _HandBrakeLight: OFF
    _FogLampLight: ON

    Switching OFF fog lights...
    _HandBrakeLight: OFF
    _FogLampLight: OFF

    Parking...
    _HandBrakeLight: ON
    _FogLampLight: OFF

    Fuel tank filled up!


    Can't drive. The Engine is turned off!

    Starting...
    _HandBrakeLight: ON
    _FogLampLight: OFF


    Drove 100km
    94.12l of fuel still left
    Drove 100km
    88.24l of fuel still left
    Drove 100km
    82.35l of fuel still left
    Drove 100km
    76.47l of fuel still left
    Drove 100km
    70.59l of fuel still left
    Drove 100km
    64.71l of fuel still left
    Drove 100km
    58.82l of fuel still left
    Drove 100km
    52.94l of fuel still left
    Drove 100km
    47.06l of fuel still left
    Drove 100km
    41.18l of fuel still left
    Drove 100km
    35.29l of fuel still left
    Drove 100km
    29.41l of fuel still left
    Drove 100km
    23.53l of fuel still left
    Drove 100km
    17.65l of fuel still left
    Drove 100km
    11.76l of fuel still left
    Drove 100km
    5.88l of fuel still left
    Drove 100km
    0.00l of fuel still left

{% endhighlight %}


### Decorator desgin pattern:
This pattern dynamically adds/overrides existing method to add new functionality without changing it's calling style. Let's look at an example below. In this example we have two files 
1. decorator.py
2. implement.py
   
The decorator.py file has the decorator design pattern code and the implement.py file uses decorator.py method to get Cost, Ingredient, Sales tax for various items.
<br/>
<br/>
Code in decorator.py
{% highlight python %}
    import six
    from abc import ABCMeta


    @six.add_metaclass(ABCMeta)
    class AbstractCoffee:
        def get_ingridents(self):
            pass

        def get_price(self):
            pass

        def get_taxes(self):
            return 0.1 * self.get_price()


    class ConcreteCoffee(AbstractCoffee):
        def get_ingridents(self):
            return 'coffee'

        def get_price(self):
            return 100


    @six.add_metaclass(ABCMeta)
    class AbstractCoffeeDecorator(AbstractCoffee):
        def __init__(self, decorated_coffee):
            self.decorated_coffee = decorated_coffee

        def get_ingridents(self):
            return self.decorated_coffee.get_ingridents()

        def get_price(self):
            return self.decorated_coffee.get_price()


    class Suger(AbstractCoffeeDecorator):
        def get_ingridents(self):
            return self.decorated_coffee.get_ingridents() + " ,suger"

        def get_price(self):
            return self.decorated_coffee.get_price() + 25


    class Milk(AbstractCoffeeDecorator):
        def get_ingridents(self):
            return self.decorated_coffee.get_ingridents() + " ,milk"

        def get_price(self):
            return self.decorated_coffee.get_price() + 75


    class Vanilla(AbstractCoffeeDecorator):
        def get_ingridents(self):
            return self.decorated_coffee.get_ingridents() + " ,vanilla"

        def get_price(self):
            return self.decorated_coffee.get_price() + 200


{% endhighlight %}


Code in implement.py
{% highlight python %}
    import decorator

    my_coffee = decorator.ConcreteCoffee()
    print(
        'Ingredients :', my_coffee.get_ingridents(),
        ', Cost :', my_coffee.get_price(),
        ', Sales Tax :',my_coffee.get_taxes()
    )

    my_coffee = decorator.Milk(my_coffee)
    print(
        'Ingredients :', my_coffee.get_ingridents(),
        ', Cost :', my_coffee.get_price(),
        ', Sales Tax :',my_coffee.get_taxes()
    )
    my_coffee = decorator.Vanilla(my_coffee)
    print(
        'Ingredients :', my_coffee.get_ingridents(),
        ', Cost :', my_coffee.get_price(),
        ', Sales Tax :',my_coffee.get_taxes()
    )
    my_coffee = decorator.Suger(my_coffee)
    print(
        'Ingredients :', my_coffee.get_ingridents(),
        ', Cost :', my_coffee.get_price(),
        ', Sales Tax :',my_coffee.get_taxes()
    )

{% endhighlight %}

Now, let's run implement.py and see the output
Output:

{% highlight shell %}
    Ingredients : coffee , Cost : 100 , Sales Tax : 10.0
    Ingredients : coffee ,milk , Cost : 175 , Sales Tax : 17.5
    Ingredients : coffee ,milk ,vanilla , Cost : 375 , Sales Tax : 37.5
    Ingredients : coffee ,milk ,vanilla ,suger , Cost : 400 , Sales Tax : 40.0
{% endhighlight %}


### Proxy desgin pattern:
This pattern provides a placeholder for another object to control access, reduce cost, and reduce complexity. Let's look at an example below.

{% highlight python %}
    class Image:
        def __init__(self, filename):
            self._filename = filename

        def load_image(self):
            print('Loading...', self._filename)

        def display_image(self):
            print('display...', self._filename)


    class Proxy:
        def __init__(self, subject):
            self._subject = subject
            self._proxystate = None


    class ImageProxy(Proxy):

        def display_image(self):
            if self._proxystate is None:
                self._subject.load_image()
                self._proxystate = 1
            print("Displaying ...",self._subject._filename)


    img1 = ImageProxy(Image("IMAGE_SUB_1"))
    img2 = ImageProxy(Image("IMAGE_SUB_2"))

    img1.display_image()
    img1.display_image()

    img2.display_image()
    img2.display_image()

    img1.display_image()
    img1.display_image()


{% endhighlight %}
Output:

{% highlight shell %}
    Loading... IMAGE_SUB_1
    Displaying ... IMAGE_SUB_1
    Displaying ... IMAGE_SUB_1
    Loading... IMAGE_SUB_2
    Displaying ... IMAGE_SUB_2
    Displaying ... IMAGE_SUB_2
    Displaying ... IMAGE_SUB_1
    Displaying ... IMAGE_SUB_1
{% endhighlight %}


In the next lession we will look at some behaviour design patterns.