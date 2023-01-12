[Algorithms and data structures](Algorithms%20and%20data%20structures.md)
## Algorithm complexity
[Big-O Algorithm Complexity Cheat Sheet (Know Thy Complexities!) @ericdrowell (bigocheatsheet.com)](https://www.bigocheatsheet.com/)
[How to find time complexity of an algorithm? | Adrian Mejia Blog](https://adrianmejia.com/how-to-find-time-complexity-of-an-algorithm-code-big-o-notation/)
[How can I find the time complexity of an algorithm? - Stack Overflow](https://stackoverflow.com/questions/11032015/how-can-i-find-the-time-complexity-of-an-algorithm)
[Analysis of Loop in Programming (enjoyalgorithms.com)](https://www.enjoyalgorithms.com/blog/time-complexity-analysis-of-loop-in-programming)

Recommended by Reddit:
[A Gentle Introduction to Algorithm Complexity Analysis (discrete.gr)](http://discrete.gr/complexity/)
[Big-O is easy to calculate, if you know how (abrah.ms)](https://justin.abrah.ms/computer-science/how-to-calculate-big-o.html)
[(6) A simple introduction to Big O notation. : learnprogramming (reddit.com)](https://www.reddit.com/r/learnprogramming/comments/370908/a_simple_introduction_to_big_o_notation/)

- O: Obere Komplexitätsgrenze 
- Ω: Untere Komplexitätsgrenze (Omega)
- Θ: Genaue Komplexität (Theta)
- n: Größe einer Eingabe, der zu verarbeiten ist -> hier: Anzahl von Elementen eines Arrays, die zu sortieren ist
- **Klausur**: Komplexität ableiten

## Selection sort
- The **selection sort algorithm** sorts an array by repeatedly finding the minimum element (considering ascending order) from the unsorted part and putting it at the beginning.
```java
void selection_sort(int arr[]) {
	int n = arr.length;
	// move boundary of unsorted subarray
	for (int i = 0; i < n-1; i++) {
		// find minimum of unsorted array
		int min = i;
		for (int j = i+1; j < n; j++) {
			if (arr[j] < arr[min]) {
				min = j;
			}
		}
		// swap found minimum with first element
		int temp = arr[min];
		arr[min] = arr[i];
		arr[i] = temp;
	}
}
```
- **Example:**
	- 5 1 4 2 8 | (n-1)+1 Operationen (n-1) - Vergleichen; 1 - Vertauschen (oder nicht)
	- **1** 5 4 2 8 | (n-2)+1
	- **1 2** 4 5 8 | (n-3)+1
	- **1 2 4** 5 8 | (n-4)+1
	- **1 2 4 5** 8 | (n-5)+1
	- alles*(n+1)
	- T(n) = n*(n+1) / 2
	- Θ(n) = O(n) = Ω(n) = n^2 (best case = worst case = average case)
- **Time complexity:** O(N^2) 
	- One loop to select an element of Array one by one = O(N)
	- Another loop to compare that element with every other array element = O(N)
	- Time complexity = O(N) * O(N) = O(N^2)

## Insertion sort
- **Insertion sort** is a simple sorting algorithm that works similar to the way you sort playing cards in your hands. The array is virtually split into a sorted and an unsorted part. Values from the unsorted part are picked and placed at the correct position in the sorted part.
	- Iterate from `arr[1]` to `arr[N]` over the array. 
	- Compare the current element (key) to its predecessor. 
	- If the key element is smaller than its predecessor, compare it to the elements before. Move the greater elements one position up to make space for the swapped element.
```java
void insertion_sort(int arr[]) {
	int n = arr.length;
	// compare arr[i + 1] with arr[i] (i >=1)
	for (int i = 1; i < n; i++) {
		int key = arr[i];
		int j = i - 1;
		// move elements of arr[0...i-1] that are **greater than the key** to 1 position ahead of their current position
		while (j >= 0 && arr[j] > key) {
			arr[j+1] = arr[j];
			j = j-1;
		}
		arr[j+1] = key;
	}
}
```
- Best case: Θ(n)
- Worst case: O(n^2)

## Bubble sort
-  Place the largest element at its position, this operation makes sure that the first largest element will be placed at the end of the array.
-  Recursively call for rest **n – 1** elements with the same operation and place the next greater element at their position.
-  The base condition for this recursion call would be, when the number of elements in the array becomes **0** or **1** then, simply return (as they are already sorted).
```java
// n = length of unsorted array
public void bubble_sort(int[] arr, int n) {
	for (int i = 0; i < n-1; i++) {
		if (arr[i] > arr[i+1]) {
			int temp = arr[i];
			arr[i] = arr[i+1];
			arr[i+1] = temp;
		}
	}
	bubblesort(arr, n-1) //recursive
}
```

```java
public class BubbleSorter { public static void sort(int[] arr) { 
	for (int i = 0; i < arr.length - 1; i++) {
		for (int j = 0; j < arr.length - 1; j++) {
			if (arr[j] > arr[j+1]) {
				int bigger = arr[j]; 
				int smaller = arr[j+1]; 
				arr[j+1] = bigger; 
				arr[j] = smaller; 
			} 
		} 
	}
]
```
- Best case: Θ(n)
- Worst case: O(n^2)

## Quick sort
Visualizer: [Visualizer - Quick sort](https://cscircles.cemc.uwaterloo.ca/java_visualize/#code=public+class+GFG+%7B%0A++%0A++++//+A+utility+function+to+swap+two+elements%0A++++static+void+swap(int%5B%5D+arr,+int+i,+int+j)%0A++++%7B%0A++++++++int+temp+%3D+arr%5Bi%5D%3B%0A++++++++arr%5Bi%5D+%3D+arr%5Bj%5D%3B%0A++++++++arr%5Bj%5D+%3D+temp%3B%0A++++%7D%0A++%0A++++/*+This+function+takes+last+element+as+pivot,+places%0A+++++++the+pivot+element+at+its+correct+position+in+sorted%0A+++++++array,+and+places+all+smaller+(smaller+than+pivot)%0A+++++++to+left+of+pivot+and+all+greater+elements+to+right%0A+++++++of+pivot+*/%0A++++static+int+partition(int%5B%5D+arr,+int+low,+int+high)%0A++++%7B%0A++%0A++++++++//+pivot%0A++++++++int+pivot+%3D+arr%5Bhigh%5D%3B%0A++%0A++++++++//+Index+of+smaller+element+and%0A++++++++//+indicates+the+right+position%0A++++++++//+of+pivot+found+so+far%0A++++++++int+i+%3D+(low+-+1)%3B%0A++%0A++++++++for+(int+j+%3D+low%3B+j+%3C%3D+high+-+1%3B+j%2B%2B)+%7B%0A++%0A++++++++++++//+If+current+element+is+smaller%0A++++++++++++//+than+the+pivot%0A++++++++++++if+(arr%5Bj%5D+%3C+pivot)+%7B%0A++%0A++++++++++++++++//+Increment+index+of%0A++++++++++++++++//+smaller+element%0A++++++++++++++++i%2B%2B%3B%0A++++++++++++++++swap(arr,+i,+j)%3B%0A++++++++++++%7D%0A++++++++%7D%0A++++++++swap(arr,+i+%2B+1,+high)%3B%0A++++++++return+(i+%2B+1)%3B%0A++++%7D%0A++%0A++++/*+The+main+function+that+implements+QuickSort%0A++++++++++++++arr%5B%5D+--%3E+Array+to+be+sorted,%0A++++++++++++++low+--%3E+Starting+index,%0A++++++++++++++high+--%3E+Ending+index%0A+++++*/%0A++++static+void+quickSort(int%5B%5D+arr,+int+low,+int+high)%0A++++%7B%0A++++++++if+(low+%3C+high)+%7B%0A++%0A++++++++++++//+pi+is+partitioning+index,+arr%5Bp%5D%0A++++++++++++//+is+now+at+right+place%0A++++++++++++int+pi+%3D+partition(arr,+low,+high)%3B%0A++%0A++++++++++++//+Separately+sort+elements+before%0A++++++++++++//+partition+and+after+partition%0A++++++++++++quickSort(arr,+low,+pi+-+1)%3B%0A++++++++++++quickSort(arr,+pi+%2B+1,+high)%3B%0A++++++++%7D%0A++++%7D%0A++%0A++++//+Function+to+print+an+array%0A++++static+void+printArray(int%5B%5D+arr,+int+size)%0A++++%7B%0A++++++++for+(int+i+%3D+0%3B+i+%3C+size%3B+i%2B%2B)%0A++++++++++++System.out.print(arr%5Bi%5D+%2B+%22+%22)%3B%0A++%0A++++++++System.out.println()%3B%0A++++%7D%0A++%0A++++//+Driver+Code%0A++++public+static+void+main(String%5B%5D+args)%0A++++%7B%0A++++++++int%5B%5D+arr+%3D+%7B+10,+7,+8,+9,+1,+5+%7D%3B%0A++++++++int+n+%3D+arr.length%3B%0A++%0A++++++++quickSort(arr,+0,+n+-+1)%3B%0A++++++++System.out.println(%22Sorted+array%3A+%22)%3B%0A++++++++printArray(arr,+n)%3B%0A++++%7D%0A%7D%0A++%0A//+This+code+is+contributed+by+Ayush+Choudhary&mode=display&curInstr=69)
- Choose a pivot element (usually: last element)
- Divide elements into two groups: F1 (all elements < pivot element) and F2 (elements >= pivot element)
- Sort F1 and F2 recursively until F1 and F2 only contains 1 elementelement
```java
static void swap(int[] arr, int i, int j) {
	int temp = arr[i];
	arr[i] = arr[j];
	arr[j] = temp;
}

//takes last element as pivot, places the pivot element at its correct posotion in sorted array, places all smaller to left of pivot and all greater to high of pivot
static int partition(int[] arr, int low, int high) {
	//pivot is last element
	int pivot = arr[high];
	//index of smaller element and indicates the right position so far
	int i = (low - 1);
	for (int j = low; j <= high; j++) {
		//if current element is smaller than pivot
		if (arr[j] < pivot) {
			//increment index of smaller element
			i++;
			swap(arr, i, j)
		}
	}
	swap(arr, i+1, high);
	return (i + 1);
}

// low: starting index, high: ending index
static void quicksort(int[] arr, int low, int high) {
	if (low < high) {
		//pi is partition index 
		int pi = partition(arr, low, high);
		//separately sort elements before partition and after partition
		quicksort(arr, low, pi-1);
		quicksort(arr, pi+1, high);
	}
}
```
- Time complexity: [Time complexity: Best and Worst cases | Quick Sort | Appliedcourse - YouTube](https://www.youtube.com/watch?v=4nVbJV5pZa8)
	- Best case: T(n) = O(nlogn) -> z.B: 4 1 3 9 6 7 5
		- 4 1 3 9 6 7 5: n = 7 -> 2 Durchläufe
		- In jedem Durchlauf: alle Elementen mit dem Pivotelement vergleichen -> (n-1) Schritte
		- ![[../../_assets/quick sort - best case.png | 300]]
	- Worst case: O(n^2) -> sortierte List z.B: 1 2 3 4 5 6 (because algorithm has to recursively sort the list until only 1 element in group)
		- (n-1) Durchläufe, in jedem Durchlauf (n-1)/2 (Menge wird nach jedem Durchläuf kleiner)
	- Average case: n = 7 

## Heap sort
Visualizer: [Visualizer - Heap sort](https://cscircles.cemc.uwaterloo.ca/java_visualize/#code=public+class+HeapSort+%7B%0A++++public+void+sort(int+arr%5B%5D)%0A++++%7B%0A++++++++int+N+%3D+arr.length%3B%0A+%0A++++++++//+Build+heap+(rearrange+array)%0A++++++++for+(int+i+%3D+N+/+2+-+1%3B+i+%3E%3D+0%3B+i--)%0A++++++++++++heapify(arr,+N,+i)%3B%0A+%0A++++++++//+One+by+one+extract+an+element+from+heap%0A++++++++for+(int+i+%3D+N+-+1%3B+i+%3E+0%3B+i--)+%7B%0A++++++++++++//+Move+current+root+to+end%0A++++++++++++int+temp+%3D+arr%5B0%5D%3B%0A++++++++++++arr%5B0%5D+%3D+arr%5Bi%5D%3B%0A++++++++++++arr%5Bi%5D+%3D+temp%3B%0A+%0A++++++++++++//+call+max+heapify+on+the+reduced+heap%0A++++++++++++heapify(arr,+i,+0)%3B%0A++++++++%7D%0A++++%7D%0A+%0A++++//+To+heapify+a+subtree+rooted+with+node+i+which+is%0A++++//+an+index+in+arr%5B%5D.+n+is+size+of+heap%0A++++void+heapify(int+arr%5B%5D,+int+N,+int+i)%0A++++%7B%0A++++++++int+largest+%3D+i%3B+//+Initialize+largest+as+root%0A++++++++int+l+%3D+2+*+i+%2B+1%3B+//+left+%3D+2*i+%2B+1%0A++++++++int+r+%3D+2+*+i+%2B+2%3B+//+right+%3D+2*i+%2B+2%0A+%0A++++++++//+If+left+child+is+larger+than+root%0A++++++++if+(l+%3C+N+%26%26+arr%5Bl%5D+%3E+arr%5Blargest%5D)%0A++++++++++++largest+%3D+l%3B%0A+%0A++++++++//+If+right+child+is+larger+than+largest+so+far%0A++++++++if+(r+%3C+N+%26%26+arr%5Br%5D+%3E+arr%5Blargest%5D)%0A++++++++++++largest+%3D+r%3B%0A+%0A++++++++//+If+largest+is+not+root%0A++++++++if+(largest+!%3D+i)+%7B%0A++++++++++++int+swap+%3D+arr%5Bi%5D%3B%0A++++++++++++arr%5Bi%5D+%3D+arr%5Blargest%5D%3B%0A++++++++++++arr%5Blargest%5D+%3D+swap%3B%0A+%0A++++++++++++//+Recursively+heapify+the+affected+sub-tree%0A++++++++++++heapify(arr,+N,+largest)%3B%0A++++++++%7D%0A++++%7D%0A+%0A++++/*+A+utility+function+to+print+array+of+size+n+*/%0A++++static+void+printArray(int+arr%5B%5D)%0A++++%7B%0A++++++++int+N+%3D+arr.length%3B%0A+%0A++++++++for+(int+i+%3D+0%3B+i+%3C+N%3B+%2B%2Bi)%0A++++++++++++System.out.print(arr%5Bi%5D+%2B+%22+%22)%3B%0A++++++++System.out.println()%3B%0A++++%7D%0A+%0A++++//+Driver's+code%0A++++public+static+void+main(String+args%5B%5D)%0A++++%7B%0A++++++++int+arr%5B%5D+%3D+%7B+12,+11,+13,+5,+6,+7+%7D%3B%0A++++++++int+N+%3D+arr.length%3B%0A+%0A++++++++//+Function+call%0A++++++++HeapSort+ob+%3D+new+HeapSort()%3B%0A++++++++ob.sort(arr)%3B%0A+%0A++++++++System.out.println(%22Sorted+array+is%22)%3B%0A++++++++printArray(arr)%3B%0A++++%7D%0A%7D&mode=display&curInstr=124)
- Build complete binary tree
- Transform into max heap (parent node should always be greater or equal to child nodes)
- Perform heap sort: remove max element in each step (move to last position), consider the remaining elements and transform it into a max heap

```java
public void heapsort(int[] arr) {
	int n = arr.length;
	//build heap (Anfangsheap herstellen) , all elements after (n/2 -1) are children (has no children)
	for (int i = n/2 - 1; i >= 0; i--) {
		heapify(arr, n, i);
	}

	//one by one extract root element from heap and replace it with last element
	for (int i = n-1; i > 0; i--) {
		//move current root to end, move end to root
		int temp = arr[0];
		arr[0] = arr[i];
		arr[i] = temp;
		//call max heapify on the reduced heap (so the biggest element is root)
		heapify(arr, i , 0);
	}
}

// root: node i; n: size of heap
void heapify(int arr[], int n, int i) {
	int largest = i; //largest is root 
	int left = 2*i + 1; //left child
	int right = 2*i + 2; //right child

	//if left child is larger than largest
	if (left < n && arr[left] > arr[largest]) {
		largest = left;
	}

	//if right child is larger than largest
	if (right < n && arr[right] > arr[largest]) {
		largest = right;
	}

	//if largest is not root -> swap
	if (largest != i) {
		int swap = arr[i];
		arr[i] = arr[largest];
		arr[largest] = swap;
		//recursively heapify the affected subtree
		heapify(arr, n, largest);
	}
}
```
- Time complexity: O(nlogn)

## Radix sort
- 123, 113, 005, 011, 451, 900
- L = 3 (dreistellig = k ); m = 10 (Dezimal); n = 5 (Anzahl von Elementen)
- Verteilungsphase: die n zu sortierenden Elementen werden auf m Fächer verteilt (nach dem FIFO-Prinzip) 
- Sammelphase: 

```java
// Radix Sort in Java Programming
import java.util.Arrays;

class RadixSort {

  // Using counting sort to sort the elements in the basis of significant places
  void countingSort(int array[], int size, int place) {
    int[] output = new int[size + 1];
    int max = array[0];
    for (int i = 1; i < size; i++) {
      if (array[i] > max)
        max = array[i];
    }
    int[] count = new int[max + 1];

    for (int i = 0; i < max; ++i)
      count[i] = 0;

    // Calculate count of elements
    for (int i = 0; i < size; i++)
      count[(array[i] / place) % 10]++;

    // Calculate cumulative count
    for (int i = 1; i < 10; i++)
      count[i] += count[i - 1];

    // Place the elements in sorted order
    for (int i = size - 1; i >= 0; i--) {
      output[count[(array[i] / place) % 10] - 1] = array[i];
      count[(array[i] / place) % 10]--;
    }

    for (int i = 0; i < size; i++)
      array[i] = output[i];
  }

  // Function to get the largest element from an array
  int getMax(int array[], int n) {
    int max = array[0];
    for (int i = 1; i < n; i++)
      if (array[i] > max)
        max = array[i];
    return max;
  }

  // Main function to implement radix sort
  void radixSort(int array[], int size) {
    // Get maximum element
    int max = getMax(array, size);

    // Apply counting sort to sort elements based on place value.
    for (int place = 1; max / place > 0; place *= 10)
      countingSort(array, size, place);
  }
}
```