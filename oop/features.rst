Buzz words, err.. Features!
===========================


Encapsulation
-------------

In simple terms, encapsulation is data hiding. In our previous
example, we created a new pizza object and set some properties which
basically represent the data about the pizza. But there may be a need
that some data should be ``private`` and should not be changeable from
outside. 

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

    $pizza1 = new Pizza();
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';
    $pizza1->size = 'Medium';
    $pizza1->toppings = array('mushroom', 'capsicum', 'peppers');

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

    $pizza1 = new Pizza();
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';
    $pizza1->size = 'Medium';
    $pizza1->toppings = array('mushroom', 'capsicum', 'peppers');

    echo $pizza1->get_price(); // 100 + 3 * 12 = 136

    $pizza1->price = 240; // $% Error !#


Now price cannot be set from outside, but only from inside the class.
So the problem is fixed. Well, this one is but there is another problem
waiting for us.


Getter and Setter Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Could you find the problem in the above code? Let's see what the problem is.

Consider we wrote code as such

.. sourcecode:: php

    <?php

    $pizza1 = new Pizza();
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';
    $pizza1->size = 'Medium';
    $pizza1->toppings = array('mushroom', 'capsicum', 'peppers');

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

    $pizza1 = new Pizza();
    $pizza1->name = 'Mushroom Capsicum Pepper Pizza1';
    $pizza1->size = 'Medium';
    $pizza1->toppings = array('mushroom', 'capsicum', 'peppers');

    echo $pizza1->get_price(); // 100 + 3 * 12 = 136

    echo $pizza1->get_price(); // 100 + 3 * 20 = 160


Now the business is good!

.. note:: 

   Always do calculations on latest state. Because state _is_ bad.


Abstraction
-----------
TODO


Inheritance
-----------
TODO


Polymorphism
------------
TODO


