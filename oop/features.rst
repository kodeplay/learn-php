Buzz words, err.. Features!
===========================


Encapsulation
-------------

In simple terms, encapsulation is data hiding or precisely hiding of
state details. In our previous example, we created a new pizza object
and set some properties which basically represent the data about the
pizza. But there may be a need that some data should be ``private``
and should not be changeable from outside.

The ultimate truth about building softwares is that requirements
change all the time. Lets suppose we need to make the price of the
pizza dynamic depending upon the size and the number of toppings 
that go on to it. Let's try to modify our class definition to 
facilitate this.

.. sourcecode:: php

    <?php

    class Pizza {

        // ...

        public function __construct($toppings, $size) {
            $this->toppings = $toppings;
            $this->size = $size;
            $this->price = $this->calculate_price();
        }

        public function calculate_price() {
            if ($this->size === 'regular') $sf = 8;
            if ($this->size === 'medium') $sf = 12;
            if ($this->size === 'large') $sf = 20;
            return 100 + $sf * count($this->toppings);
        }
    }

    // Now let's create a pizza instance

    $pizza1 = new Pizza(array('mushroom', 'capsicum', 'peppers'),
                        'Medium');
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';

    echo $pizza1->price; // 100 + 3 * 12 = 136


So the price of the pizza comes to be 136. But what if we set the price
from outside 

.. sourcecode:: php

    <?php

    $pizza1->price = 240;

    echo $pizza1->price; // 240


Looks like we could hack the pizza!

.. note::

   Apart from this problem of data security, there is another problem with the 
   above implementation. Can you spot it without looking at the next section ?


Did you notice that so far we have declared all members of a class as
``public``? This essentially means that they can be both accessed and
set from anywhere in the code. To fix our current problem, we need to
a way to define the instance property price such that it can only be
changed from inside the class. For that we have the ``private`` access
modifier. Let's rewrite our class

.. sourcecode:: php

    <?php

    class Pizza {

        // name, size, toppings remain public

        private $price;

        public function __construct($toppings, $size) {
            $this->toppings = $toppings;
            $this->size = $size;
            $this->price = $this->calculate_price();
        }

        public function calculate_price() {
            if ($this->size === 'regular') $sf = 8;
            if ($this->size === 'medium') $sf = 12;
            if ($this->size === 'large') $sf = 20;
            return 100 + $sf * count($this->toppings);
        }

        public function get_price() {
            return $this->price;
        }
    }

    $pizza1 = new Pizza(array('mushroom', 'capsicum', 'peppers'),
                        'Medium');
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';

    echo $pizza1->get_price(); // 100 + 3 * 12 = 136

    $pizza1->price = 240; // $% Error !#


Now price cannot be set from outside, but only from inside the class.
So the problem is fixed. Well, this one is but there is another problem
waiting for us.


State is aweful
~~~~~~~~~~~~~~~

Could you find the problem in the above code? Let's see what the problem is.

Consider we wrote code as such

.. sourcecode:: php

    <?php

    $pizza1 = new Pizza(array('mushroom', 'capsicum', 'peppers'),
                        'Medium');
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';

    echo $pizza1->get_price(); // 100 + 3 * 12 = 136

    $pizza1->size = 'Large';
    
    echo $pizza1->get_price(); // 136


What just happened? We just sold some one a large pizza at the price of medium! #fail

So the bug was that we calculated the price of the pizza once while initializing it
but we didn't keep track of the changes in size or toppings. Here is how we fix it 
by asking our object to do the price calculations for us on demand

.. sourcecode:: php

    <?php

    class Pizza {

        // name, size, toppings remain public

        private $price;

        public function __construct($toppings, $size) {
            $this->toppings = $toppings;
            $this->size = $size;
        }

        public function get_price() {
            if ($this->size === 'regular') $sf = 8;
            if ($this->size === 'medium') $sf = 12;
            if ($this->size === 'large') $sf = 20;
            return 100 + $sf * count($this->toppings);
        }
    }

    $pizza1 = new Pizza(array('mushroom', 'capsicum', 'peppers'),
                        'Medium');
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';

    echo $pizza1->get_price(); // 100 + 3 * 12 = 136

    echo $pizza1->get_price(); // 100 + 3 * 20 = 160


Now we are doing business!

.. note:: 

   Always do calculations on latest state. Because state _is_ aweful.


Inheritance
-----------

Introduction
~~~~~~~~~~~~

Inheritance is the mechanism of deriving a new class from an existing class.
Just like we say a kid inherits features or skills of their parents, similarly
inheritance in in context of OOP means a child class (also known as a sub class)
inherits some or all properties and methods of it's parent class. 

