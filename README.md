# Math317Assignment2
#1
First we just used the power series function from Assignment 1. Then we implemented matplotlib and generated points for each iteration, x being the number of iterations, y being the log of the difference in error. 
```
import matplotlib.pyplot as plt
import math
n = 50
def mysin(x, n):
        #First term is x
        sin = 0
        term = x
        #Similar to above, but have to take a step now (only odd number exponents)
        for i in range (1, n+1, 2):
            #Simply the the series formula (multiplying each step by -x^2 and moving
            #the factorial in the denominator along 2 steps
            sin += term 
            term *= -(x * x) / ((i + 1) * (i + 2))
        return sin
    
y_list = []
x_list = []
def mysinplot(x,n):
    for i in range (1, n+1, 2):
        #At some big n, error becomes too small and ln(0) cannot be computed, so Value Error added to stop that.
        try:
            y_list += [(math.log(abs(math.sin((math.pi/2))-mysin((math.pi/2), i))))] 
            x_list += [i]
        except ValueError:
                break

    #Just functions required to generated plot
    plt.plot(x_list,y_list)
    plt.xlabel('X-axis')
    plt.ylabel('Y-axis')
    plt.title("Error per iterations")
    plt.show()
 ```
##2
#We'll simply add two methods of argument reduction. One that reduces arguments to pi/2 using the periodicity of sin: sin(x + 2pi) = sin(x). 

```
x = x % 2*math.pi
```
And one where we use it's symmetry: sin(-x) = -sinx
wow
```
if x < 0:
  x = -x
#And afterwards
if x <0 : 
  return -sin
```
##3
Similarly, we will use the mylog function from Assignment 1 and combine it with an identical plotting function.

```
def mylog(x,n):
  z = x-1
  for i in range(n):
    z = z/(1+math.sqrt(1+z))
  l = z
  for i in range(n):
    l = l*2
  return l

def mylogplot(x,n):
    y_list = []
    x_list = []
    for i in range (1, n, 2):
                #Simply the the series formula (multiplying each step by -x^2 and moving
                #the factorial in the denominator along 2 steps
        try:
            y_list += [(math.log(abs(math.log((math.pi/2))-mylog((math.pi/2), i))))]
            
            x_list += [i]
        except ValueError:
                break
    plt.plot(x_list,y_list)
    plt.xlabel('X-axis')
    plt.ylabel('Y-axis')
    plt.title("A simple line graph")
    plt.show()
```
##4
Here we will look at Mercators log formula and two methods to compute Gregory's.
First let's start with Mercators.
```
log3_mer = 0
term = 1
for i in range(1, 100):
    term = 1/i * ((2/3) ** i)
    log3_mer += term
    #print(term)

print(log3_mer)
```
Then we'll look at the first version of Gregory Series:
```
log3_grog = 0
term = 1
#term = 1/3
for i in range (1, 100, 2):
    term = 2/i * ((1/2) ** i)
    log3_grog += term
print(log3_grog)
```
and a faster version of Gregories where (x+1)/(x-1) = 1/(1+sqrt3) -> x = 1/(1 + 2sqrt(3))
```

```
