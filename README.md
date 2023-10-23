# Math317Assignment2
#1
First we just used the power series function from Assignment 1. Then we implemented matplotlib and generated points for each iteration, x being the number of iterations, y being the log of the difference in error. I am not sure how to insert images into GitHub, but the slope of the plotted logarithm is negative and shows a negative correlation between the error term and the amount of iterations.
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
    

def mysinplot(x,n):
    y_list = []
    x_list = [] 
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

print(mysin(1.2, 20)) #0.9320390859672263

print(mysin(5, 20))   #-0.9589331651965991

print(mysin(20, 20))  #-22100480.976672374 #Clearly incorrect
print(mysin(-30, 20)) #68044275598.92524


 ```
##2
We'll simply add two methods of argument reduction. One that reduces arguments to pi/2 using the periodicity of sin: 
sin(x + 2pi) = sin(x). 

```
x = x % (2*math.pi)
```
And one where we use it's symmetry: sin(-x) = -sinx
```
if x < 0:
  x = -x
#And afterwards
if x <0 : 
  return -sin
```
In order to save your time (and amount of scrolling), the combination of the two best helps with being able to estimate any range of inputs, versus strictly positive or strictly small ones.
Which gives us a complete function of:
```
def mysin(x, n):
        x = x % (2*math.pi)
        if x < 0:
            x = -x
        sin = 0
        term = x

        #Taking a -x^2 step while increasing factorial in denominator 
        for i in range (1, n+1, 2):
            #Simply the the series formula (multiplying each step by -x^2 and moving
            #the factorial in the denominator along 2 steps
            sin += term 
            term *= -(x * x) / ((i + 1) * (i + 2))
        if x < 0:    
            return -sin
        else:
            return sin 
    

def mysinplot(x,n):
    y_list = []
    x_list = []
    for i in range (1, n+1, 2):
                #Simply the the series formula (multiplying each step by -x^2 and moving
                #the factorial in the denominator along 2 steps
        try:
            y_list += [(math.log(abs(math.sin((math.pi/2))-mysin((math.pi/2), i))))]
            
            x_list += [i]
        except ValueError:
                break
    plt.plot(x_list,y_list)
    plt.xlabel('X-axis')
    plt.ylabel('Y-axis')
    plt.title("A simple line graph")
    plt.show()

print(mysin(1.2, 20)) #0.9320390859672263

print(mysin(5, 20)) #-0.9589331651965991

print(mysin(20, 20)) #0.9129452507276279
print(mysin(-30, 20)) #0.9880316240928616


```
##3
Similarly, we will use the Meractor method and combine it with an identical plotting function. Similarly to #1, the slope shows a negative correlation between number of iterations and the error term. Of course abs(x) < 1 here. It is also VERY slow.

```
def log_mer_gen(x, n):
    x= x-1
    log_mer = 0
    term = -1
    for i in range(1,n):
        #Multiplying each successive term by x/i, and -1 because it is alternating
        term *= -1/i * x
        log_mer += term

        #Making sure to not carry fraction coefficient into the next term
        term *= i
    return(log_mer)

print(log_mer_gen(2, 5000)) #0.6932471905599505
#After 5000 iterations it still hardly has the first few digits correct

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
For argument reduction, we will look at the reduction via (x+1)/(x-1) = 1 + x/(1+sqrt(x)) for the Mercator method.
This allows x to be greater than 1! 
```
def log_reduced(x, n):
    log3_mfast = 0
    term = 1
    for i in range(1, 20, 2):
        #This expression simply lines up the algebra such that any x > 0
        # can be calculated in the argument reduced method
        term *= 2/i * ( (x-1) / ( (x+1) + 2 * myheron(x, n) ) )

        #since the only term not multipled by and "x^2" step is the first, this if/else
        #helps us get around it
        if i > 1:
            term *= ( (x-1) / ( (x+1) + 2 * myheron(x, n) ) )
        log3_mfast += term
        term *= i/2
    return log3_mfast

#Multiplying the result by 2 as Gregory Series gives us 0.5log
print(2*log_reduced(10, 15)) #2.302584823696781
print(2*log_reduced(5, 15)) #1.609437912067221
print(2*log_reduced(20, 15)) #2.9957108383773385
print(2*log_reduced(17, 15)) #2.833204497276931

```
#4
Here we will look at Mercators log formula and two methods to compute Gregory's. The reasoning behind the code is the same as in the two versions of Question 3.
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
and a faster version of Gregory's where (x+1)/(x-1) = 1/(1+sqrt3) -> x = 2/(4 + 2sqrt(3))
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
def myheron(a, n):
    x = a
    for i in range(1, n):
        #Simply creating the sequence using the formula given in class
        x_nplus1 = (x + a / x) / 2
        x = x_nplus1
    return x

print(myheron(16, 7)) # 4.0
print(myheron(1500, 15)) # 38.72983346207417


```
Here are the iterations leading to the second print out:
```
750.5
376.2493337774817
190.11802590009145
99.0039308317101
57.077422205291754
41.6787579201503
38.834157053915604
38.72997358886769
38.72983346232766
38.72983346207417
```
We can see the error term grow smaller and smaller when we add a print(x-math.sqrt(1500))/math.sqrt(1500))
to the end of the previous function. No matter what x is it continues to hone in on the square root value.
```
18.377826675524446
8.714716024945483
3.9088263208325635
1.556270502135228
0.4737327043036292
0.0761409021023505
0.0026936235587894605
```
Then Goldschmidts, but it's important to assume a < 5 or else the formula's used to compute the sequence keep b_n+1 = b_n, or render it unintelligible.
```
def mygoldschmidts(a, n):
    b = a
    y = a
    for i in range(1, n):
        #Clearly defined k, b, and y values and sequences as seen in class
        k = (b-3)/2
        b_nplus1 = b/4 * ((3-b) ** 2)
        b = b_nplus1
        y *= k
    return y

print(mygoldschmidts(2, 18)) # 1.4142135623730951
print(mygoldschmidts(7, 18)) # OverflorError: (34, 'Result too large')

```
The second it's off by a bit it takes off and diverges very quickly. 
And here are the first four iterations for the second printout:
```
9.0
47.25
8719.83984375
55251513136.57361
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
        #Finding where x0 hits the curve, then creating that to be the next x point (as we look for where x = y)
        y0 = math.asin(math.e ** -x)
        x = y0
    return x
print(myphi_linear(2,15)) # 0.5864732789154583 (alpha)

```
Now we are going to try a fixed point method using Newton-Raphson: phi(x) = x - (sin(x) - e^-x)/(cos(x) + e ^-x)

```
def myphi_quadratic(x,n):
    for i in range(n):
        #Similar reasoning to the previous method
        y0 = x - (math.sin(x) - (math.e ** -x)) / (math.cos(x) + (math.e ** -x))
        x = y0
    return x

print(myphi_quadratic(1, 15)) #0.5885327439818611

```
