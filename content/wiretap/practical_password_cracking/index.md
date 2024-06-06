+++
title = "Practical Password Cracking with a Modern Touch"
date = 2024-05-28
[taxonomies]
tags = ["password cracking", "hashcat", "passgpt", "LLM"]
+++

[Practical password cracking - hardware, tools, methods, and AI](https://github.com/sean-t-smith/pwned-by-passgpt/blob/main/Practical%20Password%20Cracking%20-%20Hardware%2C%20Tools%2C%20Methods%20and%20AI%20-%20vF.pdf)

Great walkthrough presentation about how to approach password cracking in a smarter and practical way. The multi-stage approach starting from easy to hard:

* known passwords and dictionary words
* brute-force of lower number of characters (1-8)
* character mask attack for larger number of characters (9-12)
* digit brute force 13-14 characters long
* rules usage and multiple wordlist combinations
* rules usage and weakpass wordlist combinations
* LLM dictionary
* rules usage and LLM dictionary combinations

The new discovery was the part where it mentions using an LLM model called PassGPT to brute force passwords that are 10-16 characters long. Based on the given stats, 150K cracked out of 39 billion keyspace within 3 days is quite impressive as the final dictionary size is the most difficult to crack.