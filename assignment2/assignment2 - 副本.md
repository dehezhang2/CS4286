# CS4286: Assignment 2

[TOC]

## 1. Risk and the impact of security



## 2. Digital Signature

* Can these algorithms be used to encrypt a message?
  * These algorithms cannot be used to encrypt messages
* Main difference in how you verify the validity of the message with these schemes when compared to RSA
  * The digest is not directly used in DSA and ElGamal
* An example of 

## 3. Mutual Authentication

- This protocol is vulnerable to reflection attack. 
- When Alice sends $R$ to Bob, attacker can send $R+1$ to Alice and get the signature $[R+1]_A$. Then in the third step, attacker can send $[R+1]_A$ to Bob to pretend him to be Alice. Therefore, only Bob is authenticated

## 4. Key Management



## 5. Digital Certificates