## Implementation of Byte-Pair-Encoder algorithm.

The first ingredient of the BPE algorithm is a corpus of text. Let's take the following example. 


                                The world is full of wonders.  
                                Mountains and oceans,  
                                beauty everywhere.  
                                The world is amazing!

We begin by preprocessing the text. This involves converting the entire corpus to lowercase, removing punctuation marks, and replacing spaces with underscores to indicate the end of a word.

                                [t, h, e, _, 
                                w, o, r, l, d, _, 
                                i, s, _, 
                                f, u, l, l, _, 
                                o, f, _, 
                                w, o, n, d, e, r, s, _, 
                                m, o, u, n, t, a, i, n, s, _, 
                                a, n, d, _, 
                                o, c, e, a, n, s, _, 
                                b, e, a, u, t, y, _, 
                                e, v, e, r, y, w, h, e, r, e, _, 
                                t, h, e, _, 
                                w, o, r, l, d, _, 
                                i, s, _, 
                                a, m, a, z, i, n, g, _]



Out basic dictionary consists of the set of characters used in out corpus. In this case, it is given by 22 tokens.

    {'i': 0, 'w': 1, 'r': 2, 'z': 3, 'e': 4, 's': 5, 't': 6, 'd': 7, 'y': 8, 'l': 9, 'a': 10, 'f': 11, 'v': 12, 'h': 13, '_': 14, 'c': 15, 'b': 16, 'g': 17, 'u': 18, 'm': 19, 'n': 20, 'o': 21}

Using the BPE algorithm, we will expand the size of our vocabulary. First, let's count the occurrences of consecutive character pairs in our corpus. For example, the pair `(t, h)` occurs twice, while the pair `(c, e)` occurs once. We identify the pair with the highest count, which in this case is `(s, _ )`, occurring 5 times. We will add this pair to our vocabulary, increasing the total number of tokens to 23. Next, we will revisit our corpus and merge every occurrence of `s` followed by `_`. This results in:

                                [t, h, e, _, 
                                w, o, r, l, d, _, 
                                i, s_, 
                                f, u, l, l, _, 
                                o, f, _, 
                                w, o, n, d, e, r, s_, 
                                m, o, u, n, t, a, i, n, s_, 
                                a, n, d, _, 
                                o, c, e, a, n, s_, 
                                b, e, a, u, t, y, _, 
                                e, v, e, r, y, w, h, e, r, e, _, 
                                t, h, e, _, 
                                w, o, r, l, d, _, 
                                i, s_, 
                                a, m, a, z, i, n, g, _]

Now we will repeat the exact same algorithm by counting consecutive pairs, taking the one that appears most frequently, adding it to the vocabulary, and then merging the corpus again. This process continues until we reach the desired vocabulary size, which is a hyperparameter that controls the size of the final vocabulary. If we run the BPE algorithm with 5 merging cycles, the final vocabulary is:

    {'i': 0, 'w': 1, 'r': 2, 'z': 3, 'e': 4, 's': 5, 't': 6, 'd': 7, 'y': 8, 'l': 9, 'a': 10, 'f': 11, 'v': 12, 'h': 13, '_': 14, 'c': 15, 'b': 16, 'g': 17, 'u': 18, 'm': 19, 'n': 20, 'o': 21, ('s', '_'): 22, ('h', 'e'): 23, ('w', 'o'): 24, ('d', '_'): 25, ('t', 'he'): 26}

Now our initial sequence, "wow, the world is amazing," is encoded as:


    ['wo', 'w', '_', 'the', '_', 'wo', 'r', 'l', 'd_', 'i', 's_', 'a', 'm', 'a', 'z', 'i', 'n', 'g', '_']

As you can see, since `"the"`, `"wo"` and ` "d_"` appear quite frequently in our corpus, the BPE algorithm included those in the vocabulary as tokens, which is more efficient than trying to assign a code to each character.

