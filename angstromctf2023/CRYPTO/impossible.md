# impossible
### 50 Points
Desctiption: Is this challenge impossible?

## Solution
We are given a python script to find a way to expoit it and find the flag.

The keys to take note of are lines 32, 9, and 20. The functions called pass a range of 64.
![image](https://user-images.githubusercontent.com/28494055/235210976-38718268-01bc-4c92-bd78-9c1bbc59f638.png)

I was curious what would happen if I entered values for x and y with a range above 64, so I just entered a bunch of 1's for x and y input:
![image](https://user-images.githubusercontent.com/28494055/235211254-b12cf619-04aa-4310-a1b8-e5707d558492.png)

