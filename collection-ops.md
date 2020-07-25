Collection Operations
---------------------



### What's that `var total = 0` doing in your main function?

```dart
var total = 0;

for (final product in cartProducts) {
  total += product.price;
}
```

* Getting the total price is business logic
  - `main()` acts as if it were presentation
* getting totals is a common collection operation
  - a **reduction**: _reducing_ a list or array of several elements into a single value 
  (like `total`) or object



### Reductions (1/2)

* Usually getters, unless expensive or not idempotent <!-- .element style="font-size: 1em" -->
  - final grade from `student.activityResults`
  - average salary of `company.employees`
  - highest daily increase of `covid.dailyStats`

```dart []
// imperative idiom
DataType get someReduction {
  var finalResult = initialValue;

  for (final element in parts) {
    finalResult /* operation/method */ element.someProp;
  }

  return finalResult;
}
```



### Reductions (2/2)

```dart [1-6 | 8-21 | 12-20]
class Stat {
  int confirmedCaseCount;
  int deathCount;
  int recoveriesCount;
  DateTime tallyDate;
}

class PandemicRegion {
  String name; // e.g. Iloilo City, or Region 6
  List&lt;Stat&gt; dailyTally;

  int get highestDailyIncrease {
    var highest = 0;

    for (final stat in dailyTally) {
      highest = max(highest, stat.confirmedCaseCount);
    }

    return highest;
  }
}
```

* Reductions can return objects too, e.g. `Stat`
  - so you know which date has the highest daily increase



### Linear Search (1/7)

* The slowest search algorithm, running at O(n)
  - ignore if you haven't taken SE 1222 (Data Structures) yet
* examines each element one at a time
  - adds it to a `searchResults` list if it passes the search criteria, usually given as params
  - can be a getter if no params
  - empty list if no matches found
* can also be used to find a single item
  - return `null` if no matches found
    + it is the usual OOP idiom



### Linear Search (2/7)

```dart []
List&lt;PartsType&gt; findBySomeCriteria(SomeType criteria) {
  final searchResults = &lt;PartsType&gt;[];

  for (final element in parts) {
    if (part matches criteria) {
      searchResults.add(element);  // ✔ CORRECT
      /* parts.add(element);  // ❌ insufficient coffee */
    }
  }

  return searchResults;
}
```

* This is the usual imperative idiom.
  - just be aware of a possible error in Line 7 when drowsy
  - no compiler warning (and most likely no linter warning)



### Linear Search (3/7)

Searching for "alcohol"

<div style="flex: 1; font-size: 0.65em">
  <ul>
    <li style="list-style-type: none">*** <code>shop.items</code></li>
    <li>
      Red wine 15% alcohol <span style="color: blue">280.00</span>
      <span class="fragment" style="color: green" data-fragment-index="1">✔</span>
    </li>
    <li>
      Green Cross 40% ethyl alcohol <span style="color: blue">37.00</span>
      <span class="fragment" style="color: green" data-fragment-index="3">✔</span>
    </li>
    <li>
      Petguard dog shampoo <span style="color: blue">61.50</span>
      <span class="fragment" style="color: red" data-fragment-index="5">❌</span>
    </li>
    <li>
      Safeguard Lemon Handwash <span style="color: blue">99.00</span>
      <span class="fragment" style="color: red" data-fragment-index="6">❌</span>
    </li>
    <li>
      Joy Antibac Dishwashing liquid <span style="color: blue">139.00</span>
      <span class="fragment" style="color: red" data-fragment-index="7">❌</span>
    </li>
    <li>
      Band-aid 70% isopropyl alcohol <span style="color: blue">48.00</span>
      <span class="fragment" style="color: green" data-fragment-index="8">✔</span>
    </li>
    <li>
      Disposable face mask <span style="color: blue">29.75</span>
      <span class="fragment" style="color: red" data-fragment-index="10">❌</span>
    </li>
    <li>
      Devotion for alcoholics <span style="color: blue">350.00</span>
      <span class="fragment" style="color: green" data-fragment-index="11">✔</span>
    </li>
    <li>
      Vitamin C with Zinc 100 tab box <span style="color: blue">625.00</span>
      <span class="fragment" style="color: red" data-fragment-index="13">❌</span>
    </li>
  </ul>
  <ul>
    <li style="list-style-type: none">*** <code>searchResults</code></li>
    <li class="fragment" data-fragment-index="2">
      Red wine 15% alcohol <span style="color: blue">280.00</span>
    </li>
    <li class="fragment" data-fragment-index="4">
      Green Cross 40% ethyl alcohol <span style="color: blue">37.00</span>
    </li>
    <li class="fragment" data-fragment-index="9">
      Band-aid 70% isopropyl alcohol <span style="color: blue">48.00</span>
    </li>
    <li class="fragment" data-fragment-index="12">
      Devotion for alcoholics <span style="color: blue">350.00</span>
    </li>
    <li style="list-style-type: none">&nbsp;</li>
    <li style="list-style-type: none">&nbsp;</li>
    <li style="list-style-type: none">&nbsp;</li>
    <li style="list-style-type: none">&nbsp;</li>
    <li style="list-style-type: none">&nbsp;</li>
  </ul>
