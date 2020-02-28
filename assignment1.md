# CS4286 Assignment 01

* Name: Zhang Deheng
* SID: 55199998

[TOC]

------------------------------

## Q1: Security Service and Mechanisms ([Link]())



------------------------------

## Q2: Substitution Cipher

* Key: 

  ```
  trxcaplymzogvdqfjhbekswiun 
  ```

* Result: 

  ```
  IN THE THIRD WEEK OF NOVEMBER, IN THE YEAR 1895, A DENSE
  YELLOW FOG SETTLED DOWN UPON LONDON. FROM THE MONDAY
  TO THE THURSDAY I DOUBT WHETHER IT WAS EVER POSSIBLE
  FROM OUR WINDOWS IN BAKER STREET TO SEE THE LOOM OF
  THE OPPOSITE HOUSES. THE FIRST DAY HOLMES HAD SPENT IN
  CROSS-INDEXING HIS HUGE BOOK OF REFERENCES. THE SECOND
  AND THIRD HAD BEEN PATIENTLY OCCUPIED UPON A SUBJECT
  WHICH HE HAD RECENTLY MADE HIS HOBBY–THE MUSIC OF THE
  MIDDLE AGES. BUT WHEN, FOR THE FOURTH TIME, AFTER PUSHING
  BACK OUR CHAIRS FROM BREAKFAST WE SAW THE GREASY, HEAVY
  BROWN SWIRL STILL DRIFTING PAST US AND CONDENSING IN
  OILY DROPS UPON THE WINDOW-PANES, MY COMRADE’S IMPATIENT
  AND ACTIVE NATURE COULD ENDURE THIS DRAB EXISTENCE NO
  LONGER. HE PACED RESTLESSLY ABOUT OUR SITTING-ROOM IN
  A FEVER OF SUPPRESSED ENERGY, BITING HIS NAILS, TAPPING
  THE FURNITURE, AND CHAFING AGAINST INACTION.
  ```