Here is an example that illustrates parent-child relationship in 
a class. 

.. sourcecode:: php

    <?php

    class Person {
	    private $name;
	    private $address;
     
	    public function getName() {
		    return $this->name;
	    }
    }
     
    class Customer extends Person {
	    private $customer_id;
	    private $record_date;
     
	    public getCustomerId() {
		    return $this->customer_id;
	    }
     
	    public getCustomerName() {
		    return $this->getName();// getName() is in Person
	    }
    }


In the above example we have inherited class customer from person. We
tell PHP to do this by using the keyword ``extends`` while defining
the child class. Now the class customer will have all the properties
(variable and methods) of the class person. The class customer can
also have its own properties for e.g. `getCustomerId()` method in
Customer class.


Access Specifiers - private, protected, public
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Lets say you have a house. Now whole house is not accessible to all
the people of the world. If a courierman comes to your home he is
allowed to see only the door of your home. A relative is allowed to
enter inside the house but till the drawing room and not in the
bedroom or the cupboard of your bedroom. But your family members are
allowed to access all the properties of your house.

Now let us take it into programming world.

Private
********

.. sourcecode:: php

    <?php

    class Customer {
        private $name;
        public $age;
         
        public function __construct($name, $age) {
	    $this->name = $name;
	    $this->age = $age;
        }
    }
         
    $c = new Customer("Jimit","28");
    echo "Name : " . $c->name; //causes an error 
       
        
In the above example, the statement;

echo “Name : ” . $c->name;

causes an error as we are trying to access $name that has been declared as a private 
member variable. We can however access the $age data member without any limitation as its 
public. We will learn more about public later in this tutorial.            

Public
*******

.. sourcecode:: php

    <?php
    
        class Customer {
	        private $name;
	        public $age;
         
	        public function __construct($name, $age) {
		        $this->name = $name;
		        $this->age = $age;
	        }
        }
         
        $c = new Customer("Sunil","28");
        echo "Age : " . $c->age; //prints 28

In the above example, the statement;

echo “Age : ” . $c->;age;

prints 28 on the screen as $age is a public variable and hence can be accessed from 
anywhere in the script. Please note that if you declare any data member or method 
without a access specifier it is considered as ‘public’. 

Protected
**********

.. sourcecode:: php

    <?php
        class Person {
	        protected $name;
        }
         
        class Customer extends Person {
	        function setName($name) {
		        //this works as $name is protected in Person
		        $this->name = $name;
	        }
        }
         
        $c1 = new Customer();
        $c1->setName("Jimit");
        $c1->name = "Jimit"; //this causes error as $name is protected and not public
    
In the above example, the statement;

$this->name = $name;

in the setName() function is referring to the $name data member of the Person class. 
This access is only possible because the $name variable has been declared as protected. 
Had this been private; the above statement would have raised an error. Further, in the 
statement towards the end;

$c1->name = “Jimit”;

raises an error as $name in the Person class has been declared as protected and not public.   


Method Overridding
~~~~~~~~~~~~~~~~~~

Method overriding is when the function of base class is re-defined with the same name, 
function signature and access specifier (either public or protected) of the derived class.

The reason to override method is to provide additional functionality over and above what 
has been defined in the base class. Imagine that you have a class by the name of Bird 
from which you derive two child classes viz. Eagle and Swift. The Bird class has methods 
defined to eat, fly, etc, but each of the specialized classes viz Eagle and Swift will 
have its own style of flying and hence would need to override the flying functionality.

.. sourcecode:: php

    <?php

    class Bird {
        public function fly() {
            echo "Fly method of Bird Class called";
        }
    }
    
    class Eagle extends Bird {
        public function fly() {
             echo "Fly method of the Eagle Class called";
        }
    }

    class Swift extends Bird {
        public function fly() {
            echo "Fly method of the Swift Class called";
        }
    }

    $e = new Eagle();
    $s = new Swift();

    $e->fly();
    echo "\n";
    $s->fly();


In the above example, we create two objects of class Eagle and Swift. 
Each of these classes have overridden the method fly() and have provided their own 
implementation of the fly() method that has been extended from the Bird class. The 
manner in which they have been extended the Bird class fly() method is not called as 
both these classes have provided a new functionality for the fly() method.


Invoking parent methods
~~~~~~~~~~~~~~~~~~~~~~~

When you override a method of the base class, it’s functionality is completely hidden 
unless it has been explicitly invoked from the child class. To invoke a parent class 
method you should use the keyword parent followed by the scope resolution operator 
followed by the name of the method as mentioned below: 

    `parent::function_name();`
    
