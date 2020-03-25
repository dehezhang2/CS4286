# CS4286: Assignment 2

[TOC]

## 1. Risk and the impact of security

- ***Real-name system*** is a system in which users can register an account on a blog, website or bulletin board system using their legal name. It is used by several countries including China, Germany, South Korea as well as some famous social networking sites including Facebook, Twitter. [[1]](<https://en.wikipedia.org/wiki/Real-name_system>) The main purpose of this system is to avoid anonymous crime.  In some perspective, it does make the online system more secure since it increases accountability and makes the system more professional appearance. [[2]](<http://www.managingcommunities.com/2011/01/20/usernames-vs-real-names-on-your-community-pros-and-cons/>) However, there is unintended consequence of this countermeasure. 
- ***Displacement***:  Since the public network (system) is real-name based, criminals might be encouraged implicitly to use other anonymous systems or network like dark web. For example, if online shop is set to be real-name system, customer who want to protect their personal information might turn to dark web to buy products. As a consequence, the dark web might be enlarged. Therefore displacement consequence might be involved. 
- ***Insecure norms***: As a consequence of displacement, more techniques to provide anonymity might be developed to support larger number of anonymous user in dark web. Therefore, insecure behavior is encouraged. 
- ***Additional costs***: Use of real-name system also increase additional costs. For normal users, they need to provide more information such as citizenship ID or mobile binding when registering a new account. For system provider (either government or website provider), they need to provide additional protection of user information since it is more dangerous to leak the information when the system is real-name. 
- ***Misuse***: The information of user might be misused. Once the information is not protected, real-name information can be more harmful to the users since attackers can identify each one of target more accurately. 
- ***Amplification***: The real-name system is considered to reduce cyberbullying since the everyone uses real-name, everyone is expected to behave more appropriately. However, some user may prefer to use anonymous account to share some feeling which they do not want to share with real-name to avoid bullying. Once their real-name is published, they may fall in trouble in real life. 
- ***Disruption***: As mentioned in previous paragraph, some user may not willing to share their feelings with real name. If real-name system is enforced, their operation can be interrupted. 

## 2. Digital Signature

### (a) ElGamal & DSA

* Can these algorithms be used to encrypt a message?
  * These algorithms cannot be used to encrypt messages, because these algorithms are not directly comparing the digest. 
  * For RSA, the role of public key and private key can be changed, which is the reason that it can be used as both encryption and signature algorithm. (we can call this property as duality)
  * For these two algorithms, the role of private key and public key cannot be changed. Therefore, we have to use different computation algorithm for DSA and ElGamal when used in encryption and signature respectively. 
* Main difference in how you verify the validity of the message with these schemes when compared to RSA
  * RSA directly compares the digest of message $M$ during the verification stage
  * ElGamal compares the base raised to the power of digest $g^{H(m)}\equiv y^rr^s(\ mod\ p)$, and DSA compares the base raised to the random selected number with the participation of the digest $g^k \equiv g^{H(m)s^{-1}}g^{xrs^{-1}} (mod\ p\ mod\ q)$. The digest is not directly used for DSA and ElGamal.

### (b) An example of ElGamal

* Set up: 

  * Base $g = 7$

  * Private key $x = 16$

  * Modular $p = 71$

  * Public key $y = g^x\ mod\ p = 7^{16}\ mod\ 71 = 19$

    ```
    16 = 2^4
    7^(2^1) mod 71 = 7^2 mod 71 = 49
    7^(2^2) mod 71 = 49^2 mod 71 = 58
    7^(2^3) mod 71 = 58^2 mod 71 = 27
    7^(2^4) mod 71 = 27^2 mod 71 = 19
    ( 19 ) mod 71 = 19
    ```

* Sign: 

  * Random $k = 9$

  * $r = g^k \mod\ p = 7^{9}\ mod\ 71 = 47$

    ```
    9 = 2^3 + 2^0
    7^(2^1) mod 71 = 7^2 mod 71 = 49
    7^(2^2) mod 71 = 49^2 mod 71 = 58
    7^(2^3) mod 71 = 58^2 mod 71 = 27
    ( 7 * 27 ) mod 71 = 47
    ```

  * Digest $H(m) = 13$

  * $s = (H(m)-xr)k^{-1}\ mod\ (p-1)$ 

    * Find $k^{-1}(mod\ (p-1)) = 9^{-1}\ (mod\ 70)$
      $$
      70 = 9*7+7 \\
      9 = 7*1+2 \\
      7 = 2*3 + 1\\
      1 = 7 - 2*3 \\
      1 = 7 - (9 - 7)*3 = 7*4 - 9*3\\
      1 = (70-9*7)*4 - 9*3 = 70*4 - 9*31\\
      9^{-1}\equiv 39\ (mod\ 70) \\
      $$

    * Find $s$
      $$
      s = (H(m)-xr)k^{-1}\ mod\ (p-1) \\
      = (13 - 16*47)*39\ mod\ 70 \\
      = 19
      $$

  * Send the signature as $(r,s) = (47, 19)$, as well as message $M$

* Verify:

  * $0<47<71$, $0 <19< 70$

  * $g^{H(M)}\ mod\ p = 7^{13}\ mod\ 71 = 28$

    ```
    13 = 2^3 + 2^2 + 2^0
    7^(2^1) mod 71 = 7^2 mod 71 = 49
    7^(2^2) mod 71 = 49^2 mod 71 = 58
    7^(2^3) mod 71 = 58^2 mod 71 = 27
    ( 7 * 58 * 27 ) mod 71 = 28
    ```

  * $y^r r^s\ mod\ p = 19^{47}*47^{19}\ mod\ 71$

    * $19^{47}\ mod\ 71 = 9$

      ```
      47 = 2^5 + 2^3 + 2^2 + 2^1 + 2^0
      19^(2^1) mod 71 = 19^2 mod 71 = 6
      19^(2^2) mod 71 = 6^2 mod 71 = 36
      19^(2^3) mod 71 = 36^2 mod 71 = 18
      19^(2^4) mod 71 = 18^2 mod 71 = 40
      19^(2^5) mod 71 = 40^2 mod 71 = 38
      ( 19 * 6 * 36 * 18 * 38 ) mod 71 = 9
      ```

    * $47^{19}\ mod\ 71 = 11$

      ```
      19 = 2^4 + 2^2 + 2^1
      47^(2^1) mod 71 = 47^2 mod 71 = 8
      47^(2^2) mod 71 = 8^2 mod 71 = 64
      47^(2^3) mod 71 = 64^2 mod 71 = 49
      47^(2^4) mod 71 = 49^2 mod 71 = 58
      ( 47 * 8 * 58 ) mod 71 = 11
      ```

    * $y^r r^s\ mod\ p = 19^{47}*47^{38}\ mod\ 71 = 9*11 \ mod\ 71 = 28$

    * Therefore, the message is successfully verified

## 3. Mutual Authentication

### (a)

* This protocol is vulnerable to **man-in-the-middle attack**

* The reason of it is not secure is $R+1$ is predictable, therefore freshness is not satisfied. 

* When Alice sends $R$ to Bob, Trudy can intercept it and send to Bob, Bob send $[R]_B$ to Trudy, then Trudy sends it to Alice. Alice sends $[R+1]_A$ again intercepted by Trudy, and Trudy sends $[R+1]_T$ to Bob. Alice and Bob do not know the existence of Trudy. (Shown as bellow as well as another method to attack)

  ![20200324_150655660_iOS](../assignment1/assets/20200324_150655660_iOS.png)

### (b)

* Assume all the public keys are certificated by trusted CA
* To achieve mutual authentication, we can replace the messages by
  * $R_A$
  * $R_B||[R_B||R_A||B||A]_B$
  * $[R_A||R_B||A||B]_A$ 
  * where $A, B$ are public available identifier of Alice and Bob respectively, and $R_A, R_B$ are random number generated by Alice and Bob respectively. By using the identifier to add the information of sender and receiver, man-in-the-middle attack does not work
* This protocol also protect from replay attack when one of Alice and Bob is using poor random number generation (i.e. one of them is repeating use one nonce)
* This protocol also protect from replay attack when Alice and Bob are using counter as nonce by changing the order of the two nonce in the signature. 

## 4. Key Management



## 5. Digital Certificates