</div>



### Linear Search (4/7)

Searching for "sanitizer"

<div style="flex: 1; font-size: 0.65em">
  <ul>
    <li style="list-style-type: none">*** <code>shop.items</code></li>
    <li>
      Red wine 15% alcohol <span style="color: blue">280.00</span>
      <span class="fragment" style="color: red" data-fragment-index="1">❌</span>
    </li>
    <li>
      Green Cross 40% ethyl alcohol <span style="color: blue">37.00</span>
      <span class="fragment" style="color: red" data-fragment-index="2">❌</span>
    </li>
    <li>
      Petguard dog shampoo <span style="color: blue">61.50</span>
      <span class="fragment" style="color: red" data-fragment-index="3">❌</span>
    </li>
    <li>
      Safeguard Lemon Handwash <span style="color: blue">99.00</span>
      <span class="fragment" style="color: red" data-fragment-index="4">❌</span>
    </li>
    <li>
      Joy Antibac Dishwashing liquid <span style="color: blue">139.00</span>
      <span class="fragment" style="color: red" data-fragment-index="5">❌</span>
    </li>
    <li>
      Band-aid 70% isopropyl alcohol <span style="color: blue">48.00</span>
      <span class="fragment" style="color: red" data-fragment-index="6">❌</span>
    </li>
    <li>
      Disposable face mask <span style="color: blue">29.75</span>
      <span class="fragment" style="color: red" data-fragment-index="7">❌</span>
    </li>
    <li>
      Devotion for alcoholics <span style="color: blue">350.00</span>
      <span class="fragment" style="color: red" data-fragment-index="8">❌</span>
    </li>
    <li>
      Vitamin C with Zinc 100 tab box <span style="color: blue">625.00</span>
      <span class="fragment" style="color: red" data-fragment-index="9">❌</span>
    </li>
  </ul>
  <ul>
    <li style="list-style-type: none">*** <code>searchResults</code></li>
    <li style="list-style-type: none; margin-left: 32px" class="fragment" data-fragment-index="10">
      <img src="images/empty.png" alt="empty results">
    </li>
  </ul>
</div>



### Linear Search (5/7)

```dart [1-10 | 12-26 | 28-38]
class Item {
  String description;
  double price;

  // TELL DON'T ASK
  bool matchesDescription(String keyword) =>
    description
      .toLowerCase()
      .contains(keyword.toLowerCase());
}

class Shop {
  List&lt;Item&gt; items = [];

  List&lt;Item&gt; findByKeywords(String keyword) {
    final searchResults = &lt;Item&gt;[];

    for (final item in items) {
      if (item.matchesDescription(keyword)) {
        searchResults.add(item);
      }
    }

    return searchResults;
  }
}

// main.dart
final keyword = stdin.readLineSync(); // shampoo
final results = shop.findByKeywords(keyword);

if (results.isEmpty) {
  print("*** Sorry, no matching items for '$keyword'.");
} else {
  for (final item in results) {
    print('${item.description}: ${item.price}');
  }
}
```

It's always a good practice (and good UX) to handle empty search results <!-- .element class="fragment" -->




### Linear Search (6/7)

```dart [1-5 | 10-22 | 24-31]
class Item {
  String description;
  double price;
  Promo promo; // one-to-one
}

class Shop {
  List&lt;Item&gt; items = [];

  // can be a getter too, can be expensive if many items
  List&lt;Item&gt; get promoItems {
    final results = &lt;Item&gt;[];

    for (final item in items) {
      if (item.promo != null) {
        results.add(item);
      }
    }

    return results;
  }
}

// main.dart
if (shop.promoItems.isEmpty) {
  print("*** Sorry, no promos right now.");
} else {
  for (final item in shop.promoItems) {
    print('${item.description} ${item.price}');
  }
}
```

We can also have another getter `hasPromo`, remember code should read like sentences.



### Linear Search (7/7)

