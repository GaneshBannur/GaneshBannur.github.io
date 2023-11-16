---
layout: post
title: "A fast fuzzy substring algorithm"
date: 2023-9-3
---

This article is about an algorithm I came up with for fast fuzzy substring search, i.e., searching for the best match substring of a query in a corpus. Given a query string and a corpus string, the algorithm tries to find the substring in the corpus which is the most similar to the query.  
The code implementing the algorithm is at [https://github.com/GaneshBannur/fuzzy-substring](https://github.com/GaneshBannur/fuzzy-substring).  
This article is an extension of the Stackoverflow answer I gave [https://stackoverflow.com/a/77019582](https://stackoverflow.com/a/77019582).

A naive approach would be to go through every substring of the corpus and find the most similar one, however this would be too slow for large corpora. Instead the algorithm focuses on speed while providing the optimal result in most cases. It is not possible to guarantee that the result is optimal without some level of brute forcing, which slows down the search significantly. This is because at some point (after possibly narrowing down the search region in the corpus) such an algorithm would have to check every substring. Hence this algorithm does not provide that guarantee but it has been seen empirically that it provides the optimal result in most cases. At some point I'll add the theoretical characterization of the failure cases.

The first step is to define what it means for a substring of the corpus to be similar to the query. Any one of the plethora of string similarity/distance measures can be used in this algorithm. Once the measure is decided, the definition of the closest substring is as follows: The substring of the corpus which has the lowest distance (for distance measures) or highest similarity (for similarity measures) with the query.

I tested two measures: Levenshtein distance and Ratcliff-Obershelp similarity (specifically the `ratio` function in Python's `difflib` module). Empirically Levenshtein distance was found to work best for two reasons:
- It provides more accurate results
- It is extremely fast to compute

Now it is easy to define the most similar substring: The substring of the corpus most similar to the query is the one which has the lowest Levenshtein distance from it.

The algorithm finds the best substring match in two stages
1. First it  performs a quick low resolution scan over the corpus to identify the general region of the corpus in which the best match may exist
2. Second it performs multiple high resolution scans over the region identified to try and find the exact substring that is the  best match

Resolution here includes the step size and window size of the sliding window scan. The low resolution scan will have a high step size and high window size. The high resolution scan will have a low step size and low window size. Multiple high resolution scans with varying window sizes and `step size = 1` are performed to be able to identify the best match exactly, regardless of its length or position in the region identified.

<br>
# First Stage (low resolution scan)  
The first stage constructs substrings of the same size as the query using a sliding window over the corpus (the sliding window moves forward by `step` every time). The substrings constructed would be

    corpus[0 ... query_len - 1], corpus[step ... step + query_len - 1], corpus[2*step ... 2*step + query_len - 1], .....

The best value for `step` is empirically found to be `step = query_len//2` (`//` is integer division). This value of `step` works well because it provides a balance between speed and accuracy: a smaller step size would decrease speed and a larger step size may miss regions of the corpus. The smallest step size possible is 1. It gives the best accuracy as every substring of the corpus of length `query_len` is considered. It is also the slowest for the same reason. The largest step size possible is `query_len`, any larger than this and the scan would miss parts of the corpus. It gives the fastest scan since it takes large steps. It also has the worst accuracy for the same reason. A step size of `query_len//2` sits between the smallest and largest step sizes, giving it a balance between speed and accuracy.

The substrings constructed with `step = query_len//2` are (assuming `query_len` is divisible by two for simplicity)

    corpus[0 ... query_len - 1], corpus[query_len/2 ... 1.5*query_len - 1], corpus[query_len ... 2*query_len - 1], .....

The first stage scans over the corpus by constructing these substrings. The substring with the minimum Levenshtein distance from the query is found. This substring gives the region of the corpus around which the best match will most likely exist. The first stage serves the purpose of narrowing down the search region in the corpus. Once the region is found by the first stage it is doubled in length by considering `query_len//2` characters to both the left and right of the region. If the first stage narrowed down to `corpus[i ... i + query_len - 1]` then the search region for the second stage is `corpus[i - query_len//2 ... i + query_len - 1 + query_len//2]`. This gives us a region of length `query_len//2 (left extension) + query_len (first scan gives region of length = query_len) + query_len//2 (right extension) = 2*query_len`. The region is expanded to account for the error in identifying the exact region (since the first scan was a low resolution scan that jumped over the corpus with steps of `query_len//2`)

<br>
# Second Stage (high resolution scans)  
The second stage performs a more thorough search over the region identified (called `narrowed_corpus` in the code). This is done by constructing substrings of varying lengths over `narrowed_corpus` with `step = 1`. This is equivalent to performing multiple scans over `narrowed_corpus`, each one with a different window size and `step = 1` for the sliding window. Multiple scans are performed to consider more substrings of varying positions and lengths within `narrowed_corpus`, increasing our chances of finding the optimal solution.

Now we need to decide what substring lengths to consider. To decide this, we first narrow down the range of values considered using the following result.  
> The best match string `s`, which has the least Levenshtein distance from the query, has length `l` which satisfies `l <= 2*query_len`.

The proof of this result is as follows

***Proof***  

---  

We can show that no string of length more than `2*query_len` can have the minimum Levenshtein distance from the query.
Let `s = corpus[i ... i + l - 1]` be the `s` defined in the result.  

The Levenshtein distance of `s` from the query is the number of insertions, deletions and substitutions required to convert `s` to `query`. This has two components
1. Substitution of characters in `s`, if some ***subsequence*** of `s` does not match `query` exactly
2. Removal of `l - query_len` extra characters from `s`, since it is longer than the query

Considering only the second part tells us that the Levenshtein distance must be `> query_len` since `l > 2*query_len => l - query_len > query_len`. But any string of length `<= query_len` can be converted to `query` with `<= query_len` insertions and substitutions. So no string of length `> 2*query_len` can be the best match since.

---

<br>

The result imposes a theoretical upper limit on the substring lengths we should consider.
We can also impose a heuristic lower limit on the substring lengths by using the result. We can consider only substrings with length `> query_len/2`. Let `s` be a substring with length `l` such that `l < query_len/2` (assume it's divisible by 2). Since `query_len > 2*l`, the result tells us that `query` cannot be the best match for `s`. Note that `s` may still be the best match for `query`, it is just that there is some other substring in `corpus` which is a better match for `s` than `query` is. But empirically we can assume that if `query` is not the best match for `s` then `s` is probably not the best match for `query` (of course, this need not always be true). But assuming this gives us the lower bound on substring lengths as `query_len//2`. Empirically this assumption is found to not affect the optimality of the result in most cases.  

But here's an example of when this assumption causes the algorithm to be non-optimal:  
Input

    query = "philanthropic"
    corpus = "bbbbbbbbbbbbbbphiicbbbbbbbbbbbbbb"

Optimal Output

    Best Match: "phiic"
    Minimum Levenshtein distance: 8

Actual Output

    Best Match: "bphiic"
    Minimum Levenshtein distance: 9

***Characterizing the failures because of this assumption***  

---

Even when the best match has length `< query_len/2`, the assumption does not cause the algorithm to be non-optimal in one specific case: when the best match is a ***substring*** of `query` and not just a ***subsequence***. The example failure given above occurs because `"phiic"` is a subsequence of `"philanthropic"` but not a substring. This means some characters need to be filled *in between* the characters (rather than appending extra characters to a side) of `"phiic"` to get `"phi`*`lanthrop`*`ic"`. If however the best match is a substring of `query` then we can find a substring with length `> query_len/2` that has the same Levenshtein distance.  
Let `s`, a substring of `query`, be the best match with length `< query_len/2`. Then to convert `s` to `query` we would simply need to add the remaining characters of `query` to the sides of `s`. This would give us a Levenshtein distance of `query_len - s_len`. As an example, consider the failure case. If instead the corpus was `"bbbbbbbbbbbbbblanthbbbbbbbbbbbbbb"`, the best match `"lanth"` would be a substring of `"philanthropic"` and we would only need to add characters to the sides (*`"phi`*`lanth`*`ropic"`*).  
Suppose `n` characters need to be added to the left of `s` and `m` characters to the right to convert it to `query`. In this case we can find another substring of `corpus` which adds `n` and `m` characters of ***`corpus`*** to `s`. If `s = corpus[i ... i + s_len - 1]` then the new substring would be `s' = corpus[i - n ... i + s_len - 1 + m]` (in our example `s' = "`*`bbb`*`lanth`*`bbbbb`*`"`). `s'` would have the same Levenshtein distance as `s` since instead of adding `n` and `m` characters to `s`, we can instead substitute the `n` and `m` extra characters in `s'` to get `query`. But we could not have done the same if `s` was a subsequence and not a substring of `query` and required insertions *in between* characters.

---

<br>

In light of this, the second stage does not consider any substring of length `< query_len//2` or `> 2*query_len`. So the lengths of substrings considered for the multiple high resolution scans range from `query_len//2` to `2*query_len`.

We have established the range of substring lengths that we will consider. The next step is to decide which substring lengths to consider. The second stage considers substring lengths at fixed intervals between `query_len//2` and `2*query_len`. The intervals are defined by `step = query_len//step_factor`. The lengths of substrings constructed depend on the argument `step_factor`. The lengths considered are

    len(query)//2 - 1, len(query)//2 - 1 + step, len(query)//2 - 1 + 2*step, ....., 2 * len(query) + 2

The second stage considers lengths at intervals as considering all lengths between `query_len//2` and `2*query_len` would slow it down significantly.  
Empirically `step_factor = 128` works well, compromising between speed and chances of an optimal solution. Using this value, the lengths considered are

    len(query)//2 - 1, len(query)//2 - 1 + query_len//128, len(query)//2 - 1 + 2*query_len//128, ....., 2 * len(query) + 2

Increasing `step_factor` increases the number of substring lengths considered hence increasing the chances of finding the optimal substring with minimum Levenshtein distance. However increasing it will lead to increased runtime because more substrings need to be checked.

<br>

A final consideration in the algorithm is the preference for smaller or larger matches. Among all the substrings that the algorithm ends up searching, more than one may have minimum Levenshtein distance. The algorithm can return a list of all the substrings which have the same distance (the minimum distance). However when implementing the algorithm practically it is nice to return one substring as the best match. In this case we have to choose which substring from the list we return. Empirically it is found to be better to prefer the largest match. Perhaps because it is easier for people to skip over extraneous characters in a larger match than fill in missing characters in a smaller match.

<br>
# An example of the algorithm

Input

    query = "ipsum dolor"
    corpus = "lorem 1psum dlr sit amet"

Output

    Best match: "1psum dlr"
    Minimum Levenshtein distance: 3

<br>
# Example that breaks the algorithm
This is an example which misdirects the first stage of the algorithm and causes it to select the wrong region, making the end result of the algorithm entirely wrong.  

Input (note the extra spaces)

    query: "antidisestablishmentarianism"
    corpus: "    anidietabishetariism    antidbmjrietabishetariism   "

Optimal Output

    Best match: "anidietabishetariism"
    Minimum Levenshtein distance: 8

Actual Output

    Best match: "antidbmjrietabishetariism"
    Minimum Levenshtein distance: 11