Look at the example below:    

.. sourcecode:: php

    <?php    

    class Person {
            
        public function calculateData() {
            echo "Data calculated in Person Class \n";
        } 
            
        public function showData() {
            echo "This is Person's showData()\n";
        }
    }

    class Customer extends Person{
    
        public function calculateData() {
            echo "Data calculated in Customer Class \n";
        }
        
        public function showData() {
            parent::showData();
            echo "This is Customer's showData()\n";
        }
    }
    
    $c = new Customer();
    $c->showData();
    $c->calculateData();

    
In the above example, look at the way in which the showData() function in the Customer 
child class is invoking the the Person parent class’s showData() function. When the 
program executes the showData() method if the Customer class is called which inturn calls 
the showData() function of the parent class. After the parent class’s showData() function 
complets its execution the remaining code in showData() function of the Customer class is 
executed.


Abstraction
-----------

Abstraction facilitates providing a description to a some behaviour or concept or an
object in your code. In our previous pizza example, we have already seen abstraction.
Let us see if we can point it out by going through the code again.

.. sourcecode:: php

    <?php

    class Pizza {

        // name, size, toppings remain public

        private $price;

        public function __construct($toppings, $size) {
            $this->toppings = $toppings;
            $this->size = $size;
        }

        public function get_price() {
            if ($this->size === 'regular') $sf = 8;
            if ($this->size === 'medium') $sf = 12;
            if ($this->size === 'large') $sf = 20;
            return 100 + $sf * count($this->toppings);
        }
    }

In the above example, get_price is an abstraction. What if our
language did not allow us to write a method like get_price? In that
case we would still be able to solve the problem. How ? We could get
along by doing the price calculations everytime we need to find the
price.

.. sourcecode:: php

    <?php

    $pizza1 = new Pizza(array('mushroom', 'capsicum', 'peppers'),
                        'Medium');
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';

    if ($pizza1->size === 'regular') $sf = 8;
    if ($pizza1->size === 'medium') $sf = 12;
    if ($pizza1->size === 'large') $sf = 20;
    echo 'Price of ' . $pizza1->name . 'is ' . 100 + $sf * count($pizza1->toppings);

This is both inconvenient and inelegant because not only it means
duplicate code. We would be able to compute the price but our 
class now lacks the ability to express or describe the concept
of computing price from toppings and size. When an end user (someone using 
or reading your code) has a description of get_price then he/she doesn't 
need to know about the specifics of how the price is calculated. Tomorrow the 
price calculation formula might be different but the end user can still
safely expect that ``get_price`` will return a valid price.

Similarly, when we say ``class Pizza extends FoodItem``, FoodItem is a 
useful abstraction because it right away gives some meaning or description
to Pizza, even before we define a Pizza class. 

.. note:: 

   If you were thinking that abstraction is data hiding then you were
   plain wrong. This statement misguides and is no. 1 reason for all
   confusion about abstraction. More than data, it aims at hiding 
   the implementation.

   The definition of abstraction discussed above is taken from the
   book Structure & Interpretation of computer programs or SICP. If
   you like the above definition of abstraction, then this book
   is strongly recommended to you although it's about Functional
   Programming, another approach to building programs.

    
Polymorphism
------------

Polymorphism is a feature of OOP in which classes are allowed to have
different implementation but with the same interface.

Let us get hands on and build upon our Pizza example to write 
a program for a Pizzeria for printing their menu or catalog.

Our Pizzeria sells Garlic Breads, Pasta and beverages too along with
Pizzas. We need to write a program that will print a menu card 
consisting of all these items. But before that we need to understand
what are interfaces. 

.. sourcecode:: php

    <?php

    interface FoodMenuItem {
        public function get_name();
        public function is_veg_or_nonveg();
        public function get_price();
        public function get_ingredients();
    }

Interface is nothing but a contract. The above interface defines and
four methods but without any implementation. We are already familiar
with the ``is_veg_or_nonveg`` and ``get_price`` methods from the Pizza
class.  The other methods are ``get_name`` and
``get_ingredients``. What does this mean?

Let us go one step at a time - 

* We are defining a contract for all objects that will be part of 
  a food menu, Pizza is one such type (class) of object and there
  can be other types too.

* Foodmenu will be used by customers who visit the food joint to decide what 
  to they want to eat. So the menu should tell them 4 things
  - is the food item veg or nonveg
  - the price of the food item
  - it's name
  - the ingredients
    
* So by specifying that a class implements foodmenu interface, we are making 
  sure that that class will provide us with these 4 methods so that we 
  can obtain all the information needed to put an object of that class
  into our foodmenu.

