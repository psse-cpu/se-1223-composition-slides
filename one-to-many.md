One-to-many
-----------



### No new syntax or keyword to learn

![seriously](images/again.jpg)



### You already know how to do this

```dart [1-6 | 8-22 | 10-19]
class Hobby {
  String name;
  int calorieBurn;

  Hobby(this.name, this.calorieBurn) {}
}

class Person {
  String name;
  List&lt;Hobby&gt; _hobbies = []; // ensure not null

  learnHobby(Hobby newHobby) {
    // we can change list to another data structure later
    // or even reject hobbies already learned
    // or maybe reject hobbies bad for the health
    _hobbies.add(newHobby);
  }

  List&lt;Hobby&gt; get hobbies => _hobbies;

  Person({ this.name }) {}
}
```

+ In **some cases**, we might be able to forego the methods and getters



#### Test-driving the class

```dart [1-11 | 13-18]
final people = [
  Person(name: 'Boy Balhas'),
  Person(name: 'Boy Em-El')
];

people[0].learnHobby(Hobby('Biking', 650));
people[0].learnHobby(Hobby('Rock Climbing', 825));

people[1].learnHobby(Hobby('ML', 75));
people[1].learnHobby(Hobby('Eating', -300));
people[1].learnHobby(Hobby('Trashtalking', 57));

for (final person in people) {
  print(person.name + ':');
  for (final hobby in person.hobbies) {
    print('\t${hobby.name} 😰 ${hobby.calorieBurn}/hr');
  }
}
```

<pre style="font-size: 0.4em" class="fragment">
Boy Balhas:
	Biking, 😰 650/hr
	Rock Climbing, 😰 825/hr
Boy Em-El:
	ML, 😰 75/hr
	Eating, 😰 -300/hr
	Trashtalking, 😰 57/hr
</pre>
