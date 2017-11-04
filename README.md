# Smith-Numbers

Smith Numbers

Description

While skimming his phone directory in 1982, Albert Wilansky, a mathematician of Lehigh University,noticed that the telephone number of his brother-in-law H. Smith had the following peculiar property: The sum of the digits of that number was equal to the sum of the digits of the prime factors of that number. Got it? Smith's telephone number was 493-7775. This number can be written as the product of its prime factors in the following way:

4937775= 3*5*5*65837


The sum of all digits of the telephone number is 4+9+3+7+7+7+5= 42,and the sum of the digits of its prime factors is equally 3+5+5+6+5+8+3+7=42. Wilansky was so amazed by his discovery that he named this kind of numbers after his brother-in-law: Smith numbers.
As this observation is also true for every prime number, Wilansky decided later that a (simple and unsophisticated) prime number is not worth being a Smith number, so he excluded them from the definition.

Wilansky published an article about Smith numbers in the Two Year College Mathematics Journal and was able to present a whole collection of different Smith numbers: For example, 9985 is a Smith number and so is 6036. However,Wilansky was not able to find a Smith number that was larger than the telephone number of his brother-in-law. It is your task to find Smith numbers that are larger than 4937775!

Input

The input file consists of a sequence of positive integers, one integer per line. Each integer will have at most 8 digits. The input is terminated by a line containing the number 0.

Output

For every number n > 0 in the input, you are to compute the smallest Smith number which is larger than n,and print it on a line by itself. You can assume that such a number exists.

Sample Input

4937774

0

Sample Output

4937775


寻找最接近而且大于给定的数字的SmithNumber

什么是SmithNumber？


用sum(int)表示一个int的各位的和，那一个数i如果是SmithNumber,则sum（i） = sigma( sum(Pj )),Pj表示i的第j个质因数。例如4937775= 3*5*5*65837，

4+9+3+7+7+7+5 = 42，3+5+5+（6+5+8+3+7） = 42，所以4937775是SmithNumber。


思路：要寻找大于给定数字且最接近给定数字的SmithNumber，只要将给定数字不断的加1，判断它是否是SmithNumber就行了，如果是SmithNumber就立即输出。


但是如何判断是否是SmithNumber呢？首先就是要对数字进行质因数分解。质因数分解要保证因子都是质数。这里先介绍一个如何判断一个数int是否是质数呢，如果对于这个数，i = 2.....sqrt(int)都不是它的约数，那int就是一个质数。所以我们可以将i初始化为2，然后不断递增i，看i是否是int的一个约数，如果是的话那i就是int的一个质因数（因为这个数是从2开始第一个可以整除int的数，它不可能是一个可以分解的合数，否则，它的约数在它之前就整除int），然后将int除以该质因数，重置i为2，重新对int判断它是否是质数。这样最后剩下的int一定是一个质数，从而对int进行了质因数分解


然后就很简单的将数字各质因数的各位加起来，看和是否等于该数字的各位和，如果相等那它可能就是SmithNumber，为什么说只是可能呢，因为这个数可能是质数，但是质数不是SmithNumber。

#include <stdio.h>

#include <math.h>

int Sum( int number )

{

int sum = 0;

while( number != 0 )

{

   sum += number % 10;
   
   number /= 10;
   
}

return sum;

}

bool SmithNumber( int number )

{

int i = 2;

int temp = number;

int sumOfNumber = Sum( number );

int sum = 0;

while( i <= (int)sqrt( (double)number ) )

{

   if ( number % i ==0 )     
   
   {
   
    sum += Sum( i );
    
    number /= i;
    
    i = 2;
    
   }
   
   else
   
   {
   
    ++i;
    
   }
   


//以上的代码做了无谓的计算，可用下面的代码

//while( number % i == 0 )

//{

//     sum += sum(i);

//     number /= i;

//}

// ++i;

}

sum += Sum( number );

if ( sum == sumOfNumber && number != temp )

{

   return true;
   
}

else

{

   return false;
   
}

}


int main()

{

while( true )

{

   int num;
   
   scanf("%d",&num );
   
   if ( num == 0 )
   
   {
   
    break;
    
   }
   
   while( true )
   
   {
   
    if ( SmithNumber(++num))
    
    {
    
     printf("%d\n", num);
     
     break;
     
    }
    
   }
   
}

return 0;

}