For a concrete example, lets us get back to our pizzeria example. Apart from 
Pizzas, our pizzeria also sells other food items such as Garlic bread, Pasta
and beverages and so the menu card should include these too. Let us define classes
for these food items and bind them to have a contract with our interface.

.. sourcecode:: php

    <?php

    class Pizza implements FoodMenuItem {

        // define name, size etc.

        public static $TOPPING_SETS = array(
            'veg' => array('tomato', 'capsicum', 'paneer', 'onions', 'olives'),
            'nonveg' => array('chicken', 'mutton', 'beef', 'pork'),
        );

        public function get_name() {
            return $this->name;
        }

        private $price;

        public function __construct($toppings, $size) {
            $this->toppings = $toppings;
            $this->size = $size;
        }

        public function get_price() {
            if ($this->size === 'regular') $sf = 8;
            if ($this->size === 'medium') $sf = 12;
            if ($this->size === 'large') $sf = 20;
            return 100 + $sf * count($this->toppings);
        } 

        public function is_veg_or_nonveg() {
            foreach ($this->toppings as $topping) {
                if (in_array($topping, self::$TOPPING_SETS['non_veg'])) {
                    return 'non_veg';
                }
            }
            return 'veg';
        }

        public function get_ingredients() {
            return array_merge($this->toppings, array('cheese'));
        }
    }

    
    class GarlicBread implements FoodMenuItem {
        
        public function get_name() {
            return 'Garlic Bread'
        }

        public function get_price() {
            return 100;
        }

        public function is_veg_or_nonveg() {
            return 'veg';
        }

        public function get_ingredients() {
            return array('bread', 'garlic', 'butter', 'cheese', 'pepper');
        }
    }


    class Beverage implements FoodMenuItem {

        public function __construct($name) {
            $this->name = $name;
        }

        public function get_name() {
            return $this->name;
        }

        public function get_price() {
            return 30;
        }

        public function is_veg_or_nonveg() {
            return 'veg';
        }

        public function get_ingredients() {
            return array('caffiene', 'sugar', 'co2', 'pesticides');
        }
    }

    
    class Pasta implements FoodMenuItem {

        public color;

        public function get_name() {
            return $this->color . ' Pasta';
        }

        public function get_price() {
            return $this->is_veg_or_nonveg() === 'veg' ? 100 : 130;
        }

        public function is_veg_or_nonveg() {
            foreach ($this->get_ingredients() as $ing) {
                if (in_array($ing, Pizza::$TOPPING_SETS['non_veg'])) {
                    return 'non_veg';
                }
            }
            return 'veg';
        }

        public function get_ingredients() {
            return array('cheese', 'pasta', 'olives', 'capsicum', 'onions');
        }
    }


Whenever a class implements an interface, it must include all the methods 
that the interface defines along with the same signature. Now let us write 
a FoodMenu class that will also take care of telling the printer how to 
print itself.

.. sourcecode:: php

    <?php 

    class FoodMenu {

        private $items;

        public function add_item(FoodMenuItem $item) {
            $this->items[] = $item;
        }

        public function to_print() {
            $p = '<ul>';
            for ($this->items as $item) {
                $p .= '<li class="' . $item->is_veg_or_nonveg() . '">';
                $p .= '    <h3>' . $item->get_name() . '</h3>'
                $p .= '    <p>' . implode(', ', $item->get_ingredients()) . '</p>'
                $p .= '    <p>' . $item->get_price() . '</p>'
                $p .= '</li>';
            }
            $p .= '</ul>';
            return $p;
        }
    }

    $foodmenu = new FoodMenu();
    
    $pizza = new Pizza(array('toppings', 'here'), 'Large');
    // create and initialize few more pizzas

    $foodmenu->add_item($pizza);

    $garlicbread = new Garlicbread();
    // create and initialize garlic breads

    $foodmenu->add_item($garlicbread);

    $coke = new Beverage('coke');
    $foodmenu->add_item($coke);
    $pepsi = new Beverage('pepsi');
    $foodmenu->add_item($pepsi);

    $whitepasta = new Pasta();
    $whitepasta->color = 'white';
    $foodmenu->add_item($whitepasta);

    $redpasta = new Pasta();
    $redpasta->color = 'red';
    $foodmenu->add_item($redpasta);

    echo $foodmenu->to_print();


When the to_print method is invoked, it is capable of printing 
information about all menu items even though all these food 
items are quite uncommon and different from each other. The only
way this was possible was because of the contract that the interface
makes. It's like the interface tells the class that "it's alright that
you are unique or have your own identity but make sure that you know
how to speak to a printer if you happen to get added to a foodmenu"

