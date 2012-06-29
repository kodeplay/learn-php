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


To fix this, we need to define the instance property price such that it 
can only be changed from inside the class. For that we have the ``private``
access modifier. Let's rewrite our class

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
Or we can say a new human being born from from an existing human being.
Lets say a baby is born to the women. That baby will have all the all or some features 
of the women. She will have two hands, two legs, eyes and lots more.

Now, lets see how it is done. No not how the baby is born (you are smart enough 
to know that :) ). We will see how the classes are inherited in PHP. The classes are 
inherited with the keyword `extends` . Look at the example below.

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


In the above example we have inherited class customer from person. Now the class customer 
will have all the properties (variable and methods) of the class person. The class
customer can also have its own properties for e.g. `getCustomerId()` method in Customer
class.

Access Specifiers - private, protected, public
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Lets say you have a house (or imagine or your parents). Now whole house is not accessiable
to all the people of the world. If a courierman comes to your home he is allowed to see 
only the door of your home. A relative is allowed to enter inside the house but till the 
drawing room and not in the bedroom or the cupboard of your bedroom. But your family 
members are allowed to access all the properties of your house.

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
    
Polymorphism
------------
TODO


