Basics of Classes and Objects
=============================

Class
-----

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


Creating objects or instances
-----------------------------

``new`` keyword is used to create a new instance of the class. Any no.
of instances of the same class can be created but the class is only one.


Instance methods and properties
-------------------------------

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


Class Methods and Properties
----------------------------

Besides having instance properties and methods, there are some 
methods and properties which are associated with all instances
of the same class. These are called ``class properties`` and 
``class methods``. These are not associated with any single
instance and can be accessed by any part of the code even if 
there is no instance of the class created.

In our pizza example, the set of nonveg toppings we are using to
decide if the pizza is veg or nonveg will remain the same for 
all instances of pizza objects. Similarly there can be a set of
veg toppings. We can define these as a class property as follows

.. sourcecode:: php

    <?php

    class Pizza {
    
        // ...        

        public static $TOPPING_SETS = array(
            'veg' => array('tomato', 'capsicum', 'paneer', 'onions', 'olives'),
            'nonveg' => array('chicken', 'mutton', 'beef', 'pork'),
        );

        public function is_veg_or_nonveg() {
            foreach ($this->toppings as $topping) {
                if (in_array($topping, self::$TOPPING_SETS['non_veg'])) {
                    return 'non_veg';
                }
            }
            return 'veg';
        }
    }

Here we have introduces three more syntax, namely ``static`` and
``self`` and ``::``

Here the use of ``static`` is done to declare that a particular
property is a class property. Similarly, it can be used to declare
class methods too.

You must have already guessed that ``self`` is used to access a 
static or class property/method from inside the class. Instead of 
an arrow ``->`` we use double colons ``::``. To access a static 
member from outside a class following is the syntax

.. sourcecode:: php

    $all_toppings = Pizza::$TOPPING_SETS;


Constructors and Destructors
----------------------------

These are some of the special methods that php provides us. 


Constructors
~~~~~~~~~~~~

Constructor (``__construct``) is a method that, if declared is called
automatically on a newly created object. Hence this is a good place to
initialize the object.  It can be thought as the php equivalent of
python's ``__init__`` method.

A practical use of constructor is to enforce setting of invariants. To
understand this better, let us look at how we created the pizza objects
earlier.

.. sourcecode:: php

    <?php

    $pizza1 = new Pizza();
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';
    $pizza1->price = '180';
    $pizza1->size = 'Medium';
    $pizza1->toppings = array('mushroom', 'capsicum', 'peppers');

Here we are setting the pizza's properties. But there is a chance that
we forget to set some properties which are required for any further 
processing of the pizza object. For eg. If toppings is not set, then
the is_veg_or_nonveg method will fail to tell us if the pizza is 
safe for pure vegitarians to eat! To fix this, we can define a 
constructor method as follows

.. sourcecode:: php

    <?php

    class Pizza {
    
        public function __construct($toppings) {
            $this->toppings = $toppings;
        }
    }

Now, it is not possible to create a new pizza instance without 
specifying an array of toppings.

.. note::

   This approach should be used carefully ie. in cases only when it's 
   extremely necessary to initialize the object with some must required
   properties. This is because php doesnot support method overloading
   which we will see later.


Destructors
~~~~~~~~~~~

Destructor (``__destruct``) will be called when there are no more
references to the objects.

.. sourcecode:: php

    <?php

    class Pizza {
    
        public function __destruct() {
            echo "Some one just ate me!";
        }
    }


Next up
-------

With this much basic knowlegde, we can now move to the `features of OOP`
where we will also see the meanings of terms such as ``public`` we we 
haven't discusses yet.

