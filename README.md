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
First let's start with Mercator's:
```
log3_mer = 0
term = 1
for i in range(1,100):
    term *= 1/i * (2/3)
    log3_mer += term
    term *= i
print(log3_mer) #1.0986122886681087
```
Here are the first 12 iterations:
```
0.6666666666666666
0.8888888888888888
0.9876543209876543
1.037037037037037
1.0633744855967078
1.0780064014631916
1.0863674962440395
1.0912448015328675
1.0941350565188397
1.0941350565188397
1.098612288668109
1.0986122886681098
```
Then we'll look at the first version of Gregory Series:
```
log3_grog = 0
term = 1
for i in range (1, 100, 2):
    term *= 2/i * ((1/2))
    if i > 0:
        term *= (1/2)
    log3_grog += term
    term *= i/2
print(2*log3_grog) #1.098612288668109

```
Here are the first 11 iterations:
```
1.0
1.0833333333333333
1.0958333333333332
1.098065476190476
1.0984995039682537
1.0985882823773445
1.0986070624254214
1.098611131435838
1.0986120290116652
1.0986122297852055
1.0986122297852055
```
and a faster version of Gregories where (x+1)/(x-1) = 1/(1+sqrt3) -> x = 2/(4 + 2sqrt(3))
```

log3_fast = 0
term = 1
for i in range(1, 100, 2):
    term *= 2/i * ((2/ (4 + 2 * (math.sqrt(3))) ))
    if i > 1:
        term*= ((2/ (4 + 2 * (math.sqrt(3))) ))
    log3_fast += term
    term *= i/2
print(2*log3_fast) #1.0986122886681098

```
here are the first 11 iterations
```
1.0717967697244908
1.0974472850136117
1.0985522594973283
1.0986089263534418
1.098612090740168
1.0986122766251412
1.0986122879178601
1.0986122886205367
1.0986122886650513
1.0986122886679108
1.0986122886679108
```
As we can see the way I listed these methods are in increasing order of speed, as each is much faster than the one before it. 

####Part 2

##1
First is Heron's method:
```
import math
def myheron(a, n):
    x = a
    for i in range(1, n):
        x_nplus1 = (x + a / x) / 2
        x = x_nplus1
    return x

print(myheron(16, 7)) # 4.0
```
Then Goldschmidts, but it's important to assume a < 5 or else the formula's used to compute the sequence keep b_n+1 = b_n, or render it unintelligible 
```
def mygoldschmidts(a, n):
    b = a
    y = a
    for i in range(1, n):
        k = (b-3)/2
        b_nplus1 = b/4 * ((3-b) ** 2)
        b = b_nplus1
        y *= k
    return y

print(mygoldschmidts(2, 18)) # 1.4142135623730951

```

#2

First we are going to try the fixed point method x = arcsin(e^-x).
x0 must simply be > 0.

phi'(x) = -(e^-x) / sqrt(1 - e ^ (-2x)) 
0< abs(phi'(x)) < 1 as e^-x < 1 and  lim(x-> inf) sqrt(1-e^-2x) =  1 
Thus this converges linearly.
```
def myphi_linear(x,n):
    for i in range(n):
        y0 = math.asin(math.e ** -x)
        x = y0
    return x
print(myphi_linear(2,15)) # 0.5864732789154583 (alpha)

```
Now we are going to try a fixed point method using Newton-Raphson: phi(x) = x - (sin(x) - e^-x)/(cos(x) + e ^-x)

```
def myphi_quadratic(x,n):
    for i in range(n):
        y0 = x - (math.sin(x) - (math.e ** -x)) / (math.cos(x) + (math.e ** -x))
        x = y0
    return x
print(myphi_quadratic(1, 15)) #0.5885327439818611

```

