#### **CSCI 1300 CS1: Starting Computing: Homework 5**
#### **Hoenigman/Naidu/Park/Ramesh - Fall 2023**
#### **Due: Friday, October 20th by 5:00pm MST**

<br/>

# Table of contents
1. [Objectives](#objectives)
2. [Background](#background)
    1. [Arrays](#arrays)
    2. [Passing arrays to functions](#arraytofunc)
    3. [Multidimensional Arrays](#2darrays)
    4. [Vectors](#vectors)
    5. [Reminders](#reminders)
3. [Questions](#questions)
    1. [Question 1](#question1)
    2. [Question 2](#question2)
    3. [Question 3](#question3)
    4. [Question 4](#question4)
    5. [Question 5a](#question5)
    6. [Question 5b](#question6)
4. [Deliverables](#deliverables)
    1. [File Header](#fileheader)
    2. [Checklist](#checklist)
    3. [Grading Rubric](#grading)

# Objectives <a name="objectives"></a>

* Understand how and when to use 1D and 2D arrays in C++
* Understand how and when to use vectors
* Understand how to pass arrays and vectors to functions
* Work through the assigned questions in VSCode and submit answers on Coderunner

# Background <a name="background"></a>
## Arrays <a name="arrays"></a>
An array is a data structure that can store primitive data types like `double`, `int`, `char`, `boolean`, and `string`.
Arrays have both a `type` and a `size`.

**How to declare arrays**
```cpp
// data_type array_name[declared_size];
bool my_booleans[10];
string my_strings[15];
int my_ints[7];
```

**How to initialize arrays (method 1)**

```cpp
bool my_booleans[4] = {true, false, true, true};
```
If you do not declare the size inside the square brackets, the array size will be set to however many entries you provide on the right.
```cpp
bool my_booleans[] = {true, false, true}; // size = 3
```
Note: the size specified in the brackets needs to match the number of elements you wrote in the curly brackets.

*Example 1*

When the specified size is larger than the actual number of elements, the elements provided in the curly brackets will be the first several elements in the array, while the additional elements will be filled with default values. If it’s an integer/double array, the default values are zero, while if it’s a string array, the default values are empty strings.

```cpp
#include <iostream>
using namespace std;
int main()
{
    int int_array[5] = {1,2,3};
    for (int i = 0; i < 5; i ++)
    {
        cout << int_array[i] << “ ”;
    }
}
```

Output:
```
1 2 3 0 0
```

*Example 2*

When the specified size is smaller than the actual number of elements, there will be a compilation error.

```cpp
#include <iostream>
using namespace std;
int main()
{
    int int_array[3] = {1,2,3,4,5};
}
```

Output:
```
error: excess elements in array initializer
int int_array[3] = {1,2,3,4,5};
                         ^
1 error generated.
```

* **How to Initialize Arrays (Method 2)**
You can also initialize elements one by one using a for loop:
```cpp
int my_ints[10];
for (int i = 0; i < 10; i++)
{
    my_ints[i] = i;
}
//{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
```
* **How to Access Elements in an Array**
We have essentially already had practice with accessing elements in an array, as in C++, a string is an array of characters.
You can access elements in arrays using the same syntax you used for strings:
```cpp
string greetings[] = {"hello", "hi", "hey", "what’s up?"};
cout << greetings[3] << endl;
```
Output
```
what's up
```

Arrays can be iterated in the same way we iterated over strings last week. Below, we are iterating through an array of strings:
```cpp
string greetings[] = {"hello", "hi", "hey", "what’s up?"};
int size = 4;
for (int i = 0; i < size; i++)
{
    cout << greetings[i] << endl;
}
```
Output:
```
hello
hi
hey
what's up?
```

## Passing arrays to functions <a name="arraytofunc"></a>
* **Passing By Value**
Up until now, when calling functions, we have always passed by value. When a parameter is passed in a function call, a new variable is declared and initialized to the value passed in the function call.

Observe that the variable `x` in main and variable `x` in addOne are separate variables in memory. When `addOne` is called with `x` on line 10, it is the value of `x` (i.e. 5) that is passed to the function. This value is used to initialize a new variable `x` that exists only in addOne's scope. Thus, the value of the variable `x` in main's scope remains unchanged even after the function `addOne` has been called.

```cpp
void addOne(int x)
{
    x = x + 1;
    cout << x << endl;
}

int main()
{
    int x = 5;
    cout << x << endl;
    addOne(x);
    cout << x << endl;
}
```

Output:
```
5
6
5
```

* **Passing By Reference**
Arrays, on the other hand, are passed by reference (to the original array’s location in the computer’s memory). So, when an array is passed as a parameter, the original array is used by the function.
Observe that there is only one array `X` in memory for the following example. When the function `addOne` is called on line 10, a reference to the original array `X` is passed to `addOne`. Because the array `X` is passed by reference, any modifications done to `X` in `addOne` are done to the original array. These modifications persist and are visible even after the flow of control has exited the function and we return to main.

```cpp
void addOne(int X[])
{
   X[0] = X[0] + 1;
   cout << X[0] << endl;
}
int main()
{
    int X[4] = {1, 5, 3, 2};
    cout << X[0] << endl;
    addOne(X);
    cout << X[0] << endl;
}
```

Output:
```
1
2
2
```

When we pass a one-dimensional array as an argument to a function, we also provide its length. For two-dimensional arrays, in addition to providing the length (or number of rows), we will also assume that we know the length of each of the subarrays (or the number of columns). A function taking a two-dimensional array with 10 columns as an argument might look something like this:
```cpp
void twoDimensionalFunction(int matrix[][10], int rows){ … }
```

## Multidimensional arrays <a name="2darrays"></a>
In C++, we can declare an array of arrays known as a multidimensional array. Multidimensional arrays store data in tabular form.
* **How to Declare Multidimensional arrays**
```cpp
// data_type array_name[dimension_1][dimension_2]....;
int my_ints[7][5];
bool my_booleans[10][15][12];
string my_strings[15][10];
```
* **How to Initialize Multidimensional arrays (Method 1)**
```cpp
int my_ints[2][2] = {1, 2, 3, 4};
```
The 2D array, in this case, will be filled from left to right from top to bottom.
```cpp
int my_ints[2][2] = {{1, 2}, {3, 4}}
```
You can also initialize a 2D array by explicitly separating the rows as shown above.

* **How to Initialize Multidimensional  arrays  (Method 2)**
You can also initialize elements using nested loops:
```cpp
int my_ints[2][2];
for(int i=0; i < 2; i++)
{
    for(int j=0; j < 2; j++)
    {
        my_ints[i][j] = i + j;
    }
}
```
The above code will create the following 2D array {{0, 1}, {1, 2}}.

* **How to Access Elements in a Multidimensional array**

You can use  `my_ints[i][j]` to access the ith row and jth column of a 2D array

Multidimensional arrays can be iterated using nested loops as shown below:
```cpp
int my_ints[2][2] = {{0, 1}, {1, 2}};
int res = 0;
for(int i=0; i < 2; i++)
{
    for(int j=0; j < 2; j++)
    {
        res = res + my_ints[i][j];
    }
}
cout << “Result is “ << res;
```
Output:
```
Result is 4
```
## **Vectors** <a name="vectors"></a>

Let's start with something we already know about - Arrays.

To recap, an array is a contiguous series that holds a *fixed number of values* of the same datatype.

A vector is a template class that uses all of the syntax that we used with vanilla arrays, but adds functionality that relieves us of the burden of keeping track of memory allocation for the arrays. It also introduces a bunch of other features that make handling arrays much simpler.

First things first, we need to include the appropriate header files to use the vector class.
```cpp
#include <vector>
```
We can now move on to declaring a vector. This is the general format of any vector declaration:
```cpp
vector <datatype_here> name(size);
```
The *size* field is optional. Vectors are dynamically sized, so the size that you give them during initialization isn't permanent - they can be resized as necessary.

You can access elements of a vector in the same way you would access elements in an array, for example `my_array[4]`. Remember, indices begin from 0.

The C++ vector class comes with [several member functions](https://www.cplusplus.com/reference/vector/vector/vector/), but the following are the ones you will need this week:

* ```size()``` returns the size of a vector
* ```at()``` takes an integer parameter for the index and returns the value at that position

Adding elements to the vector is done primarily using two functions

* ```push_back()``` takes in one parameter (the element to be added) and appends it to the end of the vector.
```cpp
vector<int> vector1; // initializes an empty vector
vector1.push_back(1); //Adds 1 to the end of the vector.
vector1.push_back(3); //Adds 3 to the end of the vector.
vector1.push_back(4); //Adds 4 to the end of the vector.
cout<< vector1.size(); //This will print the size of the vector - in this case, 3.
// vector1 looks like this: [1, 3, 4]
```

Elements can also be removed.

* ```pop_back()``` deletes the last element in the vector
```cpp
vector <int> vector1; // initializes an empty vector
vector1.push_back(1); //Adds 1 to the end of the vector.
vector1.push_back(3); //Adds 3 to the end of the vector.
vector1.push_back(4); //Adds 4 to the end of the vector.
vector1.pop_back(); //Removes the last element of the vector.
//vector1 looks like this: [1, 3]
```

## Reminders <a name="reminders"></a>
Here is a collection of useful things:

- Setting decimal points in cout (remember to include ```<iomanip>```!):

    ```cout << fixed << setprecision(number of decimal points) << floating point value << endl```

    As an example, try running this program to see the difference:
    ```cpp
    #include <iostream>
    #include <iomanip>
    using namespace std;

    int main()
    {
        cout << 7.0/13.0 << endl;
        cout << fixed << setprecision(2) << 7.0/13.0 << endl;
        return 0;
    }
    ```
- Code compilation with g++: <br>
    ```g++ -Wall -Werror -Wpedantic -std=c++17 name_of_source_file.cpp```
- Using the ```-o``` option provided by g++ to name your executable [OPTIONAL]: <br>
    ```g++ -Wall -Werror -Wpedantic -std=c++17 name_of_source_file.cpp -o nameOfExecutable```
- Good coding style: <br>
    - Name variables well, for example: ```double d = 42.0167``` is confusing, whereas ```double height_of_rectangle = 42.0167``` is clearer about what the variable represents
    - Name functions well, for example: ```int Func(int x);``` is confusing, whereas ```int calculateSquare(int x);``` is clearer about what the function does
    - Variables should be named using snake-case, where_all_words are all lowercase and separated by underscores: this_is_snake_case.
    - Functions should be named using camel-case, where all words except the first start with an uppercase letter, and there are no separators: thisIsCamelCase.
    - Indent things properly! If-else blocks should be well-spaced and indented, function blocks as well, etc. Use tab to increase indent, and shift+tab to decrease indent. Here is an example: <br>
    ```cpp
    void function(void){
    if(condition) {
    <code>}else{
    <code>}return;}
    ```
    The above is very confusing and hard to understand, simply adding space and indents really helps:
    ```cpp
    void function(void)
    {
        if (condition)
        {
            <code>
        }
        else
        {
            <code>
        }
        return;
    }
    ```
    - Remember to comment your code! Comment lines start with ```//```
    - Good places to put comments include (but are not limited to!): next to function prototypes, next to variable declarations, and next to important lines, such as numerical calculations, etc.
    - These conventions will make your code easier to read and understand


# Questions <a name="questions"></a>

### **Warning : you are not allowed to use global variables for this homework.**
### **If you are suspected of using an outside source to complete homework, you may be called for an in person interview, and could risk losing points for the assignment.**
### **For questions which require the use of a function, Coderunner will check that you have written the function correctly. DO NOT WRITE ALL YOUR CODE IN ```main()```! Your code will NOT compile if you do not have the correct function, see below for an example of what this could look like:** <br>

![Sample Incorrect Function](./images/function_error.png)

## **Question 1 (4 points): Array Expedition** <a name="question1"></a>
**You are not allowed to use vectors for this question**

*This question may require the use of arrays, if-else statements, string comparisons, declaring and calling a function, and nested loops.*

Write a C++ program to create 4 arrays and populate them with relevant values based on the description below and print them.
* **card_names**: an array of the strings "Ace", "Clubs", "Diamonds", "Hearts", "Spades", "Jack", "Queen", "King", and "Joker" in that order.
* **sq_root_nums**: an array of doubles that stores the square root of numbers from 1 to 10 (inclusive). .
* **numbers**: an array of integers that are divisible by 7 between the numbers 50 to 100.
* **letters**: an array of all uppercase and lowercase alphabets in order, A, a, B, b, C, c, ... , Z, z. Hint: the ASCII table will be helpful here!

In order to test that you correctly created and populated the arrays, print the values of each of their elements, in order, one element per line. The elements in the ```sq_root_nums``` array should be printed with 3 digits after the decimal point.

**Note:** You are required to use a loop to insert values into `sq_root_nums`, `numbers`, and `letters` arrays. Use of individual cout statements for these three arrays will receive a **0**

Develop and validate your solution in VS Code. When you are happy with your solution head over to Coderunner on Canvas and submit **the entire program** in the answer box. Please make sure to include your header! See [File Header](#fileheader) for instructions.

**Expected output:**
```
Ace
Clubs
Diamonds
.
.
.
Joker
1.000
1.414
1.732
.
.
.
3.162
56
63
.
.
.
98
A
a
B
b
C
c
.
.
.
Z
z
```

## **Question 2 (6 points): Weather Statistics** <a name="question2"></a>

**You are not allowed to use vectors for this question**

*This question may require the use of arrays, if-else statements, string comparisons, declaring and calling a function, and nested loops.*
<br>
<br>
Bob has been monitoring temperature every day to track global warming effects.
Help Bob to present his findings by writing  three functions: temperatureMean(), temperatureRange(), and temperatureDelta().
You may assume that the array will be non-empty.

*Function Specifications*:
  * The function should compute the mean/average of all the values in the array.
  * **Name**: temperatureMean()
  * **Parameters (in this order)**:
    * ```double``` new_temperature[]: array of current year's temperature.
    * ```const int``` TEMP_SIZE: size of new_temperature.
  * **Return Type**: ```double```
    * The computed mean.
  * The function should not print anything.

*Function Specifications*:
  * The function should compute the difference between the maximum and minimum values found in the array.
  * **Name**: temperatureRange()
  * **Parameters (in this order)**:
    * ```double``` new_temperature[]: array of current year's temperature.
    * ```const int``` TEMP_SIZE: size of new_temperature[].
  * **Return Type**: ```double```
    * The computed range.
  * The function should not print anything.

*Function Specifications*:
  * The function should compute the difference between the last and current year's temperature readings and store the results in the delta array.
  * **Name**: temperatureDelta()
  * **Parameters (in this order)**:
    * ```double``` new_temperature[]: array of current year's temperature readings.
    * ```double``` old_temperature[]: array of last year's temperature readings.
    * ```double``` delta[]: array to store temperature difference between last and current year's readings.
    * ```const int``` TEMP_SIZE: size of temperature[], old_temperature[] and delta[] arrays.
  * **Return Type**: ```void```
  * The function should not print anything.

Develop and validate your solution on VS code and test it by writing a main() with either assert calls or one that supports user input. Below we provide a few examples testing out valid ```temperatureMean(), temperatureRange(), temperatureDelta()``` functions.

Take note to match the function specifications exactly. When you are happy with your solution head over to Coderunner on Canvas and paste your function in the answer box! Make sure to ONLY include the ```temperatureMean(), temperatureRange(), temperatureDelta()``` functions (do not include ```main()``` or any other statements). Please make sure to include your header! See [File Header](#fileheader) for instructions.

**Sample run 1:**

Elements in arrays
```
const int TEMP_SIZE = 3;
double old_temperature[TEMP_SIZE] = {55.5, 77.7, 88.8};
double new_temperature[TEMP_SIZE] = {50.5, 67.2, 78.4};
```
Output

```
Temperature Mean : 65.367
Temperature Range : 27.900
Temperature Delta :
5.000
10.500
10.400
```
<!-- ![Q2 Sample Run 1](./images/question2_1.png) -->

**Sample run 2:**

Elements in arrays
```
const int TEMP_SIZE = 4;
double old_temperature[TEMP_SIZE] = {9, 109.5, 55.25, 47.67};
double new_temperature[TEMP_SIZE] = {-2, 100, 52, 50.55};
```
Output

```
Temperature Mean : 50.138
Temperature Range : 102.000
Temperature Delta :
11.000
9.500
3.250
-2.880
```
<!-- ![Q2 Sample Run 2](./images/question2_2.png) -->

**Sample run 3:**

Elements in arrays
```
const int TEMP_SIZE = 5;
double old_temperature[TEMP_SIZE] = {0, -19.5, 55.25, 71.167, 1.8};
double new_temperature[TEMP_SIZE] = {12, 1, 60, 50.55, -16.087};
```
Output

```
Temperature Mean : 21.493
Temperature Range : 76.087
Temperature Delta :
-12.000
-20.500
-4.750
20.617
17.887

```
<!-- ![Q2 Sample Run 3](./images/question2_3.png) -->


## **Question 3 (8 points): Most Popular Word** <a name="question3"></a>
**You are not allowed to use vectors for this question**

*This question may require the use of arrays, if-else statements, string comparisons, declaring and calling a function, and  **nested loops.***

A word is said to be most popular if it occurs more frequently than any other word.
Write a function named ```mostPopularWord()``` that finds the most popular word in an array and print the most popular word, its frequency, and the indices where you found it.
You may assume there's always one correct answer and the array will be non-empty.
If it has equal occurrence, then we always consider the most recent one.

**Note:** You do not need sorting to solve this.

*Function Specifications*:
  * **Name**: mostPopularWord()
  * **Parameters (in this order)**:
    * ```string``` words[]: array of words.
    * ```const int``` WORD_SIZE: size of words array.
  * **Return Type**: ```void```
  * **Ouput**:
    * Print the word with the highest frequency, its count, and its index positions in the array

Develop and validate your solution on VS code and test it by writing a main() with either assert calls or one that supports user input. Below we provide a few examples testing out valid ```mostPopularWord()``` function.

Take note to match the function specifications exactly. When you are happy with your solution head over to Coderunner on Canvas and paste your function in the answer box! Make sure to ONLY include the ```mostPopularWord()``` function (do not include ```main()``` or any other statements). Please make sure to include your header! See [File Header](#fileheader) for instructions.

**Sample run 1:**

Elements in array
```
const int WORDS_SIZE = 4;
string words[WORDS_SIZE] = {"mail", "text", "spam", "spam"};
```
Output

```
The most popular word : spam
Frequency : 2
Found at pos :
2
3
```

**Sample run 2:**

Elements in array
```
const int WORDS_SIZE = 5;
string words[WORDS_SIZE] = {"apple", "corn", "corn", "apple", "lettuce"};
```
Output

```
The most popular word : apple
Frequency : 2
Found at pos :
0
3
```

**Sample run 3:**

Elements in array
```
const int WORDS_SIZE = 6;
string words[WORDS_SIZE] = {"apple", "apple", "foo", "bar", "apple", "code"};
```
Output

```
The most popular word : apple
Frequency : 3
Found at pos :
0
1
4
```

<!-- ![Q3 Sample Run 1](./images/question3_1.png)
![Q3 Sample Run 2](./images/question3_2.png)
![Q3 Sample Run 3](./images/question3_3.png) -->


## **Question 4 (10 points): Kroga Gas Station** <a name="question4"></a>

**You are not allowed to use arrays for this question**

*This question may require the use of **vectors**, if-else statements, strings, nested loops, declaring and calling a function, and processing user input.*
<br>
<br>
Kroga Gas Station has reached out to you to help them write a program that can handle a menu-driven shopping experience. Write a C++ program to implement the following menu options:
  | **Option** | **Operation** |
  |----|---------|
  | 1  | Add Product   |
  | 2  | Remove Product |
  | 3  | Checkout |
  | 4  | Exit |

* Prompt the user to enter their budget and display the four menu options listed above.
* * If the user inputs a budget less than or equal to 0 then display output `Invalid input. Balance must be a non-negative value.` and exit the program.
* Your menu should run in a loop, offering the user every option until they choose to exit the program.
* Get their choice as a number 1-4, handling invalid input. Any invalid input here should print ```Invalid input.``` and then prompt them for their choice again.


**Note:** Menu options 1, 2 and 3 should be implemented in **individual functions**. The parameters and return types of these functions are up to you. You may create additional helper functions. Solutions implementing all functionality in main() will receive a **0**

  * **Add Product**:
    * Display the following menu and prompt the user to choose a product:
      | **Option** | **Product name** | **Price ($)** |
      |------------|---------------------|--------|
      | A          | Cheetos   | 2.99   |
      | B          | Oreos | 3.39   |
      | C          | Coffee           | 3.79   |
      | D          | Slurpee        | 4.29   |
    * The selected product is added to the cart if it doesn't exceed the user budget.
    * *A 9% tax is applied to all products.*

  * **Remove Product**:
    * Prompt the user to enter the product (name) they want to remove from their cart.
    * Display `Cart is empty.` if user hasn't added a product to their cart.
    * Display `Product Name not found.` if the product is not in their cart.
    * Remove the *first* occurence of the product from the cart.

  * **Checkout** :
    * Display `Cart is empty.` if user hasn't added a product to their cart.
    * Display all the products in the cart.
    * Display the cart total, three digits after the decimal point, and exit the program

  * **Exit**:
    * Exit the program

Develop and validate your solution on VS code and test it by writing a main() with either assert calls or one that supports user input. Below we provide a few examples testing out the entire program.

When you are happy with your solution head over to Coderunner on Canvas and paste your **entire program** in the answer box. Please make sure to include your header! See [File Header](#fileheader) for instructions.


(Blue is program output, and red is user input.)

**Sample run 1:**

![Q4 Sample Run 1](images/q4_1.png)

**Sample run 2:**

![Q4 Sample Run 2](images/q4_2.png)

**Sample run 3:**

![Q4 Sample Run 3](images/q4_3.png)

**Sample run 4:**

![Q4 Sample Run 4](images/q4_4.png)


## **Question 5a (8 points): Treasure map** <a name="question5"></a>

**You are not allowed to use 1D arrays or vectors for this question**

*This question may require the use of **2D arrays**, if-else statements, strings, nested loops, declaring and calling a function, and processing user input.*
<br>
<br>
You just found a treasure map, and to find the coordinates of the hidden treasure, you must solve this math problem. You will be given a list of XY coordinates of all the important clues we have solved so far and the coordinates of 3 important regions of the island. Your job is to write a C++ program to compute a distance matrix using 2D arrays and identify which region each clue belongs to.

For example, we have coordinates of 5 clues
| **Clue** | **x** | **y** |
|---|----|---|
| C1 | 2  | 10 |
| C2 | 2  | 5 |
| C3 | 8  | 4 |
| C4 | 5  | 8 |
| C5 | 1  | 2 |

we have coordinates of 3 regions in the island
| **Region** | **x** | **y** |
|---|----|---|
| R1 | 2  | 10 |
| R2 | 5  | 8 |
| R3 | 1  | 2 |

Using the distance between 2 points formula from the previous homework

$distance(p1, p2) = \sqrt{(x2 - x1)^2 + (y2 - y1)^2}$

we construct a distance matrix
|  | R1 | R2 | R3 |
|---|----|---|---|
| **C1** | 0.00  | 3.61 | 8.06
| **C2** | 5.00  | 4.24 | 3.16
| **C3** | 8.49  | 5.00 | 7.28
| **C4** | 3.61  | 0.00 | 7.21
| **C5** | 8.06  | 7.21 | 0.00

The the first value is calculated as, $d(C1, R1) = \sqrt{(2 - 2)^2 + (10 - 10)^2} = 0.00$


The the second value is calculated as, $d(C1, R2) = \sqrt{(5 - 2)^2 + (8 - 10)^2} = 3.61$ and so on.

*Function Specifications*:
  * The function should construct the distance matrix.
  * **Name**: calculateDistanceMatrix()
  * **Parameters (in this order)**:
    * ```double``` distance[][3]: empty distance matrix of size CLUE_ROWS x REG_ROWS.
    * ```double``` clue[][2]: XY coordinate for clues of size CLUE_ROWS x CLUE_COLS.
    * ```double``` region[][2]: XY coordinate for regions of size REG_ROWS x REG_COLS.
    * ```const int``` CLUE_ROWS: the number of rows for clue[][].
    * ```const int``` CLUE_COLS: the number of columns for clue[][], will be 2 for this question.
    * ```const int``` REG_ROWS: the number of rows for region[][], will be 3 for this question.
    * ```const int``` REG_COLS: the number of columns for region[][], will be 2 for this question.
  * **Return Type**: ```void```
  * The function should not print anything.

Develop and validate your solution on VS code and test it by writing a main() with either assert calls or one that supports user input. Below we provide a few examples testing out valid ```calculateDistanceMatrix()``` function.

Take note to match the function specifications exactly. When you are happy with your solution head over to Coderunner on Canvas and paste your function in the answer box! Make sure to ONLY include the ```calculateDistanceMatrix()``` function (do not include ```main()``` or any other statements). Please make sure to include your header! See [File Header](#fileheader) for instructions.


**Sample run 1:**

Elements in arrays
```
const int CLUE_ROWS = 3;
const int CLUE_COLS = 2;
const int REG_ROWS = 3;
const int REG_COLS = 2;
double clue[CLUE_ROWS][CLUE_COLS] = {{2, 10}, {8, 4}, {5, 8}};
double region[REG_ROWS][REG_COLS] = {{2, 10}, {5, 8}, {1, 2}};
```
Distance Matrix

```
0.00 3.61 8.06
8.49 5.00 7.28
3.61 0.00 7.21
```
<!-- ![Q5a Sample Run 1](./images/question5a_1.png) -->

**Sample run 2:**

Elements in arrays
```
const int CLUE_ROWS = 4;
const int CLUE_COLS = 2;
const int REG_ROWS = 3;
const int REG_COLS = 2;
double clue[CLUE_ROWS][CLUE_COLS] = {{2, 1}, {2, 3}, {4, 4}, {5, 8}};
double region[REG_ROWS][REG_COLS] = {{1, 2}, {3, 6}, {5, 8}};
```
Distance Matrix

```
1.41 5.10 7.62
1.41 3.16 5.83
3.61 2.24 4.12
7.21 2.83 0.00
```
<!-- ![Q5a Sample Run 2](./images/question5a_2.png) -->

**Sample run 3:**

Elements in arrays
```
const int CLUE_ROWS = 5;
const int CLUE_COLS = 2;
const int REG_ROWS = 3;
const int REG_COLS = 2;
double clue[CLUE_ROWS][CLUE_COLS] = {{2, 10}, {2, 5}, {8, 4}, {5, 8}, {1, 2}};
double region[REG_ROWS][REG_COLS] = {{0, 0}, {5, -1}, {-1, -2}};
```
Distance Matrix

```
10.20 11.40 12.37
5.39 6.71 7.62
8.94 5.83 10.82
9.43 9.00 11.66
2.24 5.00 4.47
```
<!-- ![Q5a Sample Run 3](./images/question5a_3.png) -->



## **Question 5b (12 points): Treasure map continued** <a name="question6"></a>

**You are not allowed to use vectors for this question**

*This question may require the use of **1D & 2D arrays**, if-else statements, strings, nested loops, declaring and calling a function, and processing user input.*
<br>
<br>
You are very close to finding the coordinates of the treasure. We'll solve this in two steps.

**Step 1:**
We'll use the computed distance matrix from 5a to find the region with the smallest distance for every clue.

For example,
|  | R1 | R2 | R3 |
|---|----|---|---|
| **C1** | 0.00  | 3.61 | 8.06
| **C2** | 5.00  | 4.24 | 3.16
| **C3** | 8.49  | 5.00 | 7.28
| **C4** | 3.61  | 0.00 | 7.21
| **C5** | 8.06  | 7.21 | 0.00

In the above matrix, the minimum value for,

**row 1** is **0.00** and its corresponding region number is **1**.

**row 2** is **3.16** and its corresponding region number is **3**.

You may assume that there is only 1 minimum value in each row.

*Function Specifications - findClueRegion()*:
  * The function should find and store the region with the smallest distance in the clue_regions array.
  * **Name**: findClueRegion()
  * **Parameters (in this order)**:
    * ```double``` distance[][3]: empty distance matrix of size DISTANCE_ROWS x DISTANCE_COLS.
    * ```int``` clue_regions[]: empty array of size DISTANCE_ROWS.
    * ```const int``` DISTANCE_ROWS: the number of rows for distance[][].
    * ```const int``` DISTANCE_COLS: the number of columns for distance[][], will be 3 for this question.
  * **Return Type**: ```void```
  * The function should not print anything.

**Step 2:**
The treasure is buried in the region with the least amount of clues.

|  | R1 | R2 | R3 | region|
|---|----|---|---|---|
| **C1** | 0.00  | 3.61 | 8.06 | 1
| **C2** | 5.00  | 4.24 | 3.16 | 3
| **C3** | 8.49  | 5.00 | 7.28 | 2
| **C4** | 3.61  | 0.00 | 7.21 | 2
| **C5** | 8.06  | 7.21 | 0.00 | 3

The **least number of clues was found in region R1** so the treasure must be there.

*Function Specifications - findTreasure()*:
  * The function should find the treasure coordinates.
  * **Name**: findTreasure()
  * **Parameters (in this order)**:
    * ```int``` clue_regions[]: region numbers for each clue
    * ```double``` region[][2]: XY coordinate of the region of size REG_ROWS x REG_COLS.
    * ```const int``` CLUE_REGIONS_SIZE: size of clue_regions[].
    * ```const int``` REG_ROWS: the number of rows for region[][], will be 3 for this question.
    * ```const int``` REG_COLS: the number of columns for region[][], will be 2 for this question.
  * **Return Type**: ```void```
  * **Output**:
    * Print the region number with least number of clues and its corresponding XY coordinates.

Develop and validate your solution on VS code and test it by writing a main() with either assert calls or one that supports user input. Below we provide a few examples testing out valid ```findClueRegion(), findTreasure(), calculateDistanceMatrix()``` functions.

Take note to match the function specifications exactly. When you are happy with your solution head over to Coderunner on Canvas and paste your function in the answer box! Make sure to ONLY include the ```findClueRegion(), findTreasure(), calculateDistanceMatrix()``` functions (do not include ```main()``` or any other statements). Please make sure to include your header! See [File Header](#fileheader) for instructions.

**Sample run 1:**

Elements in arrays
```
const int CLUE_ROWS = 5;
const int CLUE_COLS = 2;
const int REG_ROWS = 3;
const int REG_COLS = 2;
double clue[CLUE_ROWS][CLUE_COLS] = {{2, 10}, {2, 5}, {8, 4}, {5, 8}, {1, 2}};
double region[REG_ROWS][REG_COLS] = {{2, 10}, {5, 8}, {1, 2}};
```
Distance Matrix

```
0.00 3.61 8.06
5.00 4.24 3.16
8.49 5.00 7.28
3.61 0.00 7.21
8.06 7.21 0.00
```
Output

```
Region 1 had the least number of clues : 1
Treasure must be buried in the coordinates ( 2.00, 10.00 )
```
<!-- ![Q5b Sample Run 1](./images/question5b_1.png) -->

**Sample run 2:**

Elements in arrays
```
const int CLUE_ROWS = 4;
const int CLUE_COLS = 2;
const int REG_ROWS = 3;
const int REG_COLS = 2;
double clue[CLUE_ROWS][CLUE_COLS] = {{2, 1}, {2, 3}, {4, 4}, {5, 8}};
double region[REG_ROWS][REG_COLS] = {{1, 2}, {3, 6}, {5, 8}};
```
Distance Matrix

```
1.41 5.10 7.62
1.41 3.16 5.83
3.61 2.24 4.12
7.21 2.83 0.00
```
Output

```
Region 2 had the least number of clues : 1
Treasure must be buried in the coordinates ( 3.00, 6.00 )
```
<!-- ![Q5b Sample Run 2](./images/question5b_2.png) -->

**Sample run 3:**

Elements in arrays
```
const int CLUE_ROWS = 5;
const int CLUE_COLS = 2;
const int REG_ROWS = 3;
const int REG_COLS = 2;
double clue[CLUE_ROWS][CLUE_COLS] = {{2, 10}, {2, 5}, {8, 4}, {5, 8}, {1, 2}};
double region[REG_ROWS][REG_COLS] = {{0, 0}, {5, -1}, {-1, -2}};
```
Distance Matrix

```
10.20 11.40 12.37
5.39 6.71 7.62
8.94 5.83 10.82
9.43 9.00 11.66
2.24 5.00 4.47
```
Output

```
Region 3 had the least number of clues : 0
Treasure must be buried in the coordinates ( -1.00, -2.00 )
```
<!-- ![Q5b Sample Run 3](./images/question5b_3.png) -->


# Deliverables  <a name="deliverables"></a>

## File Headers <a name="fileheader"></a>

Before submitting your program on Coderunner ensure that you include the below information at the top of your file
```cpp
// CSCI 1300 Fall 2023
// Author: FirstName LastName
// TA: TA Name
// Question #
```

Example
```cpp
// CSCI 1300 Fall 2023
// Author: John Smith
// TA: Nick
// Question 1
```

## Checklist <a name="checklist"></a>
Here is a checklist for submitting the assignment:
1. Use your solutions in VS Code to complete the **Homework 5 - Coderunner** assignment on Canvas (Modules → Week 8).
2. Complete the Homework 5 Quiz. This will be published on Monday, October 16th.

## Grading Rubric <a name="grading"></a>
Note: Global variables are not permitted in this homework. The use of global variables with result in a 0 on the entire homework.

| **Criteria**                                | **Points** |
| ------------------------------------------- | ---------- |
| Question 1                                  | 4          |
| Question 2                                  | 6          |
| Question 3                                  | 8          |
| Question 4                                  | 10          |
| Question 5a                                  | 8          |
| Question 5b                                  | 12         |
| Homework 5 Quiz                             | 22         |
| Total                                       | 70         |
