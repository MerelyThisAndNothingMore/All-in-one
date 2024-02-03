---
tags:
- computerArchitecture 
---
When we speed up one part of a system, the effect on the overall system performance depends on both how significant this part was and how much it sped up.
$$
S_{latency}(s) = \frac{1}{(1 - p) + \frac{p}{s}}
$$
Furthermore,
$$
\begin{cases}
S_{latency}(s) \leq \frac{1}{1 - p} \\ \\
\lim_{s\rightarrow\infty}S_{latency}(s) = \frac{1}{1 - p}
\end{cases}
$$


# References 
[Wiki](https://en.wikipedia.org/wiki/Amdahl%27s_law)

