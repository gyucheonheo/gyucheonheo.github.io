---
layout: post
title: Sort List with fixed start items 
subtitle:
tags: [java, stream, sort]
comments: true
---

Recently, I needed to write a piece of code snippet to convert a list of strings into a list of strings that should start with 2 items. 
I started it with being dumb.

Let's say that your list should start with "Chicago" followed by "Illinois" and you don't care a rest of them once you meet "Chicago" followed by "Illinois".
Additionally, a given list may or may not have "Chicago" or "Illinois".

The first code that I pumped out smells a lot. 
1. Too many if-else : I'm lucky to have only 2 items requirement. What if it should starts with 10 items? Its complexity will grow exponentially. Error-prone.
2. I repeat myself
3. In general, it is vomit-causing code. I hate it.

``` java
List<String> newList = new ArrayList<>();
List<String> lst = List.of("San Fransisco", "California", "St Louis", "Missouri", "Washington", "Seattle", "Chicago", "Wisconsin", "Illinois");

if (lst.contains("Chicago") && lst.contains("Illinois")) {
    for (String s : lst) {
        if (s.equals("Chicago")) newList.add(0, s);
        else if (s.equals("Illinois")) newList.add(1, s);
        else newList.add(s);
    }
} else if (lst.contains("Chicago") && !lst.contains("Illinois")) {
    for (String s : lst) {
        if (s.equals("Chicago")) newList.add(0, s);
        else newList.add(s);
    }
} else if (!lst.contains("Chicago") && lst.contains("Illinois")) {
    for (String s : lst) {
        if (s.equals("Illinois")) newList.add(0, s);
        else newList.add(s);
    }
} else {
    for (String s : lst) {
        newList.add(s);
    }
}

newList.forEach(System.out::println);
```

As a result of spending a few hours playing the code, I ended up sorting it based on fixed items.
It's pretty intuitive and there is nothing that I have to explain as extra. 
Neat and simple.

``` java
Map<String, Integer> fixedItems = Map.of("Chicago", 0, "Illinois", 1);
List<String> lst = List.of("San Fransisco", "California", "St Louis", "Missouri", "Washington", "Seattle", "Chicago", "Wisconsin", "Illinois");

List<String> newList = lst.stream()
                          .distinct()
                          .sorted(Comparator.comparing( s -> {
                            int i = fixedItems.getOrDefault(s, -1);
                            return i >= 0 ? i : fixedItems.size();
                            }))
                          .collect(Collectors.toUnmodifiableList());
```
Although the number of fixed items increase, this code will be able to handle it in scale without any changes except for `fixedItems`. 
