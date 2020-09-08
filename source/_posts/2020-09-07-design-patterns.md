---
layout: post
title:  "design patterns"
tags:
 - CVA
 - i
---

interesting topics during design pattern lectures

# implmenetation by intentions

```java
    public void printReport (String CustomerID) { 
        if(!isValid(CustomerID)) throw new ArgumentException(); 
        Employee[] emps = getEmployees(CustomerID);
        if(needsSorting(emps)) sortEmployees(emps); printHeader(CustomerID);
        printFormattedEmployees(emps);
        printFooter(CustomerID);
    }
```
one public method contains multiple private methods fulfilling one specifc logic at a time. it's your logical sequences when you implement `printReport` in your head. by doing this you can achieve

- Method cohesion
- separation of concerns
    - sergeant method: calls other methods
    - private methods: implementing code
- clarity - clear code is better than comments
- easy in finding/forming certain patterns
- no extra work is required


# Commonality-Variability Analysis, CVA

Assume you start a new project solving software problem in a certain domain. you heard bunch of requirments from your clients. how would you find a entities, create an abstraction with whom, find a suitable patterns among them.

at this time CVA can help you

requirements
- US Tax
- Canadian PST, GST
- Validation of addresses strcuture in different locations

from this requiremtns you can derive a following  potential entities and abstractions.


examle of CVA:
```
Country
----
US
Canada

Tax
-----
Canadian Province
Canadian Fed
US

AddressValidation
------
US
Canada

```

