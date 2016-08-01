# PrimeMultiplicationTable
## Objective
Write a program that prints out a multiplication table of the first 10 prime number. 
 - The program must run from the command line and print one table to STDOU
 - The first row and column of the table should have the 10 primes, with each containing the product of the primes for the corresponding row and column. 
  
> **Note:**
> - Consider complexity. How fast does your code run? How does it scale? 
> - Consider cases where we want ​ N​  primes.
> - Do not use the Prime class from stdlib (write your own code).

## Execute
```
$ cd <project directory>
$ python PrimeMultiplicationTable.py
```

## Result
![image](https://raw.githubusercontent.com/Superbeet/PrimeMultiplicationTable/master/screenshots/screenshot1.PNG)

## My Thinking Process

After review the question, I found it can be divided into two parts. First one is to find the first N primes, second is to generate the table nicely. And how to realize the first part is crucial here.

###Solution 1

The first idea I got is based on the definition of Prime number. 
 
>  *A prime number (or a prime) is a natural number greater than 1 that has no positive divisors other than 1 and itself.* 

  So, basically, I can try to divide each number N by all numbers between 2 and N-1 to see if it can be divided evenly. If it can’t we find a prime. The time complexity of this solution is O(n^2).

Then, I found there are actually some ways to improve the algorithm above.

 - No even number except 2 can be a prime number. So we don’t need to check if a number can be divided evenly by an even number. This fact can reduce half of number we need to try.
 - If a number is not a prime. It should be a multiplication of at least two numbers. Then one of them must be smaller than equal to sqrt(N). So, we just need to try to divide each number by 2 to sqrt(N) instead of N.
 - After optimized the code by the two ideas above, I find there are still some duplicated division operations. For instance, to check 101, we will try dividends 3,5,7,9. However, 9 is not necessary to be checked since 3 has been checked already. So we just need to try to divide the number by all primes we have found so far.

It is implemented in method **get_primes_1** with time complexity **lower than O(n^1.5)**.

###Solution 2

After completing the code, I start thinking if there is better way to solve the problem. Division operations are generally slower than other operations, because it includes a lot of shiftings and subtractions. Then, I come up with a theory I learned before named Sieve of Eratosthenes. It is a typical example to exchange time with space.

Make a list of all the integers less than or equal to n (and greater than one). Strike out the multiples of all primes less than or equal to the square root of n, then the numbers that are left are the primes.

A challenge here is we need to know the upper bound of nth prime firstly. To find it out, I use the Prime Number Theorem. The prime distribution is π(N) ~ N / log(N), where π(N) is the prime-counting function and log(N) is the natural logarithm of N. By using this algorithm, we can approximate the nth prime number P(N)~Nlog(N). Since there will be some deviations (<20%), I expand the table size by 30% to make sure it covers all prime numbers we need.  

It is implemented in method **get_primes_2** with time complexity **O(nloglogn)**.

###Solution 3

Then, I’m thinking if we can move a little bit further to make it become O(n). In solution 2, some composite numbers are struck out more than once. If we can stipulate that each composite number must be struck out by its smallest prime factor, then we can say each composite number will only be touched once, which makes the time complexity become O(n) . 

It is implemented in method **get_primes_3** with time complexity **O(n)**. 

###Future Improvement
We can store prime status in bits rather than bytes (boolean type) to reduce the space complexity. This can also help to reduce the cache miss and improve the performance.

###Scalability
The algorithm can work with larger numbers. The table column width will be adjusted dynamically.  

![image](https://raw.githubusercontent.com/Superbeet/PrimeMultiplicationTable/master/screenshots/screenshot3.PNG)
Here is the result for 30 primes

####**Handle super large number**  
Solution 1 and Solution 2(sieve of Eratosthenes) can be scaled on machine clusters by map-reduce mode. 
In map method code, we divide the candidate numbers into groups and distribute the data to different machines. Then in reduce module, we collect the primes found by each machine and create the result.

## TDD Test Case
I created two types of test cases. 
- First group includes numbers and primes of certain number captured from official prime table 
- Second group includes random number N and the Nth prime. It Avoids putting in super long prime lists andkeeps the file more readable.

## Test Result
![image](https://raw.githubusercontent.com/Superbeet/PrimeMultiplicationTable/master/screenshots/screenshot2.PNG)