* Steps: 

  * I used the [online frequency analysis tool](<http://www.richkni.co.uk/php/crypta/freq.php>) to analyze the character frequency and compared it with the frequency [bigram](<https://en.wikipedia.org/wiki/Bigram>) and [trigram](<https://en.wikipedia.org/wiki/Trigram>). 

    ![1582785789675](assignment1.assets/1582785789675.png)

  * According to the letter frequencies, bigram and trigram, I can find that

    ```
    e => a, t => e, z => n, he => ya, th => ey, ing => mdl... 
    ```

  * I use the command `tr` to substitute the characters

    ```
    tr 'eghintz' 'eghintn' < cipher.txt > test.txt
    ```

  * Based on the output, continue observe the words, and try whether I can do some substitution to get a word, to analyze the result better, I wrote the code to select words with specific length:

    ```python
    import operator
    length = input("Length of the word: ")
    with open('test.txt', 'r') as file:
    	words = file.read().split()
        dic = {}
        for word in words:
            if len(word) == length:
                if not word in dic :
                    dic[word] = 1
                else:
                    dic[word] += 1
         sorted_dic = sorted(dic.items(), key=lambda kv:kv[1])
         print(sorted_dic)
    ```

  * Repeat the  previous three steps until all words can be found.

---------------------------------

## Q3: Modes of Operation and ‘shift cipher’

* Plaintext: ‘IAMALICE’, Key: 4

  ![1582787745372](assignment1.assets/1582787745372.png)

* (a) Encrypt the plaintext $P$ using $CBC$ mode with $IV = 0001$.

$$
C_0 = E_k(P_0\oplus IV) = E_k(1000\oplus 0001) = E_k(1001) = E_k(J) = N(1101) \\
C_1 = E_k(P_1\oplus C_0) = E_k(0000\oplus 1101) = E_k(1101) = E_k(N) = B(0001)\\
C_2 = E_k(P_2\oplus C_1) = E_k(1100\oplus 0001) = E_k(1101) = E_k(N) = B(0001)\\
C_3 = E_k(P_3\oplus C_2) = E_k(0000\oplus 0001) = E_k(0001) = E_k(B) = F(0101)\\
C_4 = E_k(P_4\oplus C_3) = E_k(1011\oplus 0101) = E_k(1110) = E_k(O) = C(0010)\\
C_5 = E_k(P_5\oplus C_4) = E_k(1000\oplus 0010) = E_k(1010) = E_k(K) = O(1110)\\
C_6 = E_k(P_6\oplus C_5) = E_k(0010\oplus 1110) = E_k(1100) = E_k(M) = A(0000)\\
C_7 = E_k(P_7\oplus C_6) = E_k(0100\oplus 0000) = E_k(0100) = E_k(E) = I(1000)\\
$$

* (b) Encrypt the plaintext $P$ using $CBC$ mode with $IV = 0101$. How does your ciphertext
  compare to that in 2(a).
  $$
  C_0 = E_k(P_0\oplus IV) = E_k(1000\oplus 0101) = E_k(1101) = E_k(N) = B(0001)\\
  C_1 = E_k(P_1\oplus C_0) = E_k(0000\oplus 0001) = E_k(0001) = E_k(B) = F(0101)\\
  C_2 = E_k(P_2\oplus C_1) = E_k(1100\oplus 0101) = E_k(1001) = E_k(J) = N(1101)\\
  C_3 = E_k(P_3\oplus C_2) = E_k(0000\oplus 1101) = E_k(1101) = E_k(N) = B(0001)\\
  C_4 = E_k(P_4\oplus C_3) = E_k(1011\oplus 0001) = E_k(1010) = E_k(K) = O(1110)\\
  C_5 = E_k(P_5\oplus C_4) = E_k(1000\oplus 1110) = E_k(0110) = E_k(G) = K(1010)\\
  C_6 = E_k(P_6\oplus C_5) = E_k(0010\oplus 1010) = E_k(1000) = E_k(I) = M(1100)\\
  C_7 = E_k(P_7\oplus C_6) = E_k(0100\oplus 1100) = E_k(1000) = E_k(I) = M(1100)\\
  $$

  * The cipher text is slightly different

* (c) Use your answer from 2(b). If the MSB bit of $C_2$ becomes an error($C_2 = F(0101)$), what is the recovered plaintext?
  $$
  P^\prime_0 = D_k(C_0) \oplus IV = N \oplus IV = 1101 \oplus 0101 = I(1000)\\
  P^\prime_1 = D_k(C_1) \oplus C_0 = B \oplus C_0 = 0001 \oplus 0001 = A(0000)\\
  P^\prime_2 = D_k({\color{red}C_2}) \oplus C_1 = {\color{red}B} \oplus C_1 = {\color{red}0001} \oplus 0101 = A(0100)\\
  P^\prime_3 = D_k(C_3) \oplus {\color{red}{C_2}} = N \oplus {\color{red}{C_2}} = 1101 \oplus {\color{red}{0}}101 = {\color{red}{I}}({\color{red}1}000)\\
  P^\prime_4 = D_k(C_4) \oplus C_3 = K \oplus C_3 = 1010 \oplus 0001 = L(1011)\\
  P^\prime_5 = D_k(C_5) \oplus C_4 = G \oplus C_4 = 0110 \oplus 1110 = I(1000)\\
  P^\prime_6 = D_k(C_6) \oplus C_5 = I \oplus C_5 = 1000 \oplus 1010 = C(0010)\\
  P^\prime_7 = D_k(C_7) \oplus C_6 = I \oplus C_6 = 1000 \oplus 1100 = E(0100)\\
  $$

* (d) Use your answer from 2(b). If the block $C_2$ is lost (receiver does not realize it is missing),
  what is the recovered plaintext?

  * Know Information: 
    $$
    C_0 = B(0001)\\
    C_1 = F(0101)\\
    C_2 = B(0001)\\
    C_3 = O(1110)\\
    C_4 = K(1010)\\
    C_5 = M(1100)\\
    C_6 = M(1100)\\
    IV  = 0101
    $$

  * Decryption
    $$
    P^\prime_0 = D_k(C_0) \oplus IV = N \oplus IV = 1101 \oplus 0101 = I(1000)\\
    P^\prime_1 = D_k(C_1) \oplus C_0 = B \oplus C_0 = 0001 \oplus 0001 = A(0000)\\
    P^\prime_2 = D_k(C_2) \oplus C_1 = N \oplus C_1 = 1101 \oplus 0101 = I(1000)\\
    P^\prime_3 = D_k(C_3) \oplus C_2 = K \oplus C_2 = 1010 \oplus 0001 = L(1011)\\
    P^\prime_4 = D_k(C_4) \oplus C_3 = G \oplus C_3 = 0110 \oplus 1110 = I(1000)\\
    P^\prime_5 = D_k(C_5) \oplus C_4 = I \oplus C_4 = 1000 \oplus 1010 = C(0010)\\
    P^\prime_6 = D_k(C_6) \oplus C_5 = I \oplus C_5 = 1000 \oplus 1100 = E(0100)\\
    $$


-------------------------------------

## Q4: Number Theory

* $X=55199998,Y=9998$

* To factorization $n$, we can use the prime numbers from $2$ to $\sqrt{n}$, if $n$ is divisible by one of the prime number $p$, then keep doing $n = n/p$ until $n$ is not divisible, if the final result is not $1$, it must be another prime number in the factorization.  

* (a) Compute $43^Y\ mod\ 4286$ using the square-and-multiply method.

  * Factorization: 
    $$
    4286 = 2*2143 \\
    gcd(4286,43) = 1
    $$

  * According to the Euler’s theorem and Euler function: 
    $$
    \alpha^{\phi(n)} \equiv 1 (mod\ n)(gcd(a,n)=1)\\
    \phi(n) = n \Pi_{p|n}(1-{1 \over p}) \\
    \phi(4286) = 4286*(1-{1\over 2143})(1-{1\over 2}) = 2142 \\
    43^{2142} \equiv 1(mod\ 4286)\\
    43^{9998}\ mod\ 4286 = 43^{4*2142+1430}\ mod\ 4286 = 43^{1430}\ mod\ 4286\\
    =2401
    $$

  * By using the following code to calculate the $43^{1430}\ mod\ 4286$ 

    ```python
    def mod_pow(a, p, n):
    	if p==1:
    		return a%n
    	x = mod_pow(a, p/2, n)
    	print("%d to the power of %d mod %d is %d"%(a, p/2, n, x))
    	if p%2 == 0:
    		return (x*x)%n	
    	else:
    		return (x*x*a)%n
    print("43 to the power of 1430 mod 4286 is %d"%(mod_pow(43, 1430, 4286)))
    ```

    ![1582872523196](assignment1.assets/1582872523196.png)

* (b) Calculate $\phi(Y)$. 

  * Factorization: $9998 = 2*4999$ 
  * According to Fermat’s little theorem $\phi(9998) = (2-1)*(4999-1)=4998$

* (c) $gcd(X, 928374827)$

  * By using the following code: 

    ```python
    def gcd(a,b):
    	if a > b :
    		if b == 1:
    			return a
    		x = gcd(b, a%b);
    		print("gcd(%d, %d) = %d"%(b,a%b,x))
    		return x
    	else:
    		return gcd(b,a)
    print(gcd(55199998, 928374827))
    ```

  * Get the answer $9$:

    ```
    gcd(9, 1) = 9
    gcd(235, 9) = 9
    gcd(479, 235) = 9
    gcd(714, 479) = 9
    gcd(1907, 714) = 9
    gcd(12156, 1907) = 9
    gcd(123467, 12156) = 9
    gcd(4950836, 123467) = 9
    gcd(5074303, 4950836) = 9
    gcd(10025139, 5074303) = 9
    gcd(45174859, 10025139) = 9
    gcd(55199998, 45174859) = 9
    gcd(928374827, 55199998) = 9
    ```

* (e) Choose any prime number $Z$ that is smaller than $X$. Calculate $X^X\ mod\ Z$

  * Choose $Z = 2$
  * $X^X\ mod \ Z=(X\ mod\ Z)^X\ mod\ Z= (55199998\ mod\ 2)^{55199998}\mod 2=0$  

---------------------------

## Q5: El-Gamal

* $p=19$ and $g = 3$

* (a) Suppose the private key is $x = 7 $. Compute the public key $y$.
  $$
  y=g^x\ mod\ p = 3^7\ mod\ 19 = ((3^3\mod 19)^2\mod19) *( 3\mod 19)\mod 19 = 2
  $$

* (b) Encrypt the message $M = 8$ using the public key above and $r = 8$.
  $$
  A = g^r\ mod\ p = 3^8\ mod\ 19 = 6\\
  B = My^r\ mod\ p = 8*2^8\ mod\ 19 = 2^{11}\ mod\ 19 = 15
  $$

* (c) Verify your calculation in part (b) above by decrypting the ciphertext you obtained in
  part (b)
  $$
  K = A^x\ mod\ p = 6^{7}\ mod\ 19 = 9 \\
  $$

  * Find $9^{-1}(mod\ 19)$  
    $$
    19 = 2*9 + 1 \\
    19 - 2*9 = 1 \\
    19 + 19*9-2*9 = 19*9+1\\
    19 + 17*9 = 19*9+1\\
    17*9\equiv 1(mod\ 19)\\
    K^{-1} = 17 \\
    $$

  * Decrypt: 
    $$
    M=BK^{-1}\ mod\ p = (15*17)\ mod\ 19 = 8
    $$


--------------------------------

## Q6: Diffie-Hellman

* $p = 19$ and $g = 10$.

* (a) Alice picks $x = 7 $, and Bob picks $y = 11$. What is the shared key $K$ resulting from the
  exchange?
  $$
  \phi(19) = 18, gcd(19,10)=1\\
  K = g^{xy}\ mod\ p = 10^{7*11}\ mod\ 19 \\
  = 10^{77}\ mod\ 19 = 10^{4*18+5}\ mod\ 19 = 10^5 \ mod\ 19 = 3
  $$

* (b) How would you modify the exchanged messages in $DH$ to prevent its main weakness?
  Clearly state all your assumptions (including any additional cryptographic algorithm or
  material needed) and the notation you used.

  * We can use PKI 
  * The assumption is there is a trusted CA for both Alice and Bob which has issued certificates to them. 
  * When Bob get the $g^x\ mod\ p$ from Alice, there should also be a certificate signed by CA, and verified by Bob. (Same for Alice)
  * In this way, the man in middle cannot change the “partial key”, because it doesn’t have the signed certificate. 


