# 3x3 Magic Square shenanigans

I was recently doing a challenge on HackerRank, where the goal is, given a 3x3 array of integers in the range [1, 9], to find the minimum total sum of numerical changes to the array, so that it would create a magic square.

## How do we make a magic square?

A magic square is just a square, for which all rows, columns and diagonals sum up to exactly the same magic value. In a 3x3 square, this magic value is 15. We can find this number by noting that the sum is:

1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 = 45

This means that the average sum of three numbers added up is 45 / 3 = 15

[Note: There is actually only "one" 3x3 magic square](https://math.stackexchange.com/a/3379950/899270)


Great, so now we must find all triplet sums which equal 15.

```py
# Python 3 code
import itertools

# [1, 2, 3, 4, 5, 6, 7, 8, 9]
range = range(1, 10)
sums = []

# Generate all possible combinations
c = itertools.combinations(range, 3)

# Loop over every combination and check the sum
for n in c:
    if sum(n) == 15:
        sums.append(n)

# Print result
print(sums)
```
This code returns all triplets:
```
[(1, 5, 9), (1, 6, 8), (2, 4, 9), (2, 5, 8), (2, 6, 7), (3, 4, 8), (3, 5, 7), (4, 5, 6)]
```

Lets start with the middle of the magic square, it must appear 4 times:

<img src="https://user-images.githubusercontent.com/16985019/208937106-8763df81-e73f-4175-bb64-db7ff46b3a03.png" alt="square_0" style="width: 30%;">

The only number that appears in 4 triplets, is **5** and we can lock it in the middle!!

<img src="https://user-images.githubusercontent.com/16985019/208937206-199ec3d3-c069-4b5e-bc33-d498e5f3c93e.png" alt="square_1" style="width: 30%;">

Now for the corners, we need 4 numbers that are seen in 3 triplets:

```
2 - (2, 4, 9), (2, 5, 8), (2, 6, 7)
4 - (2, 4, 9), (3, 4, 8), (4, 5, 6)
6 - (1, 6, 8), (2, 6, 7), (4, 5, 6)
8 - (1, 6, 8), (2, 6, 7), (3, 4, 8)
```
From now on assembling the magic square is pretty trivial, start with a random insertion and then follow it like a puzzle!

<img src="https://user-images.githubusercontent.com/16985019/208937244-04f846d6-12c9-424f-bedb-cdfbe1922291.gif" alt="square_x" style="width: 60%; display: inline-block;" data-target="animated-image.originalImage">

So now we get our perfect square!

<img src="https://user-images.githubusercontent.com/16985019/208937346-06d664f4-f0d1-424b-aa28-ff6b14662de5.png" alt="square_3" style="width: 30%;">

As mentioned before, this is the one and only perfect 3x3 magic square, this means that any other magic square is just a rotation or mirror of the original.

## Actually solving the challenge...

Now... we learned a lot about magic squares, but how do we actually solve the challenge? Well... we now know that there is only one possible magic square. This means that we can simply test the given square against all other possible rotations or mirrors of the magic cube and compare which one has the lowest difference cost!!!

Here is a really cool 2D array rotation function in Python I found on [StackOverflow](https://stackoverflow.com/questions/8421337/rotating-a-two-dimensional-array-in-python) that I wanted to share. But I will try to do all of it with numpy. We rotate the matrix 4 times, and each time create 2 mirrors of the matrix.

```py
import numpy

perfect_square = [[2, 7, 6], [9, 5, 1], [4, 3, 8]]

matrix_list = []

for n in range(4): 
    # Rotate the original 
    perfect_square = numpy.rot90(perfect_square).tolist()

    # Add all rotations and mirrors to a new list
    matrix_list.append(perfect_square)
    matrix_list.append(numpy.flip(perfect_square, 0).tolist())
    matrix_list.append(numpy.flip(perfect_square, 1).tolist())

print(matrix_list)
```
We get this output:
```
[[[6, 1, 8], [7, 5, 3], [2, 9, 4]], [[2, 9, 4], [7, 5, 3], [6, 1, 8]], [[8, 1, 6], [3, 5, 7], [4, 9, 2]], [[8, 3, 4], [1, 5, 9], [6, 7, 2]], [[6, 7, 2], [1, 5, 9], [8, 3, 4]], [[4, 3, 8], [9, 5, 1], [2, 7, 6]], [[4, 9, 2], [3, 5, 7], [8, 1, 6]], [[8, 1, 6], [3, 5, 7], [4, 9, 2]], [[2, 9, 4], [7, 5, 3], [6, 1, 8]], [[2, 7, 6], [9, 5, 1], [4, 3, 8]], [[4, 3, 8], [9, 5, 1], [2, 7, 6]], [[6, 7, 2], [1, 5, 9], [8, 3, 4]]]
```
There are a few duplicate squares in this array, but thats fine for now because we only need to compare each one once :)

Writing the rest was easy, this was a fun challenge!

```py 
def formingMagicSquare(s):
    magic_square_list = [[[6, 1, 8], [7, 5, 3], [2, 9, 4]], [[2, 9, 4], [7, 5, 3], [6, 1, 8]], [[8, 1, 6], [3, 5, 7], [4, 9, 2]], [[8, 3, 4], [1, 5, 9], [6, 7, 2]], [[6, 7, 2], [1, 5, 9], [8, 3, 4]], [[4, 3, 8], [9, 5, 1], [2, 7, 6]], [[4, 9, 2], [3, 5, 7], [8, 1, 6]], [[8, 1, 6], [3, 5, 7], [4, 9, 2]], [[2, 9, 4], [7, 5, 3], [6, 1, 8]], [[2, 7, 6], [9, 5, 1], [4, 3, 8]], [[4, 3, 8], [9, 5, 1], [2, 7, 6]], [[6, 7, 2], [1, 5, 9], [8, 3, 4]]]
    
    # Initiate lowest cost as a big number, because 0 will break the algorithm
    lowest_cost = 100000
    
    for n in magic_square_list:
        cost = 0
        
        # Concatenate both both squares into a single list
        list_1 = s[0] + s[1] + s[2]
        list_2 = n[0] + n[1] + n[2]
        
        # Loop over every single element and calculate the cost of each replacement
        for m in range(9):
            cost += abs(list_1[m] - list_2[m])
        
        if cost < lowest_cost:
            lowest_cost = cost
    
    return lowest_cost
```
