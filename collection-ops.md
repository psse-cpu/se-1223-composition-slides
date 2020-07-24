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
* **Reduction** is a common collection operation
  - _reducing_ a list or array of who-knows-how-many elements into a single value (like `total`) 
    or object.



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



### Linear Search (1/5)

* The slowest search algorithm, running at O(n)
  - ignore if you haven't taken SE 1222 (Data Structures) yet
* examines each element one at a time
  - adds it to a `searchResults` list if it passes the search criteria, usually given as params
  - can be a getter if no params
  - empty list if no matches found
* can also be used to find a single item
  - return `null` if no matches found
    + returning `null` is the usual idiom




### Linear Search (2/5)

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



### Linear Search (3/5)

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
    print('${item.name}: ${item.price}');
  }
}
```

It's always a good practice (and good UX) to handle empty search results



### Linear Search (4/5)

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
    print('${item.name}: ${item.price}');
  }
}
```

We can also have another getter `hasPromo`, remember code should read like sentences.



### Linear Search (5/5)

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
  print('${item.name}: ${item.price}');
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
  - return false when loop ends (unable to find any)



### Universal Statements

- true if -- and only if -- `every` element passes a test

```dart
bool get noFailuress { // returns one value like a reduction
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
  - return true when loop ends (unable to find any)

