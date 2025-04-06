Here i will note down where my brain went first time seeing the question 
why it was wrong and what the question actually wants 

2nd time my brain assumed a wrong thing and then finally after asking chatgpt i got to the soln 

Noting this brain pattern is very important 
Why as brain tries to repeat or the circuit is formed the way the first time you were thinking it 

I learnt about Random class in java 


****************************************************************Random Number Generator*************************

Random rand = new Random();
rand.nextInt(val); --- > generates random number from 0 to val - 1;

to generate random numbers within a range , lets say min , max
int diff = max - min
rand.nextInt(diff) --> generates from 0 to diff - 1

rand.nextInt(diff + 1 ) ---> generates from 0 to diff

num = min + rand.nextInt(diff + 1 ) -> generates from min to min + diff = max


