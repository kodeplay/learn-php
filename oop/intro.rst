Introduction to Object Oriented Programming in PHP
==================================================


What is OOP ?
-------------

Object oriented programming is a philosophy or an approach for
designing computer applications or programs. The core concept
of oop is use of complex data types to create objects. So 
your program during it's execution will have many objects
which are capable of communicating with each other by sending
and receiving messages there by acting on data. Objects help
each other to acheive the end goal.


Problems OOP strives to solve
-----------------------------

Here are some of the problems (in plain english!) that oop solves or rather
strives to solve

* Reduce duplicate code and improve reusability.

* Provide a custom data type for complex data.

* Mapping data to real world complex types eg. Bank Account

* Providing, maintaining and sharing state

* Give modularity to your application by providing container 
  for related data

* Better Code organization


OOP in PHP
----------

In PHP, which is originally a procedural language, OOP support was not
built in the language from start but was added later. Hence it doesn't
emulate true OO capabilities. Due to this, OOP approach in PHP feels
inferior to other programming languages where OOP support is baked
into the language right from the start eg. Java, Python.

This also means that when using OOP in PHP, it's important to know the 
limitations otherwise it will result in poor design decisions.

The best way to make up for the disadvantages and pain points of using 
OOP in PHP is to know it thoroughly and use it wisely.

Finally, it's easy which means it's easy to make do it the wrong way
too. Also, a lot of times OOP isn't the most elegant solution or 
often an overkill. So it's important to spot such cases early on.


It's all about objects!
-----------------------

They

* represent things (data) that your code cares about

* have Attributes

* can behave (via methods)


Classes
-------

Here is our first very basic class. It does nothing more than
providing a way to store data about pizzas and has methods that
represent how it might behave!

.. sourcecode:: php

    <?php

        class Pizza {
        
            public $name;

            public $toppings;

            public $price;

            public $size;

            public function bake() {
                // some clever logic of baking here
            }

            public function is_veg_or_nonveg() {
                $nonveg = array('chicken', 'mutton', 'beef', 'pork');
                foreach ($this->toppings as $topping) {
                    if (in_array($topping, $non_veg)) {
                        return 'non_veg';
                    }
                }
                return 'veg';
            }
        }

        $pizza = new Pizza();
        $pizza->name = 'Barbeque Chicken';
        $pizza->price = '200';
        $pizza->size = 'Medium';
        $pizza->toppings = array('chicken', 'capsicum', 'peppers');
        
        $pizza->bake(); // pizza is ready now!
        $pizza->is_veg_or_nonveg(); // non_veg
        

The ``class`` keyword is used to define a class. It represents an outline
as to how the object would be. The class by itself has no meaning because 
it's nothing but a template. From the definition above, we don't have 
any information about the pizza. What we do know is that a pizza will
have a name, price, size and toppings that go on it. 

``new`` keyword is used to create a new instance of the class. Any no.
of instances of the same class can be created but the class is only one.

The name, price, toppings and size are called ``instance attributes``
or ``instance properties``.  The functions defined inside the class a
known as ``instance methods``. In the pizza class we have two instance
methods. We use the term ``instance`` here as they are associated with an
instance of Pizza and not the class Pizza. Let us call these the 
``instance members`` for convenience.

An "arrow" ``->`` is used to access the members of an object. The
syntax to access the members is as follows

.. sourcecode:: php

    <?php

    $object->member;
    
    $object->member(); // to call a member function


How you refer to the object in the above syntax depends upon where you
are referencing it from. If you are referring to the object from
outside the class, it's pretty straight forward, just use the
reference to the object eg.

.. sourcecode:: php

    <?php

    $pizza->name;

    $pizza->bake();


But what if we want to refer to the object from inside one of the
instance method? During the method definition, there is no
object anywhere. In this case we can refer to the current instance of
the class using ``$this``. For eg. in the ``is_veg_or_nonveg``
function of the pizza class, the ``$this`` is used to access
the property `toppings`. This is the most important thing to understand
and here is a quick example

.. sourcecode:: php

    <?php

    $pizza1 = new Pizza();
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';
    $pizza1->price = '180';
    $pizza1->size = 'Medium';
    $pizza1->toppings = array('mushroom', 'capsicum', 'peppers');    

    $pizza1->is_veg_or_nonveg(); // veg
    // when this function is called, the `$this` inside the
    // is_veg_or_nonveg method refers to $pizza1 and thus 
    // $this->toppings refers to toppings that come with pizza1
    // which make pizza 1 a veg pizza

    $pizza2 = new Pizza();
    $pizza2->name = 'Chicken Delight Pizza2';
    $pizza2->price = '180';
    $pizza2->size = 'Medium';
    $pizza2->toppings = array('chicken', 'chilly', 'onions');    

    $pizza2->is_veg_or_nonveg(); // non_veg
    // when this function is called, the `$this` inside the
    // is_veg_or_nonveg method refers to $pizza2 and thus 
    // $this->toppings refers to toppings that come with pizza2
    // which make pizza 2 a non veg pizza    


Besides having instance properties and methods, there are some 
methods and properties which are associated with all instances
of the same class. These are called ``class properties`` and 
``class methods``

TODO eg.


Next up
-------

With this much basic knowlegde, we can now move to the `features of OOP`
where we will also see the meanings of terms such as ``public`` we we 
haven't discusses yet.

