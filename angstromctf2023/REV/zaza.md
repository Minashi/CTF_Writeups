# zaza
### 50 Points
Desctiption: Bedtime!

nc challs.actf.co 32760

## Solution
I was given an executable called zaza. For this challenge I used Ghidra to decompile the executable and view its contents.

I was able to find a main function:

![image](https://user-images.githubusercontent.com/28494055/234200252-01b0337f-6ae4-4290-89b9-d97691358765.png)

The program asks me 3 questions. For question 1, I saw that the program takes my input and checks it against 0x1337 whose decimal number is ```4919```, the answer for question 1. 

![image](https://user-images.githubusercontent.com/28494055/234200475-f615b34c-1a91-460e-ad58-1d5fec15d8b6.png)

For question 2 I was able to put any number input and it allowed me to proceed to the next question.

![image](https://user-images.githubusercontent.com/28494055/234200929-8cfecee1-7b82-407a-b672-4dd156542583.png)

For question 3, I enjoyed this part the most. The program takes my input, and throws it into a function called xor_ where it encodes the input. 

It then compares the encoded string to 

![image](https://user-images.githubusercontent.com/28494055/234201744-91d9c306-5722-4778-b268-57776e32f0cd.png)

![image](https://user-images.githubusercontent.com/28494055/234201963-32cea399-d97d-4c5a-a363-9ee9dbcba603.png)
