# Programming Questions
---

1) Roll a 6-sided die 1000 times and give the results.

```javascript
// JS
function rollDice(sides) {
  var count = {}
  // set all side counts to 0
  for (let x = 1; x <= sides; x++) { count[x] = 0 }
  // roll the dice 1000 times
  for (let x = 0; x < 1000; x++)
    { count[Math.ceil((Math.random() * sides))] += 1 }
  return count
}
```

2) Test if an array has duplicates. Both work for integers and strings.

```javascript
// JS
// fastest solution using a hash

function hasDuplicates(array) {
  var count = {}
  return array.some(x => {
    // test if the element has been added already
    count[x] = ( count[x] === undefined ? 1 : 2 )
    return count[x] == 2
  })
}

// slower because of the sort, but works all the same

function hasDuplicates(array) {
  return array.sort().some((x,i) => {
      if (i !== array.length-1)
        // if the element equals the next one, return false
        { return array[i] === array[i+1] }
  })
}
```

3) OO Programming: create classes with parent-child relationships.

+ show how a child would call a parent's method
+ show how overloading works (overloading doesn't exist in JS, needs to be spoofed)
+ include syntax for class variables

```javascript
// JS
class Phone {

  constructor(name, number) {
    this.name = name
    this.number = number
  }

  call() { console.log(`Calling ${this.name} at ${this.number}`) }
}

class CellPhone extends Phone {

  constructor(name, number, model, carrier) {
    super(name, number)
    this.model = model
    this.carrier = carrier
  }

  text(message) { console.log(`Texting ${this.name} at ${this.number}: "${message}"`) }
  reset() { console.log(`Turning my ${this.model} off and on again`) }
}

class CordlessPhone extends Phone {

  base() { console.log("Making the phone beep, good luck finding it") }
}

me = new Phone("Mike's Phone", "555-1212")
me.name //=> Mike's Phone
me.number //=> 555-1212
me.call() //=> Calling Mike's Phone at 555-1212

iPhone = new CellPhone("my iPhone", "555-1234", "iPhone 6", "AT&T")
iPhone.name //=> my iPhone
iPhone.model //=> iPhone 6
iPhone.call() //=> Calling my iPhone at 555-1234
iPhone.text("hey") //=> Texting my iPhone at 555-1234: "hey"
iPhone.reset() //=> Turning my iPhone 6 off and on again

housePhone = new CordlessPhone("my house phone", "555-2121")
housePhone.name //=> my house phone
housePhone.number //=> 555-2121
housePhone.call() //=> Calling my house phone at 555-2121
```

Overloading in JS (indirectly), since in JS if you redo a function it replaces it

```javascript
// JS
function multiple(a, b, c) {
  if (b === undefined)
    { console.log(a) }
  else if (c === undefined)
    { console.log(a, b) }
  else
    { console.log(a, b, c) }
}

multiple("hello") //=> hello
multiple("hello", "world") //=> hello world
multiple("hello", "world", "!") //=> hello world !
```

4) Write a SQL query to:

+ retrieve a list of order ids and their totals (including shipping amount)
+ only show records prior to 6/1/2003
+ sort newest orders first

orders | orderitems
---|---
id | id
date | order_id
shipping_amount | product_id
order_status | price
customer_id | quantity

outputting

OrderID | OrderTotal
---|---
13230 | $55.00
54455 | $40.00
59694 | 433.04

```sql
SELECT order_items.order_id AS 'OrderID', SUM(order_items.price*order_items.quantity)+orders.shipping_amount AS 'OrderTotal'
FROM order_items
JOIN orders
ON order_items.order_id = orders.id
GROUP BY order_items.order_id
ORDER BY orders.date DESC
```

5) Using the above query's results, loop with forEach that:

+ outputs the orders in an HTML table (2 columns with headings)
+ alternating with CSS classes

```html
<!DOCTYPE html>
<html>
  <head>
    <style>
      tr:nth-child(even) {
        background-color: #cccccc;
      }
    </style>
    <meta charset="utf-8">
    <title>Order Details</title>
  </head>
  <body>
    <p id="order_table"></p>

    <script>
      var db = $.ajax({url: "URL"})
      var table = document.getElementById("order_table")
      table.append("<table>")
      db.forEach((row, i) => {
        if (i == 0) {
          table.append("<tr>" + row.forEach(col => table.append(`<th>${col}</th>`)) + "/<tr>")
        } else {
          table.append("<tr>" + row.forEach(col => table.append(`<td>${col}</td>`)) + "/<tr>")
        }
      })
      table.append("</table>")      
    </script>
  </body>
</html>
```

6) Write a SQL query to:

+ retrieve a list of the most 5 popular states that had orders

orders | customers
---|---
id | id
date | first_name
shipping_amount | last_name
order_status | city
customer_id | state

outputting

State | Orders
---|---
NY | 55
CA | 40
NJ | 33

```sql
SELECT customers.state AS 'State', COUNT(orders.id) AS 'Orders'
FROM customers
JOIN orders
ON customers.id = orders.customer_id
GROUP BY customers.state
ORDER BY COUNT(orders.id) DESC
```
---

# Programming Fundamentals
---

1) Convert 0xF624 (Hex) to decimal format

```javascript
// JS
// parseInt(num, base)
parseInt("0xF624", 16) //=> 63012
```

2) Convert a custom number base to decimal (example is base 26)

```javascript
// JS
// parseInt(num, base)
parseInt("A", 26) //=> 10
parseInt("B", 26) //=> 11
parseInt("P", 26) //=> 25
parseInt("MB", 26) //=> 583
parseInt(2017, 26) //=> 35185
```

3) Object inheriting vs. instantiating a copy within a class using has-a vs. is-a

JavaScript doesn't directly have instantiating's has-a and inheriting's is-a, however classes handle the basics of these two concepts. Inheriting classes in JS means that in the above Phone example, if you extend Phone into Cell so that Cell now inherits Phone, Cell has all of Phone's methods and can add more of its own, but Phone doesn't have any of Cell's newly added methods. Instantiating just means that I can modify the Phone class to also have a method for voicemail, and the Phone would get the voicemail's methods while the voicemail wouldn't have any of Phone's. You'd use inheritance when you'd want your class to be a part of the superclass, while you'd instantiate when you'd only want your class to be able to simply use more methods included in another location.
