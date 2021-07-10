# Python Most Common Challenges

Interview Common Problems

![](https://cdn-images-1.medium.com/max/1200/1*fggC8t551kI30A-X0GYWDA.jpeg)

[**Link to Kaggle Notebook for all these exercises together**](https://www.kaggle.com/paulrohan2020/pure-python-exercises)

#### Q: Product of two matrices

Given two matrices print the product of those two matrices

#### Solution with Explanations and Theory

#### Basics of Matrix Multiplication

Multiplication rule to remember — Here also the flow is Row and then Column

1.  Sum-Product of First Row \* 1st Column => Becomes the 1-st\_Row-1-st\_Column of the resultant Matrix
2.  Sum-Product of 1st Row \* 2-nd Column => Becomes the 1-st\_Row-2-nd\_Column of the resultant Matrix

#### There are four simple rules that will help us in multiplying matrices, listed here:

#### 1\. Firstly, we can only multiply two matrices when the number of columns in matrix A is equal to the number of rows in matrix B.

#### 2\. Secondly, the first row of matrix A multiplied by the first column of matrix B gives us the first element in the matrix AB, and so on.

#### 3\. Thirdly, when multiplying, order matters — specifically, AB ≠ BA.

#### 4\. Lastly, the element at row i, column j is the product of the ith row of matrix A and the jth column of matrix B.

Further read through [this](https://www.mathsisfun.com/algebra/matrix-multiplying.html) for a very nice visual flow of Matrix Multiplication.

undefined
undefined

```
def matrix_mul(A, B):    num_rows_A = len(A)    num_columns_A = len(A[0])    num_rows_B = len(B)    num_columns_B = len(B[0])    # To multiply an m×n matrix by an n×p matrix, the ns must be the same,    # and the result is an m×p matrix.    if num_columns_A != num_rows_B:        print(            "Matrix multiplication of two arguments not possible as number of columns in first Matrix is NOT equal to the number of rows in second Matrix")        return    # Create an result matrix which will have    # dimensions of num_rows_A x num_columns_B    # And fill this matrix with zeros    result_matrix = [[0 for i in range(num_columns_B)] for j in range(num_rows_A)]    # Now implementing the key principle    # The element at row i, column j is the product of the ith row of matrix A and the jth column of matrix B.    for i in range(num_rows_A):        for j in range(num_columns_B):            for k in range(num_columns_A):                # k-th column of A should be the k-th row of B                result_matrix[i][j] += A[i][k] * B[k][j]    return result_matrixA = [[1, 3, 4],     [2, 5, 7],     [5, 9, 6]]B = [[1, 0, 0],     [0, 1, 0],     [0, 0, 1]]A1 = [[1, 2],      [3, 4]]B1 = [[1, 2, 3, 4, 5],      [5, 6, 7, 8, 9]]A2 = [[1, 3, 4], [5, 9, 6]]B2 = [[1, 0, 0], [0, 0, 1]]print(matrix_mul(A, B))print(matrix_mul(A1, B1))print(matrix_mul(A2, B2))
```

```
[[1, 3, 4], [2, 5, 7], [5, 9, 6]][[11, 14, 17, 20, 23], [23, 30, 37, 44, 51]]Matrix multiplication of two arguments not possible as number of columns in first Matrix is NOT equal to the number of rows in second MatrixNone
```

#### Q: Proportional Probability Sampling

Select a number randomly with probability proportional to its magnitude from the given array of n elements

consider an experiment, selecting an element from the list A randomly with probability proportional to its magnitude. assume we are doing the same experiment for 100 times with replacement, in each experiment you will print a number that is selected randomly from A.

Ex 1: A = \[1, 5, 27, 6, 13, 28, 100, 45, 10, 79\] let f(x) denote the number of times x getting selected in 100 experiments. f(100) > f(79) > f(45) > f(28) > f(27) > f(13) > f(10) > f(6) > f(5) > f(0)

#### Solution and Explanations and Notes on methods, steps, algorithms

The below solution is inspired by the explanations given in [this Stackoverflow](https://stackoverflow.com/questions/16489449/select-element-from-array-with-probability-proportional-to-its-value) question.com

**To explain further on the question** -

I have an array of doubles and I want to select a value from it with the probability of each value being selected being proportional to its value. For example:

```
arr[0] = 100arr[1] = 200
```

In this example, element 0 would have a 66% of being selected and element 1 a 33% chance.

#### The general algorithm is

*   sum the array
*   Pick a random number between 0 and the sum
*   Accumulate the values starting from the beginning of the array until you are >= to the random value.

**Note: All values must be positive for this to work.**

The usual technique is to transform the array into an array of cumulative sums:

```
[10 60 5 25]  --> [10 70 75 100]
```

Pick a random number in the range from zero up to the cumulative total (in the example: `0 <= x < 100`). Then, use [bisection](http://en.wikipedia.org/wiki/Binary_search_algorithm) on the cumulative array to locate the index into the original array:

```
Random variable x      Index in the Cumulative Array      Value in Original Array-----------------      -----------------------------      ---------------------- 0 <= x < 10                      0                            1010 <= x < 70                      1                            6070 <= x < 75                      2                             575 <= x < 100                     3                            25
```

For example, if the random variable _x_ is 4, bisecting the cumulative array gives a position index of 0 which corresponds to 10 in the original array.

And, if the random variable _x_ is 72, bisecting the cumulative array gives a position index of 2 which corresponds to 5 in the original array.

For an inverse proportion, the technique is exactly the same except you perform an initial transformation of the array into its reciprocals and then build the cumulative sum array:

```
[10 60 5 25]  -->  [1/10  1/60  1/5  1/25]  -->  [1/10  7/60  19/60  107/300]
```

#### Note on the bisect function and module that I am using in this solution

From [official-doc](https://docs.python.org/3/library/bisect.html) — `bisect.bisect(a, x, lo=0, hi=len(a))` - Returns an insertion point for x in a which comes after (to the right of) any existing entries of x in a.

Some nice explanations from [geeksforgeeks](https://www.geeksforgeeks.org/bisect-algorithm-functions-in-python/) -

The purpose of Bisect algorithm is to find a position (i.e. the index) in list where an element needs to be inserted to keep the list sorted.

Python in its definition provides the bisect algorithms using the module “bisect” which allows keeping the list in sorted order after insertion of each element. This is essential as this reduces the overhead time required to sort the list again and again after the insertion of each element.

**Bisect Function**

1.  **bisect(list, num, beg, end)** :- This function returns the position in the sorted list, where the number passed in argument can be placed so as to maintain the resultant list in sorted order. If the element is already present in the list, the right most position where element has to be inserted is returned. This function takes 4 arguments, list which has to be worked with, number to insert, starting position in list to consider, ending position which has to be considered.

**bisect’s operation is faster than calling a list’s sort method after each insertion.**

So in other words the syntax **bisect(list, num, beg, end)** means -

Then index of `num` in `list` i is such that each item in `list[:i]` is less than or equal to `num`, and each item in `list[i:]` is greater than `num`. `list` must be a sorted sequence. For any sorted sequence seq, `list[bisect(list,y)-1]==y` is equivalent to `y` in `list`, but is faster if `len(list)` is large. You may pass optional arguments lo and hi to operate on the slice `list[lo:hi]`.

```
from bisect import bisectfrom bisect import bisect_leftli = [1, 3, 4, 4, 4, 6, 7]print("The rightmost index to insert, so list remains sorted is  : ", end="")print(bisect(li, 4))# The rightmost index to insert, so list remains sorted is  : 5print("The leftmost index to insert, so list remains sorted is  : ", end="")print(bisect_left(li, 4))# The leftmost index to insert, so list remains sorted is  : 2
```

```
import randomfrom bisect import bisectdef pick_a_number_from_list(num_list, n=100):    # first calculate the cumulative sum of the list    # i.e.  transform the array into an array of cumulative sums:    # e.g. [10 60 5 25]  --> [10 70 75 100]    cumulative_sum_list = []    j = 0    for i in range(0, len(num_list)):        j += num_list[i]        cumulative_sum_list.append(j)    # If A = [1, 5, 27, 6, 13, 28, 100, 45, 10, 79]    # cumulative_sum_list = [1, 6, 33, 39, 52, 80, 180, 225, 235, 314]    result_list = []    for _ in range(n):        random_num = random.random()        # This will output a number between 0 and 1        # specifically [0.0, 1.0)        # Now pick a random number in the range from zero up to the cumulative total        # (in the example: 0 <= x < 314).        # Then, use bisection on the cumulative array to locate        # the index into the original array:        item_index = bisect(cumulative_sum_list, random_num * cumulative_sum_list[-1])        result_list.append(num_list[item_index])    return result_listdef sampling_based_on_magnitude(num_list):    for i in range(1, 100):        number = pick_a_number_from_list(num_list)        print(number)A = [1, 5, 27, 6, 13, 28, 100, 45, 10, 79]# sampling_based_on_magnitude(A)''' Would print like below[100, 5, 79, 100, 100, 100, 100, 27, 45, 28, 79, 79, 100, 100, 28, 27, 100, 79, 6, 28, 27, 13, 13, 45, 45, 100, 13, 6, 27, 100, 6, 79, 100, 27, 10, 100, 79, 45, 28, 13, 5, 28, 45, 45, 28, 45, 79, 79, 6, 79, 5, 45, 79, 6, 45, 79, 100, 45, 100, 27, 79, 13, 45, 28, 79, 100, 100, 79, 28, 100, 100, 100, 100, 45, 13, 79, 79, 79, 28, 27, 27, 100, 79, 45, 100, 79, 45, 100, 45, 28, 100, 100, 45, 79, 100, 100, 45, 45, 79, 100][79, 100, 45, 100, 79, 45, 79, 100, 27, 100, 100, 10, 100, 79, 79, 79, 100, 10, 45, 79, 45, 100, 100, 27, 100, 100, 13, 45, 100, 45, 79, 13, 45, 79, 27, 100, 45, 100, 45, 100, 45, 45, 100, 100, 100, 79, 45, 100, 100, 79, 100, 100, 100, 100, 79, 28, 79, 79, 100, 45, 100, 79, 100, 79, 79, 45, 45, 100, 28, 100, 45, 79, 13, 100, 79, 100, 100, 79, 100, 6, 27, 79, 100, 79, 100, 45, 79, 79, 28, 79, 28, 100, 28, 45, 10, 27, 79, 79, 13, 100]...'''
```

```
' Would print like below\n\n[100, 5, 79, 100, 100, 100, 100, 27, 45, 28, 79, 79, 100, 100, 28, 27, 100, 79, 6, 28, 27, 13, 13, 45, 45, 100, 13, 6, 27, 100, 6, 79, 100, 27, 10, 100, 79, 45, 28, 13, 5, 28, 45, 45, 28, 45, 79, 79, 6, 79, 5, 45, 79, 6, 45, 79, 100, 45, 100, 27, 79, 13, 45, 28, 79, 100, 100, 79, 28, 100, 100, 100, 100, 45, 13, 79, 79, 79, 28, 27, 27, 100, 79, 45, 100, 79, 45, 100, 45, 28, 100, 100, 45, 79, 100, 100, 45, 45, 79, 100]\n[79, 100, 45, 100, 79, 45, 79, 100, 27, 100, 100, 10, 100, 79, 79, 79, 100, 10, 45, 79, 45, 100, 100, 27, 100, 100, 13, 45, 100, 45, 79, 13, 45, 79, 27, 100, 45, 100, 45, 100, 45, 45, 100, 100, 100, 79, 45, 100, 100, 79, 100, 100, 100, 100, 79, 28, 79, 79, 100, 45, 100, 79, 100, 79, 79, 45, 45, 100, 28, 100, 45, 79, 13, 100, 79, 100, 100, 79, 100, 6, 27, 79, 100, 79, 100, 45, 79, 79, 28, 79, 28, 100, 28, 45, 10, 27, 79, 79, 13, 100]\n...\n'
```

#### Q: Replace the digits in the string with Hash symbol (#) and remove non-digits

Replace the digits in the string with # consider a string that will have digits in that, we need to remove all the not digits and replace the digits with #

Ex 1: A = 234 Output: ### Ex 2: A = a2b3c4 Output: ### Ex 3: A = abc Output: (empty string) Ex 5: A = #2a$#b%c%561# Output: ####

```
import redef replace_digits_and_remove_all_non_digits(str):    replacements = [        ('\D', '',),        ('\d', '#'),    ]    for old, new in replacements:        str = re.sub(old, new, str)    return str# Use-casess1 = "a2b3c4"s2 = "234"s3 = 'abc's4 = '#2a$#b%c%561#'print(replace_digits_and_remove_all_non_digits(s1))print(replace_digits_and_remove_all_non_digits(s2))print(replace_digits_and_remove_all_non_digits(s3))print(replace_digits_and_remove_all_non_digits(s4))
```

```
##########
```

#### Q: Students marks dashboard

consider the marks list of class students given two lists

Students = ‘student1’,’student2',’student3',’student4',’student5',’student6',’student7',’student8',’student9',’student10'

Marks = 45, 78, 12, 14, 48, 43, 45, 98, 35, 80

from the above two lists the Student0 got Marks0, Student1 got Marks1 and so on

your task is to print the name of students

*   a. Who got top 5 ranks, in the descending order of marks
*   b. Who got least 5 ranks, in the increasing order of marks
*   c. Who got marks between >25th percentile <75th percentile, in the increasing order of marks

Ex 1: Students=\[‘student1’,’student2',’student3',’student4',’student5',’student6',’student7',’student8',’student9',’student10'\] Marks = \[45, 78, 12, 14, 48, 43, 47, 98, 35, 80\]

a. student8 98 student10 80 student2 78 student5 48 student7 47

b. student3 12 student4 14 student9 35 student6 43 student1 45

c. student9 35 student6 43 student1 45 student7 47 student5 48

#### Solution and Explanations and Notes on methods, steps, algorithms

#### Note on zip() function

Python’s zip() function is defined as zip(\*iterables). The function takes in iterables as arguments and returns an iterator. This iterator generates a series of tuples containing elements from each iterable. zip() can accept any type of iterable, such as files, lists, tuples, dictionaries, sets, and so on.

```
numbers = [1, 2, 3]letters = ['a', 'b', 'c']zipped = zip(numbers, letters)print(zipped)  # <zip object at 0x7f04f57a2c40>print(list(zipped))  # [(1, 'a'), (2, 'b'), (3, 'c')]
```

undefined
undefined

[Photo Source](https://www.educative.io/edpresso/what-is-the-zip-function-in-python)

If the passed iterables are of different lengths, then the returned iterator takes the length of the shortest iterator passed to the function.

#### Note on slice syntax (selecting items from list with colon)

`:` is the delimiter of the slice syntax to 'slice out' sub-parts in sequences , `[start:end]`

```
list[start:stop]  # items start through stop-1list[start:]      # items start through the rest of the arraylist[:stop]       # items from the beginning through stop-1list[:]           # a copy of the whole array
```

Which means

```
[1:5] is equivalent to "from 1 to 5" (5 not included)[1:] is equivalent to "1 to end"[len(a):] is equivalent to "from length of a to end"
```

```
import mathdef display_dash_board(marks, students):    # zip() method - will bind together corresponding elements of students and marks    marks_to_students_zipped = list(zip(marks, students))    # print(marks_to_students_zipped)    # [(45, 'student1'), (78, 'student2'), (12, 'student3'), (14, 'student4'), (48, 'student5'), (43, 'student6'), (47, 'student7'), (98, 'student8'), (35, 'student9'), (80, 'student10')]    marks_to_students_zipped.sort(key=lambda item: item[0])    # The "key" above specifies what criteria it should sort on    # I am selecting [0] as it should be sorted on marks    top_5_ranks_by_marks = least_5_ranks_by_marks = marks_to_students_zipped    if len(marks_to_students_zipped) > 5:        top_5_ranks_by_marks = marks_to_students_zipped[:-6: -1]        # Above slice notation means -  the last 5 items, reversed        least_5_ranks_by_marks = marks_to_students_zipped[:5]    students_between_25_and_75_percentile = marks_to_students_zipped[math.ceil(0.25 * len(marks_to_students_zipped)):                                                                     math.floor(0.75 * len(marks_to_students_zipped))]    return top_5_ranks_by_marks, least_5_ranks_by_marks, students_between_25_and_75_percentileStudents = ['student1', 'student2', 'student3', 'student4', 'student5', 'student6', 'student7', 'student8', 'student9',            'student10']Marks = [45, 78, 12, 14, 48, 43, 47, 98, 35, 80]display_dash_board(Marks, Students)
```

```
([(98, 'student8'),  (80, 'student10'),  (78, 'student2'),  (48, 'student5'),  (47, 'student7')], [(12, 'student3'),  (14, 'student4'),  (35, 'student9'),  (43, 'student6'),  (45, 'student1')], [(43, 'student6'), (45, 'student1'), (47, 'student7'), (48, 'student5')])
```

#### Q: Closest points (based on Cosine Distance)

Find the closest points (based on cosine distance) in S (which is n data points in the form of list of tuples) from P (which is a point `P=(p,q)` )

Consider you have given n data points in the form of list of tuples like

`S=[(x1,y1),(x2,y2),(x3,y3),(x4,y4),(x5,y5),..,(xn,yn)]` and a point `P=(p,q)`

Your task is to find 5 closest points(based on cosine distance) in S from P

cosine distance between two points (x,y) and (p,q) is defined as

![](https://cdn-images-1.medium.com/max/800/1*uloe7_EHb5VzLzxebMGEgg.png)
undefined

Ex:

S= \[(1,2),(3,4),(-1,1),(6,-7),(0, 6),(-5,-8),(-1,-1)(6,0),(1,-1)\] P= (3,-4)

undefined
undefined

Output: (6,-7) (1,-1) (6,0) (-5,-8) (-1,-1)

#### Solution and Explanations and Notes on methods, steps, algorithms

Cosine Similarity and Cosine Distance are heavily used in recommendation systems to recommend products to the users based on their likes and dislikes. For example, Amazon, Flipkart, and similar Companies use it to recommend items to customers for a personalized experience, Movies rating and recommendation, etc.

#### Notes on Cosine-Distance concept

The cosine distance measures the angular cosine distance between vectors a and b. While cosine similarity measures the similarity between two vectors of an inner product space. It is measured by the cosine of the angle between two vectors and determines whether two vectors are pointing in roughly the same direction.

#### A comparison between Euclidean distance (d) and cosine similarity (θ).

undefined
undefined

The distance d above is euclidean distance. Which can be measured as below

#### √{(x2-x1)² + (y2-y1)²}

But cosine considers the angle between vectors (thus not taking into regard their weight or magnitude).

undefined
undefined

[Source of above image](https://medium.com/datadriveninvestor/cosine-similarity-cosine-distance-6571387f9bf8)

For two vectors that are completely identical, the cosine similarity will be 1. For vectors that are completely unrelated, this value will be 0. If there is an opposite relationship between the two vectors, this time the cosine similarity value will be -1. (cos0 = 1, cos90 = 0, cos180 = -1)

undefined
undefined

```
import mathdef closest_points_to_p(S, P):    total_list_of_closest_points_to_p = []    result_closest_to_p = []    for point in S:        denominator = math.sqrt((point[0] ** 2) + (point[1] ** 2)) * math.sqrt((P[0] ** 2) + (P[1] ** 2))        numerator = point[0] * P[0] + point[1] * P[1]        if denominator != 0:            cosine_distance_for_this_point = math.acos(numerator / denominator)            total_list_of_closest_points_to_p.append((cosine_distance_for_this_point, point))    # print('total_list_of_closest_points_to_p ', total_list_of_closest_points_to_p)    # [(2.0344439357957027, (1, 2)), (1.8545904360032246, (3, 4)), (2.9996955989856287, (-1, 1)), (0.06512516333438509, (6, -7)), (2.498091544796509, (0, 6)), (1.2021004241368467, (-5, -8)), (1.4288992721907328, (-1, -1)), (0.9272952180016123, (6, 0)), (0.14189705460416438, (1, -1))]    # So total_list_of_closest_points_to_p is an list of tuples and the first element    # of each of the tuple is the actual 'cosine distance' . So I need to sort    # total_list_of_closest_points_to_p by this first element    for item in sorted(total_list_of_closest_points_to_p, key=lambda x: x[0])[:5]:        # print('item is', item)        # (0.06512516333438509, (6, -7))        # (0.14189705460416438, (1, -1))        # ...        result_closest_to_p.append(item[1])    return result_closest_to_pS = [(1, 2), (3, 4), (-1, 1), (6, -7), (0, 6), (-5, -8), (-1, -1), (6, 0), (1, -1)]P = (3, -4)closest_points = closest_points_to_p(S, P)print("5 Closest point to P based on cosine-distance:", *[point for point in closest_points], sep="\n")
```

```
5 Closest point to P based on cosine-distance:(6, -7)(1, -1)(6, 0)(-5, -8)(-1, -1)
```

#### Twin Prime Numbers

#### Write a program to print twin primes less than 1000. If two consecutive odd numbers are both prime then they are known as twin primes

*   Twin primes are pairs of primes which differ by two
*   Example of first series of twin primes are {3,5}, {5,7}, {11,13} and {17,19}.

#### Solution Steps / Algo

*   First I will write the function to determine if a number is Prime
*   And then use that function to go over the range staring from 2 upto the given number
*   1 is not considered a Prime and 2 is considered the first Prime

```
import mathdef is_prime(number):    '''Function to check is a number is a Prime number.    Returns true if Prime else returns False'''    if number == 1: return False    if number == 2 or number == 3: return True    if number > 2 and number % 2 == 0: return False    max_number_to_check = int(math.sqrt(number) + 1)    # Now iterate over a range staring at 3 and ending at max_number_to_check    # and only for odd numbers, which I am making sure by adding the step 2    # to the range() function    for num in range(3, max_number_to_check, 2):        if number % num == 0:            return False    return Truedef get_prime_twins(number):    for num1 in range(2, number):        num2 = num1 + 2        if is_prime(num1) and is_prime(num2):            print(num1, num2)print(get_prime_twins(1000))
```

```
3 55 711 1317 1929 3141 4359 6171 73101 103107 109137 139149 151179 181191 193197 199227 229239 241269 271281 283311 313347 349419 421431 433461 463521 523569 571599 601617 619641 643659 661809 811821 823827 829857 859881 883None
```

#### Find all prime factors of a number.

Example: prime factors of 56 are

2, 2, 2, 7

```
import mathdef get_prime_factors(number):    # create an empty list to hold all the prime factors    prime_factors = []    # First get the number of two's that divide number    # i.e the number of 2's that are in the factors    while number % 2 == 0:        prime_factors.append(2)        number = number / 2    # After the above while loop, when number has been    # divided by all the 2's - so the number must be odd at this point    # Otherwise it would be perfectly divisible by 2 another time    # so now that its odd I can skip 2 ( i = i + 2) for each increment    for i in range(3, int(math.sqrt(number)) + 1, 2):        while number % i == 0:            prime_factors.append(int(i))            number = number / i    # If n has NOT become 1 after the previous 2 while-loop, that means that    # The remaining n is the last prime factor which I have to append to the list    if number > 1:        prime_factors.append(int(number))    return prime_factorsprint(get_prime_factors(84))
```

```
[2, 2, 3, 7]
```

#### Implement formulae of permutations and combinations.

Number of permutations of n objects taken r at a time: p(n, r) = n! / (n-r)!

```
from math import factorial# With built-in math.factorial functiondef p(n, r):    return int(factorial(n) / factorial(n - r))def c(n, r):    return int(factorial(n) / (factorial(r) * factorial(n - r)))print(p(7, 3))print(c(7, 3))# Without using math.factorialdef get_factorial(number):    if number == 0:        return 1    else:        return number * factorial(number - 1)def permutation(n, r):    return int(get_factorial(n) / get_factorial(n - r))def combination(n, r):    return int(factorial(n) / (factorial(r) * factorial(n-r)))print(permutation(7, 3))print(combination(7, 3))
```

```
2103521035
```

#### Converts a decimal number to binary number

#### Approach to the solution below — Recursive — Algorithm

The decimal number in the argument is considered as the dividend.

Divide this decimal number by 2 (as 2 is base of binary).

Store the remainder after above division

Repeat the above steps till the number becomes zero

Return the stored remainder in reverse order

```
def decimal_to_binary(number):    if number == 0:        return ''    else:        return decimal_to_binary(number // 2) + str(number % 2)print(decimal_to_binary(112))
```

```
1110000
```

#### Cube function and find Armstrong Number

Write a function cubesum() that accepts an integer and returns the sum of the cubes of individual digits of that number. Use this function to make functions PrintArmstrong() and isArmstrong() to print Armstrong numbers and to find whether is an Armstrong number.

#### Definition of Armstrong number

A positive integer is an Armstrong number of order n if

`abcd... = a^n + b^n + c^n + d^n + ...`

e.g. 153 is an armstrong number of order 3 because the following holds true

153 = 1_1_1 + 5_5_5 + 3_3_3

```
def cubesum(number):    cube_sum = 0    while number > 0:        digit = int(number % 10)        cube_sum += digit * digit * digit        number /= 10    return cube_sumprint(cubesum(153))def is_armstrong(number):    original_num_1 = original_num_2 = number    # Below to hold the power to which all the    # individual digits will be raised    no_of_digits = 0    power_sum_of_digits = 0    while number > 0:        no_of_digits += 1        number //= 10    while original_num_1 > 0:        this_digit = int(original_num_1 % 10)        power_sum_of_digits += this_digit ** no_of_digits        original_num_1 //= 10    if original_num_2 == power_sum_of_digits:        print(original_num_2, " is an Armstrong number of order ", no_of_digits)    else:        print (original_num_2, ' is not an Armstrong number')is_armstrong(153)is_armstrong(123)is_armstrong(370)is_armstrong(1634)is_armstrong(8208)def print_armstrong(n1, n2):    for i in range (n1, n2):        # this i is the number that I will be checking        # if its an armstrong number        no_of_digits = 0        this_number = i        # while loop to calculate no of no_of_digits        # this_number has        while this_number > 0:            no_of_digits += 1            this_number //= 10        # while loop to calculate power_sum_of_digits        # of this_number        this_number_2 = i        power_sum_of_digits = 0        while this_number_2 > 0:            # print(this_number_2)            this_digit = this_number_2 % 10            power_sum_of_digits += this_digit ** no_of_digits            this_number_2 //= 10        # Now compare i with the power_sum        if power_sum_of_digits == i:            print(str(i))print_armstrong(100, 400)print_armstrong(8000, 9000)
```

```
153153  is an Armstrong number of order  3123  is not an Armstrong number370  is an Armstrong number of order  31634  is an Armstrong number of order  48208  is an Armstrong number of order  41533703718208
```

#### Product of digits of a number.

Write a function prod\_digits() that inputs a number and returns the product of digits of that number.

```
def prod_digits(num):    result = 1    while num > 0:        result *= num % 10        num //= 10    return resultprint(prod_digits(123))print(prod_digits(452))
```

```
640
```

#### Multiplicative digital root and multiplicative persistence

If all digits of a number n are multiplied by each other repeating with the product, the one digit number obtained at last is called the multiplicative digital root of n. The number of times digits need to be multiplied to reach one digit is called the multiplicative persistence of n.

Example: 86 -> 48 -> 32 -> 6 (MDR 6, MPersistence 3) 341 -> 12->2 (MDR 2, MPersistence 2)

Using the function prod\_digits() of previous exercise write functions MDR() and MPersistence() that input a number and return its multiplicative digital root and multiplicative persistence respectively

```
def MDR(num):    product = prod_digits(num)    while product >= 10:        product = prod_digits(product)    return productprint(MDR(86))print(MDR(341))# The number of times digits need to be multiplied# to reach one digit is called the multiplicative persistence of n.def MPersistence(num):    multiplicative_persistence = 1    product = prod_digits(num)    while product >= 10:        multiplicative_persistence +=1        product = prod_digits(product)    return multiplicative_persistenceprint(MPersistence(86))print(MPersistence(341))
```

```
6232
```

#### Finds the sum of proper divisors of a number

Write a function sumPdivisors() that finds the sum of proper divisors of a number. Proper divisors of a number are those numbers by which the number is divisible, except the number itself. For example proper divisors of 36 are 1, 2, 3, 4, 6, 9, 12, 18 The sum of which is 55

```
from math import sqrtdef sumPdivisors(num):    sum_of_divisors = 0    for divisor in range(2, int(sqrt(num)) +1 ):        if num % divisor == 0:            # if for a number there are 2 equal divisor include only one            # e.g. 9 / 3 = 3            # We have to do this adjustment as we are only checking till the            # sqrt(num). e.g for num = 36 when I find 4, I will also add 9            # because otherwise I am not checking for 9 as the range will be            # till the sqrt(36) i.e. 6            if divisor == (num / divisor):                sum_of_divisors += divisor            else:                # else add both e.g.                # when dividing 36 / 4 add both 4 and 9 to the sum                sum_of_divisors += divisor + (num / divisor)    return sum_of_divisors + 1    # Adding 1 to account for the number 1    # As 1 is a divisor for all numbers# print(sumPdivisors(36))# print(sumPdivisors(28))print(sumPdivisors(220))print(sumPdivisors(284))
```

```
284.0220.0
```

#### Find Perfect Number

A number is called perfect if the sum of proper divisors of that number is equal to the number. For example 28 is a perfect number, since 1+2+4+7+14=28. Write a program to print all the perfect numbers in a given range

```
from math import sqrtdef perfect_number(n1, n2):    for num in range(n1, n2 + 1):        sum_of_divisor = 0        for divisor in range(2, int(sqrt(num)) + 1):            if num % divisor == 0:                if divisor == (num / divisor):                    sum_of_divisor += divisor                else:                    sum_of_divisor += divisor + (num/divisor)        if sum_of_divisor + 1 == num:             print(num)perfect_number(10, 500)
```

```
28496
```

#### Find Amicable Number

Two different numbers are called amicable numbers if the sum of the proper divisors of each is equal to the other number. For example, 220 and 284 are the smallest amicable numbers.

Sum of proper divisors of 220 = 1+2+4+5+10+11+20+22+44+55+110 = 284 Sum of proper divisors of 284 = 1+2+4+71+142 = 220

Write a function to print pairs of amicable numbers in a range

Similarly, 1184 and 1210 are an amicable pair

```
from math import sqrtdef sum_proper_divisors(num):    # sum_of_divisors = 0    #    # for divisor in range(2, int(sqrt(num)) +1 ):    #     if num % divisor == 0:    #         if divisor == (num / divisor):    #             sum_of_divisors += divisor    #         else:    #             sum_of_divisors += divisor + (num / divisor)    #    # return sum_of_divisors + 1    sum_of_divisors = 0    for divisor in range(2, int(sqrt(num)) +1 ):        if num % divisor == 0:            # if for a number there are 2 equal divisor include only one            # e.g. 9 / 3 = 3            # We have to do this adjustment as we are only checking till the            # sqrt(num). e.g for num = 36 when I find 4, I will also add 9            # because otherwise I am not checking for 9 as the range will be            # till the sqrt(36) i.e. 6            if divisor == (num / divisor):                sum_of_divisors += divisor            else:                # else add both e.g.                # when dividing 36 / 4 add both 4 and 9 to the sum                sum_of_divisors += divisor + (num / divisor)    return int(sum_of_divisors + 1)def is_amicable(n1, n2):    if sum_proper_divisors(n1) == n2 and sum_proper_divisors(n2) == n1:        return True    else:        return Falsedef get_amicable(n1, n2 ):    result = []    for i in range(n1, n2+1):        for j in range(n1+1, n2+1):            if i != j:                if is_amicable(i, j):                    result.append(tuple(sorted((i,j))))    # the above list will contain duplicates as the same    # set of numbers will appear twice as the loop progresses    # so now just remove duplicates from the above list    return list(set(result))print(get_amicable(100, 1250))
```

```
[(220, 284), (1184, 1210)]
```

#### Q — Find whether a given line (equation in x and y ) is able to separate the two lists of points (tuples) successfully

consider you have given two set of data points in the form of list of tuples like

```
Red =[(R11,R12),(R21,R22),(R31,R32),(R41,R42),(R51,R52),..,(Rn1,Rn2)]Blue=[(B11,B12),(B21,B22),(B31,B32),(B41,B42),(B51,B52),..,(Bm1,Bm2)]
```

and set of line equations(in the string format, i.e list of strings)

```
Lines = [a1x+b1y+c1, a2x+b2y+c2, a3x+b3y+c3, a4x+b4y+c4,.., K lines]
```

Note: you need to do string parsing here and get the coefficients of x,y and intercept

your task is for each line that is given print “YES”/”NO”, you will print yes, if all the red points are one side of the line and blue points are other side of the line, otherwise no

Example :-

Red= \[(1,1),(2,1),(4,2),(2,4), (-1,4)\] Blue= \[(-2,-1),(-1,-2),(-3,-2),(-3,-1),(1,-3)\]

Lines=\[“1x+1y+0”,”1x-1y+0",”1x+0y-3",”0x+1y-0.5"\]

undefined
undefined

#### Output:

YES NO NO YES

#### Solution and Explanations and Notes on methods, steps, algorithms

Math Theory — If the line equation is y=ax+b and the coordinates of a point is (x\_0,y\_0) then compare y\_0 and ax\_0+b, for example if y\_0>ax\_0+b then the point is above the line, etc.

In this particular case —

#### The Mathematics to determine if 2 points are on opposite sides of a line

![](https://cdn-images-1.medium.com/max/800/1*tQHC4fb03zgX1teD-0H5-A.png)
undefined

[Source](https://math.stackexchange.com/a/162730/517433)

So I take the equation of the line, say “1x+1y+0”

Now consider 2 points (1,1) and (-6,-1)

For point (1, 1) => 1(1)+ 1(1) = 2 which is > 0

For point (-6, -1) = 1(-6)+(1)(-1) = -7 which is < 0

Therefore, we can conclude that (1,1) and (-6,-1) lie on different sides of the line S.

Now in the given problem, given an equation — all red should be on one side of the equation and blue on the other side.

#### General Mathematical Principle

#### Here’s the Algorithm I will follow for the below Python Implementation

*   The question already mentioned — “you need to do string parsing here and get the coefficients of x,y and intercept”
*   I know the string in the Equation will be of the form `some_number*x` and `some_number*y`, where the 'some\_number' is the coefficients of x and y. So I need to break the string up in such a way that I am only left with just the numbers, and then replace the value of x and y in the equation with the value of the points that I am evaluating.
*   Initiate a variable `sign_of_equation_with_red_point_tuple` to be minus 1 (-1)
*   Take the first tuple of the Red\_side list (i.e. red\[0\]\[0\] and red[0](http://en.wikipedia.org/wiki/Binary_search_algorithm) ) and replace then into the Equation. If sign of Equation becomes positive with this, then change `sign_of_equation_with_red_point_tuple` to be plus 1 ( 1 )
*   Now assuming `sign_of_equation_with_red_point_tuple` to be plus one ( 1 ) do the below step
*   One by one, check for all the Red side’s tuples (points) — if Equation sign becomes negative for any point > That means that single point is on the other side of the Line Equation.
*   So in that case, then return ‘NO’ from the function.
*   Similarly in the next step, assuming `sign_of_equation_with_red_point_tuple` to be negaive 1 (-1), do the below step
*   Ony by one, check for all the Red side’s tuples (points) — if Equation sign becomes positive for any point > That means that single point is on the other side of the Line Equation.
*   So in that case, then return ‘NO’ from the function.
*   Do both the above steps for the Blue side as well.
*   And if none of the above 4 steps returns a ‘NO’ ans, then finally return ‘YES’ from the function.

```
import mathdef i_am_the_one(red, blue, line):    sign_of_equation_with_red_point_tuple = -1    if eval(line.replace('x', '*%s' % red[0][0]).replace('y', '*%s' % red[0][1])) > 0:        sign_of_equation_with_red_point_tuple = 1    for red_point in red:        if sign_of_equation_with_red_point_tuple == 1 and eval(                line.replace('x', '*%s' % red_point[0]).replace('y', '*%s' % red_point[1])) < 0:            return 'NO'        if sign_of_equation_with_red_point_tuple == -1 and eval(                line.replace('x', '*%s' % red_point[0]).replace('y', '*%s' % red_point[1])) > 0:            return 'NO'    # Now the same set of two traversal for the blue-points tuples    sign_of_equation_with_blue_point_tuple = -1 * sign_of_equation_with_red_point_tuple    for blue_point in blue:        if sign_of_equation_with_blue_point_tuple == 1 and eval(                line.replace('x', '*%s' % blue_point[0]).replace('y', '*%s' % blue_point[1])) < 0:            return 'NO'        if sign_of_equation_with_blue_point_tuple == -1 and eval(                line.replace('x', '*%s' % blue_point[0]).replace('y', '*%s' % blue_point[1])) > 0:            return 'NO'    return 'YES'Red = [(1, 1), (2, 1), (4, 2), (2, 4), (-1, 4)]Blue = [(-2, -1), (-1, -2), (-3, -2), (-3, -1), (1, -3)]Lines = ["1x+1y+0", "1x-1y+0", "1x+0y-3", "0x+1y-0.5"]for i in Lines:    yes_or_no = i_am_the_one(Red, Blue, i)    print(yes_or_no)
```

```
YESNONOYES
```

#### Q — Filling the missing values in the specified format

You will be given a string with digits and ‘\_’(missing value) symbols you have to replace the ‘\_’ symbols as explained

Ex 1: \_, \_, \_, 24 ==> 24/4, 24/4, 24/4, 24/4 i.e we. have distributed the 24 equally to all 4 places

Ex 2: 40, \_, \_, \_, 60 ==> (60+40)/5,(60+40)/5,(60+40)/5,(60+40)/5,(60+40)/5 ==> 20, 20, 20, 20, 20 i.e. the sum of (60+40) is distributed equally to all 5 places

Ex 3: 80, \_, \_, \_, \_ ==> 80/5,80/5,80/5,80/5,80/5 ==> 16, 16, 16, 16, 16 i.e. the 80 is distributed qually to all 5 missing values that are right to it

Ex 4: \_, \_, 30, \_, \_, \_, 50, \_, \_

\==> we will fill the missing values from left to right a. first we will distribute the 30 to left two missing values (10, 10, 10, \_, \_, \_, 50, \_, \_) b. now distribute the sum (10+50) missing values in between (10, 10, 12, 12, 12, 12, 12, \_, _) c. now we will distribute 12 to right side missing values (10, 10, 12, 12, 12, 12, 4, 4, 4) for a given string with comma seprate values, which will have both missing values numbers like ex: “_, \_, x, \_, \_, \_” you need fill the missing values Q: your program reads a string like ex: “, , x, , , \_” and returns the filled sequence

Ex:

Input1: “_,_,\_,24” Output1: 6,6,6,6

Input2: “40,_,_,\_,60” Output2: 20,20,20,20,20

Input3: “80,_,_,_,_” Output3: 16,16,16,16,16

Input4: “_,_,30,_,_,_,50,_,\_” Output4: 10,10,12,12,12,12,4,4,4

#### So the rule for calculation of missing values is -

1.  Wherever there is missing values, I have to add the immediately proceeding value and the immediately next available value and then divide it by the number of missing values. And then Replace the available value with the calculated value as above.
2.  For the next missing value, calculation, for the immediately preceding available value take the calculated value as explained above.

```
def curve_smoothing(string):    index_of_non_empty_cells_list = []    split_string = string.split(',')    # print(split_string)    # ['_', '_', '30', '_', '_', '_', '50', '_', '_']    for idx in range(len(split_string)):        if split_string[idx] != '_':            index_of_non_empty_cells_list.append(idx)    # Also add to index_of_non_empty_cells_list    # The total index-length of the original list    index_of_non_empty_cells_list.append(len(split_string) - 1)    print(index_of_non_empty_cells_list)    [2, 6, 8]    # Now loop over index_of_non_empty_cells_list to modify the    # original list by filing up empty_cells    position_to_start = 0    for element in index_of_non_empty_cells_list:        # Wherever, there is a missing values, I have to add the immediately proceeding value        # and the immediately next available value and then divide it by the number of missing values.        # So, initiate a variable to 'cumulative_sum_prev_and_next_value' to hold the cumulative sum of        # available (i.e. non-empty) cells        cumulative_sum_prev_and_next_value = int(split_string[element]) if split_string[element] != '_' else 0        cumulative_sum_prev_and_next_value += int(split_string[position_to_start]) if split_string[                                                                                          position_to_start] != '_' and position_to_start != element else 0        # print(cumulative_sum_prev_and_next_value)        # Now divide the cumulative_sum of prev and next value        # by the number of missing values        integer_to_replace_each_previous_empty_cell = cumulative_sum_prev_and_next_value // (                element - position_to_start + 1)        # print("integer_to_replace_each_previous_empty_cell value is ", integer_to_replace_each_previous_empty_cell)        # For the first loop it will give me 10 which is what I will replace each emtpy value with        # And now modify the original list by replacing the empty cells with the above        # value i.e.  integer_to_replace_each_previous_empty_cell        split_string = [            integer_to_replace_each_previous_empty_cell if position_to_start <= x <= element else split_string[x] for x            in range(len(split_string))]        # print("element: %s cumulative_sum_prev_and_next_value: %s  split_string: %s" %(element, cumulative_sum_prev_and_next_value, split_string ))        # After original list has been updated by filling up the empty cells        # now update the starting position for the next loop        position_to_start = element    return split_string#  ------------------------S = "_,_,30,_,_,_,50,_,_"ans = smoothed_values = curve_smoothing(S)# print(curve_smoothing(S))print(ans)
```

```
[2, 6, 8][10, 10, 12, 12, 12, 12, 4, 4, 4]
```

#### Q: Calculate Conditional Probability from 2-D Matrix

You will be given a list of lists, each sublist will be of length 2 i.e. \[\[x,y\],\[p,q\],\[l,m\]..\[r,s\]\] consider its like a martrix of n rows and two columns

1.  the first column F will contain only 5 uniques values (F1, F2, F3, F4, F5)
2.  the second column S will contain only 3 uniques values (S1, S2, S3)

your task is to find

a. Probability of P(F=F1|S==S1), P(F=F1|S==S2), P(F=F1|S==S3)

b. Probability of P(F=F2|S==S1), P(F=F2|S==S2), P(F=F2|S==S3)

c. Probability of P(F=F3|S==S1), P(F=F3|S==S2), P(F=F3|S==S3)

d. Probability of P(F=F4|S==S1), P(F=F4|S==S2), P(F=F4|S==S3)

e. Probability of P(F=F5|S==S1), P(F=F5|S==S2), P(F=F5|S==S3)

Ex:

```
[[F1,S1],[F2,S2],[F3,S3],[F1,S2],[F2,S3],[F3,S2],[F2,S1],[F4,S1],[F4,S3],[F5,S1]]a. P(F=F1|S==S1)=1/4, P(F=F1|S==S2)=1/3, P(F=F1|S==S3)=0/3b. P(F=F2|S==S1)=1/4, P(F=F2|S==S2)=1/3, P(F=F2|S==S3)=1/3c. P(F=F3|S==S1)=0/4, P(F=F3|S==S2)=1/3, P(F=F3|S==S3)=1/3d. P(F=F4|S==S1)=1/4, P(F=F4|S==S2)=0/3, P(F=F4|S==S3)=1/3e. P(F=F5|S==S1)=1/4, P(F=F5|S==S2)=0/3, P(F=F5|S==S3)=0/3
```

#### Explanations and Theory

**Defining Conditional probability** — Conditional probability is the probability of an event given that some condition is taken to be true. Mathematically

`P(A|B) = P(A intersection B) / P(B)`

So here e.g.

`P(F=F1 | S == S2) = P (F1 intersection S2 ) / P (S2)`

If A and B are independent, then P(B|A) = P(B), and P(A|B) = P(A), and in this case I don’t need to do any extra mathematics, and P(A|B) is P(A),

So in that case P(F=F1 | S == S2) would be 1/5.

But here, the assumption is that A and B are NOT independent, the question of conditional probability calculation does not arise at all.

#### Now first let’s check how we are calculating Conditional Probabilities from a list of lists or 2-D Matrix

If we have a list of lists. Let's assume I have two elements F and S like below.

\[F,S\]

And the values they take are

\[\[1,3\],\[1,2\],\[2,4\],\[2,2\],\[3,2\]\]

What is P(F1|S2)?

P(F1)=2/5 (because there are 2 lists containing F=1) of the total 5 lists P(S2)=3/5 (because there are 3 lists containing S=2) of the total 5 lists

Now for P(F1 n S2) it can be said as: **“The Probability that a randomly chosen list has both F=1 and S=2”.**

So P(F1 n S2) = 1/5 (because there is only one list where the list has both F=1 and S=2, out of 5 lists)

So Now P(F=F1|S==S2)

\= P(F1 intersection S2 ) / P (S2)

\= (1/5) / (3 / 5 ) = ( 1 / 5 ) \* ( 5 / 3 ) = 1 / 3

**In other words P(F1|S2) can also be expressed as** P(F1|S2) = 1/3 ( since given S=S2, means there are only 3 lists, and out of which F=F1 is only 1 list).

From above we can absolutely confirm that F1 and S2 are NOT independent. Because it’s not possible for F1 and S2 to be independent if an outcome of one of the events changes the probability of the other event.

#### Explanations of how P(F=F1|S==S2)=1/3 for our data point given in the question

We know the Math Equation that P(F=F1|S==S2) = P(F1 intersection S2 ) / P (S2)

P(F1 n S2) = 1/10 P(S2) = 3/10.

So P(F=F1|S==S2) = P(F1 intersection S2 ) / P (S2)

\= P(F1 n S2) / P(S2)

\= (1/10) / (3/10)

\= (1/10) \* (10/3) (by the rule of multiplicative inverse)

\= 1/3 = P(F1|S2)

```
from fractions import Fractiondef compute_conditional_probability(A):    # From the given 2-D Matrix A    # Set a dictionary containing the count of all the F and S together    # This dictionary will have as many elements as the given Matrix A    dictionary_f_intersection_s = dict()    for item_list in A:        dict_key = str(item_list[0]) + str(item_list[1])        dictionary_f_intersection_s[dict_key] = 0    # print(dictionary_f_intersection_s)    # {'F1S1': 0, 'F2S2': 0, 'F3S3': 0, 'F1S2': 0, 'F2S3': 0, 'F3S2': 0, 'F2S1': 0, 'F4S1': 0, 'F4S3': 0, 'F5S1': 0}    # Set a dictionary containing the count of all the S only    dictionary_s = {'S1': 0, 'S2': 0, 'S3': 0}    for i in range(len(A)):        f_intersection_s_key = A[i][0] + A[i][1]        dictionary_f_intersection_s[f_intersection_s_key] += 1        s_key = A[i][1]        dictionary_s[s_key] += 1    # print('a. P(F=F1|S==S1)=', Fraction(dictionary_f_intersection_s['F1S1'] / dictionary_s['S1']).limit_denominator(), ' P(F=F1|S==S2)=', Fraction(dictionary_f_intersection_s['F1S2'] / dictionary_s['S2']).limit_denominator(), ' P(F=F1|S==S3)=', Fraction(dictionary_f_intersection_s.get('F1S3', 0) / dictionary_s['S3']).limit_denominator())    idx = 97    print('{0:c}. P(F=F1|S==S1)={F1S1}/{S1}, P(F=F1|S==S2)={F1S2}/{S2}, P(F=F1|S==S3)={F1S3}/{S3}'.format(idx,                                                                                                          F1S1=dictionary_f_intersection_s.get(                                                                                                              'F1S1',                                                                                                              0), S1=                                                                                                          dictionary_s[                                                                                                              'S1'],                                                                                                          F1S2=dictionary_f_intersection_s.get(                                                                                                              'F1S2',                                                                                                              0), S2=                                                                                                          dictionary_s[                                                                                                              'S2'],                                                                                                          F1S3=dictionary_f_intersection_s.get(                                                                                                              'F1S3',                                                                                                              0), S3=                                                                                                          dictionary_s[                                                                                                              'S3']))    print('{0:c}. P(F=F2|S==S1)={F2S1}/{S1}, P(F=F2|S==S2)={F2S2}/{S2}, P(F=F2|S==S3)={F2S3}/{S3}'.format(idx+1,                                                                                                          F2S1=dictionary_f_intersection_s.get(                                                                                                              'F2S1',                                                                                                              0), S1=                                                                                                          dictionary_s[                                                                                                              'S1'],                                                                                                          F2S2=dictionary_f_intersection_s.get(                                                                                                              'F2S2',                                                                                                              0), S2=                                                                                                          dictionary_s[                                                                                                              'S2'],                                                                                                          F2S3=dictionary_f_intersection_s.get(                                                                                                              'F2S3',                                                                                                              0), S3=                                                                                                          dictionary_s[                                                                                                              'S3']))    print('{0:c}. P(F=F3|S==S1)={F3S1}/{S1}, P(F=F3|S==S2)={F3S2}/{S2}, P(F=F3|S==S3)={F3S3}/{S3}'.format(idx + 2,                                                                                                          F3S1=dictionary_f_intersection_s.get(                                                                                                              'F3S1',                                                                                                              0), S1=                                                                                                          dictionary_s[                                                                                                              'S1'],                                                                                                          F3S2=dictionary_f_intersection_s.get(                                                                                                              'F3S2',                                                                                                              0), S2=                                                                                                          dictionary_s[                                                                                                              'S2'],                                                                                                          F3S3=dictionary_f_intersection_s.get(                                                                                                              'F3S3',                                                                                                              0), S3=                                                                                                          dictionary_s[                                                                                                              'S3']))    print('{0:c}. P(F=F4|S==S1)={F4S1}/{S1}, P(F=F4|S==S2)={F4S2}/{S2}, P(F=F4|S==S3)={F4S3}/{S3}'.format(idx + 2,                                                                                                          F4S1=dictionary_f_intersection_s.get(                                                                                                              'F4S1',                                                                                                              0), S1=                                                                                                          dictionary_s[                                                                                                              'S1'],                                                                                                          F4S2=dictionary_f_intersection_s.get(                                                                                                              'F4S2',                                                                                                              0), S2=                                                                                                          dictionary_s[                                                                                                              'S2'],                                                                                                          F4S3=dictionary_f_intersection_s.get(                                                                                                              'F4S3',                                                                                                              0), S3=                                                                                                          dictionary_s[                                                                                                              'S3']))    print('{0:c}. P(F=F5|S==S1)={F5S1}/{S1}, P(F=F5|S==S2)={F5S2}/{S2}, P(F=F5|S==S3)={F5S3}/{S3}'.format(idx + 2,                                                                                                          F5S1=dictionary_f_intersection_s.get(                                                                                                              'F5S1',                                                                                                              0), S1=                                                                                                          dictionary_s[                                                                                                              'S1'],                                                                                                          F5S2=dictionary_f_intersection_s.get(                                                                                                              'F5S2',                                                                                                              0), S2=                                                                                                          dictionary_s[                                                                                                              'S2'],                                                                                                          F5S3=dictionary_f_intersection_s.get(                                                                                                              'F5S3',                                                                                                              0), S3=                                                                                                          dictionary_s[                                                                                                              'S3']))# Implementation of the above functionA = [['F1', 'S1'], ['F2', 'S2'], ['F3', 'S3'], ['F1', 'S2'], ['F2', 'S3'], ['F3', 'S2'], ['F2', 'S1'], ['F4', 'S1'],     ['F4', 'S3'], ['F5', 'S1']]compute_conditional_probability(A)
```

```
a. P(F=F1|S==S1)=1/4, P(F=F1|S==S2)=1/3, P(F=F1|S==S3)=0/3b. P(F=F2|S==S1)=1/4, P(F=F2|S==S2)=1/3, P(F=F2|S==S3)=1/3c. P(F=F3|S==S1)=0/4, P(F=F3|S==S2)=1/3, P(F=F3|S==S3)=1/3c. P(F=F4|S==S1)=1/4, P(F=F4|S==S2)=0/3, P(F=F4|S==S3)=1/3c. P(F=F5|S==S1)=1/4, P(F=F5|S==S2)=0/3, P(F=F5|S==S3)=0/3
```

### Q-9 — Given two sentences S1, S2 find common and uncommon words between them

You will be given two sentences S1, S2 your task is to find a. Number of common words between S1, S2 b. Words in S1 but not in S2 c. Words in S2 but not in S1 Ex:

S1= “the first column F will contain only 5 uniques values” S2= “the second column S will contain only 3 uniques values” Output: a. 7 b. \[‘first’,’F’,’5'\] c. \[‘second’,’S’,’3'\]

def string\_features(S1, S2):  
    s1\_unique\_words\_set = set(S1.split(" "))  
    s2\_unique\_words\_set = set(S2.split(" "))

num\_of\_common\_words = len(s1\_unique\_words\_set.intersection(s2\_unique\_words\_set)),  
    s1\_words\_not\_in\_s2 = s1\_unique\_words\_set - s2\_unique\_words\_set  
    s2\_words\_not\_in\_s1 = s2\_unique\_words\_set - s1\_unique\_words\_set

print('a. ', list(num\_of\_common\_words)\[0\])  
    print('b. ', list(s1\_words\_not\_in\_s2))  
    print('b. ', list(s2\_words\_not\_in\_s1))

\# Implementation  
S1 = "the first column F will contain only 5 uniques values"  
S2 = "the second column S will contain only 3 uniques values"  
string\_features(S1, S2)

\# OUTPUT  
\# a. 7  
\# b. \['first','F','5'\]  
\# c. \['second','S','3'\]

### Q — Implement Log-Loss Function in Plain Python

You will be given a list of lists, each sublist will be of length 2 i.e. \[\[x,y\],\[p,q\],\[l,m\]..\[r,s\]\] consider its like a matrix of n rows and two columns

*   a. the first column Y will contain integer values
*   b. the second column 𝑌𝑠𝑐𝑜𝑟𝑒Yscore will be having float values

Your task is to find the value of the below

![](https://cdn-images-1.medium.com/max/800/0*KV8Z6_Pe_uSakfPQ.png)
undefined

Ex: \[\[1, 0.4\], \[0, 0.5\], \[0, 0.9\], \[0, 0.3\], \[0, 0.6\], \[1, 0.1\], \[1, 0.9\], \[1, 0.8\]\] output: 0.4243099

### Explanations and Notes on Log Loss

Logarithmic Loss (i.e. Log Loss and also same as Cross Entropy Loss), is a classification loss function. Log Loss quantifies the accuracy of a classifier by penalising false classifications. Minimising the Log Loss is basically equivalent to maximising the accuracy of the classifier.

Log loss is used when we have {0,1} response. In these cases, the best models give us values in terms of probabilities. The log loss function is simply the objective function to minimize, in order to fit a log linear probability model to a set of binary labeled examples.

![](https://cdn-images-1.medium.com/max/800/0*NKaQy1Ny5K2wQwrI.png)
undefined

In slightly different form Log is expressed as

![](https://cdn-images-1.medium.com/max/800/0*BztHiuvuEuL-V6LZ.png)
undefined

Log Loss is a slight modification on the Likelihood Function. In fact, Log Loss is -1 \* the log of the likelihood function.

Log Loss measures the accuracy of a classifier. It is used when the model outputs a probability for each class, rather than just the most likely class.

In simple words, log loss measures the UNCERTAINTY of the probabilities of your model by comparing them to the true labels. Let us look closely at its formula and see how it measures the UNCERTAINTY.

Now the question is, your training labels are 0 and 1 but your training predictions are 0.4, 0.6, 0.89, 0.1122 etc.. So how do we calculate a measure of the error of our model ? If we directly classify all the observations having values > 0.5 into 1 then we are at a high risk of increasing the miss-classification. This is because it may so happen that many values having probabilities 0.4, 0.45, 0.49 can have a true value of 1.

**This is where logLoss comes into picture.**

Log-loss is a “soft” measurement of accuracy that incorporates the idea of probabilistic confidence. It is intimately tied to information theory: log-loss is the cross entropy between the distribution of the true labels and the predictions. Intuitively speaking, entropy measures the unpredictability of something. Cross entropy incorporate the entropy of the true distribution, plus the extra unpredictability when one assumes a different distribution than the true distribution. So log-loss is an information-theoretic measure to gauge the “extra noise” that comes from using a predictor as opposed to the true labels. By minimizing the cross entropy, one maximizes the accuracy of the classifier.

### Cases where Log-Loss function can be mostly used

The log loss function is used as an evaluation metric of the ML classifier models. This is an important metric as it is only metric which uses the actual predicted probability for evaluating the model ( ROC — AUC uses the order of the values but not the actual values). This is very useful as this penalizes the model heavily if it is very confident in predicting the wrong class(please check the plot of -log(x)). We optimize our model to minimize the log loss. Hence this metric is very useful in cases where the cost of predicting wrong class is very high. Hence model tries to reduce the probabilities of belonging to wrong class and we can choose the higher threshold of probability to predict the class label. This metric can be used for both binary and multi class classifications. The value of log loss lies between 0 ( including) and infinity. This is the only disadvantage of log loss as it is not very interpretable. We know that the best case would be 0 value of log loss however we can not interpret other values of log loss. For some cases log loss of 1 can be good while for other it may not be good enough. One hack is that we can measure the log loss of random model and try to reduce log-loss of our actual model from this value as much as possible without increasing variance.

Please note that this metric can only be used if our model can predict the probability of each class. Hence for calculating the log loss for the models which don’t provide the probability score, probability calibration methods can be used on top of the base classifier to predict the probability score for each class.

### A use-case of Logloss

Say, I am predicting Cancer from my ML Model. And Suppose you have 5 cases. 2 cases were cancer (y1 = y2 = 1) and 3 cases were benign (y3 = y4 = y5 = 0). Say your model predicted each model has 0.5 probability of cancer. In this case, what we have for log loss is…

−1/5∗(log(0.5)+log(0.5)+(1−0)∗log(1−0.5)+(1−0)∗log(1−0.5)+(1−0)∗log(1−0.5))

Essentially, y\_i and (1 — y\_i) determines which term is to be dropped depending on the ground truth label. Depending on ground truth, either the log (y\_hat) or log(1-y\_hat) will be selected to determines how far away from the truth your model’s generated probability is.

We can use the log\_loss function from scikit-learn, with documentation found [here](http://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html): But here we will implement a pure-python version

Finally Simple Python Implementation

from math import log

def compute\_log\_loss(A):  
    cross\_entropy = 0  
    for row in A:  
        cross\_entropy += (row\[0\] \* log(row\[1\], 10) + ((1 - row\[0\]) \* log(1 - row\[1\], 10)))

log\_loss = -1 \* cross\_entropy / len(A)  
    return log\_loss

A = \[\[1, 0.4\], \[0, 0.5\], \[0, 0.9\], \[0, 0.3\], \[0, 0.6\], \[1, 0.1\], \[1, 0.9\], \[1, 0.8\]\]  
print(compute\_log\_loss(A))  
\# 0.42430993457031635

By [Rohan Paul](https://medium.com/@paulrohan) on [November 23, 2020](https://medium.com/p/8f0affb25217).

[Canonical link](https://medium.com/@paulrohan/pure-python-exercises-8f0affb25217)

Exported from [Medium](https://medium.com) on December 12, 2020.