 #ikg

## Vorlesung
[[Data structures]]
[[Algorithms]]
[[Command line patterns]]
[[Sorting algorithms]]
[[Hash function]]
[[Graphs]]

## Resources
[Index of /~cs1020/tut/15s2 (nus.edu.sg)](https://www.comp.nus.edu.sg/~cs1020/tut/15s2/)

## Belegarbeit
[[Dokumentation]]
### Englisch
Each transaction is saved as an instance of the class ``Transaction``. This class has the private variables date (Type: LocalDate), name (Type: String), category (Type: String), and amount (Type: double). It constructor ``Transaction(String date, String name, String category, double amount)`` takes in a date string (which will then be converted to LocalDate using the class method ``convertDate(String string)``), a name string, a category string, and a double amount.

Transactions will be saved in a ``TransactionList``, which inherits from the class ``LinkedList``. In addition to the parent class' methods, the class ``TransactionList`` is also responsible for taking in user input as well as creating command line prompts for the users. It also has additional methods to edit transactions, import/export transactions (from and to a csv file), sort the transactions (using the quicksort algorithm), search the transactions (using simple linear search. An instance of this class is also printed in a special format in order to fit the use case.

[Time and Space complexity of Quick Sort (opengenus.org)](https://iq.opengenus.org/time-and-space-complexity-of-quick-sort/)

[algorithm - Intuitive explanation for why QuickSort is n log n? - Stack Overflow](https://stackoverflow.com/questions/10425506/intuitive-explanation-for-why-quicksort-is-n-log-n)