```dart [1-5 | 10-21 | 24-32]
class Item {
  String description;
  double price;
  String serialNumber;
}

class Shop {
  List&lt;Item&gt; items = [];

  // serial numbers are unique, only one item has it
  Item findBySerialNumber(String serialNumber) {
    final searchResults = &lt;Item&gt;[];

    for (final item in items) {
      if (item.serialNumber == serialNumber) {
        return item;
      }
    }

    return null; // loop ends without finding a match
  }
}

// main.dart
final serial = stdin.readLineSync(); // 42G-AZNY943
final item = shop.findByKeywords(serial);

if (item == null) {
  print("*** Sorry, no item w/ S/N '$serial'.");
} else {
  print('${item.description}: ${item.price}');
}
```

No loop this time, but should still check if you got a result (not null).



### Existential Statements

- true if `some` element(s) passes a test (at least one)

```dart
bool get hasAthiests { // returns one value like a reduction
  for (final member in members) {
    // has a conditional like linear search
    if (member.religion == null) {
      return true; // stop search, you've found an athiest
    }
  }

  return false; // for-loop ends without finding an athiest
}
```

* Return **true** when you find one that passes
  - return false when loop ends and unable to find any



### Universal Statements

- true if -- and only if -- `every` element passes a test

```dart
bool get noFailures { // returns one value like a reduction
  for (final student in classroll) {
    // has a conditional like linear search
    if (student.grade.isDeficiency()) { // 5.0, D, L, Inc, -
      return false; // stop search, you've found a failure
    }
  }

  return true; // for-loop ends without finding a failure
}
```

* Return **false** when you find one that doesn't pass
  - return true when loop ends and unable to find any



### Mapping (1/2)

* More commonly used in the presentation layer
  - but domain classes can make use of this in some cases
* a list **maps** 1:1 to another list (or other structures)
  - same number of elements, could be different types
  - useful when extracting some inner objects

<div style="flex: 1; font-size: 0.65em; margin-top: 16px">
<ul>
  <li style="list-style-type: none; font-size: 1.3em">
    <strong>country.cities</strong>
  </li>
  <li><code>City('Iloilo')</code></li>
  <li><code>City('Pasig')</code></li>
  <li><code>City('Manila')</code></li>
  <li><code>City('Quezon')</code></li>
  <li><code>City('Davao')</code></li>
</ul>
<ul style="margin-left: 128px">
  <li style="list-style-type: none; font-size: 1.3em">
    <strong>country.mayors</strong>
  </li>
  <li><code>Leader('Jerry', 'Treñas')</code></li>
  <li><code>Leader('Vico', 'Sotto')</code></li>
  <li><code>Leader('Isko', 'Moreno')</code></li>
  <li><code>Leader('Joy', 'Belmonte')</code></li>
  <li><code>Leader('Sara', 'Duterte')</code></li>
</ul>
</div>



### Mapping (2/2)

```dart [1-8 | 10-17 | 19-24 | 26-35 | 38-50]
class Leader {
  String firstName;
  String lastName;

  Leader(this.firstName, this.lastName);

  @override String toString() => '$firstName $lastName';
}

class City {
  String name;
  Leader mayor;

  City(this.name, {this.mayor});

  @override String toString() => '$name City';
}

class Country {
  String name;
  Leader president;
  List&lt;City&gt; cities;

  Country(this.name, {this.cities});

  List&lt;Leader&gt; get mayors {
    // we can call the local var `mayors` as well
    final mayors = &lt;Leader&gt;[];

    for (final city in cities) {
      mayors.add(city.mayor);
    }

    return mayors;
  }
}

void main() {
  final ph = Country('Philippines',
    cities: [
      City('Iloilo', mayor: Leader('Jerry', 'Treñas')),
      City('Pasig', mayor: Leader('Vico', 'Sotto')),
      City('Manila', mayor: Leader('Isko', 'Moreno')),
      City('Quezon', mayor: Leader('Joy', 'Belmonte')),
      City('Davao', mayor: Leader('Sara', 'Duterte')),
    ]
  );
  print(ph.cities);
  print(ph.mayors);
}
```

<pre style="font-size: 0.5em">
ph.cities:
[Iloilo City, Pasig City, Manila City, Quezon City, Davao City]
ph.mayors:
[Jerry Treñas, Vico Sotto, Isko Moreno, Joy Belmonte, Sara Duterte]
</pre>



### Functional approaches

* All those ceremonial for-loops are repetitive
  - In SE-2123, you'll learn FP, allowing you to do these:

```dart [4-16]
class Country {
  /* some other code */

  List&lt;Leader&gt; get mayors => 
    cities.map(city => city.mayor);

  bool get hasFemaleLeaders =>
    cities.any(city => city.mayor.isFemale);

  List&lt;Leader&gt; searchByName(String name) => 
    cities.where(city => 
      city
        .name
        .toLowerCase()
        .contains(name.toLowerCase())
    );
}
```