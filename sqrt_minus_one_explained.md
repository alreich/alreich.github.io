# $\sqrt{-1}$ Explained

A **complex number** is an ordered pair, (a, b), where $a$ and $b$ are real numbers. The set of all real numbers is denoted by $\mathbb{R}$, and the set of all complex numbers by $\mathbb{C}$. Addition and multiplication of complex numbers are well-defined, and always produce another complex number.

So, if $a, b, c, d$ are in $\mathbb{R}$, then $(a,b), (c,d)$ are in $\mathbb{C}$, and 

$$\begin{align}
(a,b) + (c, d) &= (a + c, b + d) \\
(a, b) \cdot (c, d) &= ( ac - bd,\ ad + bc )
\end{align}$$

Subtraction and division in $\mathbb{C}$ are also well defined, but not shown here.

The subset of $\mathbb{C}$, where the second component is always zero, such as $(a, 0)$, is called the *real-axis*, and is denoted by $\mathcal{Re}$. The naming is due to the fact that, with respect to the first components, and the definitions of complex addition and multiplication given above, the arithmetic of elements of $\mathcal{Re}$ corresponds to that of $\mathbb{R}$, as shown below:

$$\begin{align}
(a, 0) + (c, 0) &= (a + c, 0) \\
(a, 0) \cdot (c, 0) &= (ac, 0)
\end{align}$$

So, for any real number $a$, there is a corresponding complex number $(a, 0)$, and vice versa, denoted here as, $a \cong (a,0)$.

In particular, $-1 \cong (-1, 0)$. And, although $\sqrt{-1}$ does **not** exist in $\mathbb{R}$, $\sqrt{( -1, 0 )}$ **does** exist in $\mathbb{C}$, and equals $( 0, 1 )$, as shown by the calculation below:

$$\begin{align}
{( 0, 1 )}^2 &= ( 0, 1 ) \cdot ( 0, 1 ) \\
&= ( 0 \cdot 0 - 1 \cdot 1,\ 0 \cdot 1 + 1 \cdot 0 ) \\
&= ( -1, 0 ) \\
\\
\text{so, }( 0, 1 ) &= \sqrt{( -1, 0 )}
\end{align}$$

$( 0, 1 )$ is usually denoted by $i$, and sometimes $j$. That is, $i = (0, 1) = \sqrt{(-1, 0)}$. Despite this, you will almost always see it written, informally, as $i = \sqrt{-1}$.

## A Note on Notation

We often see complex numbers, such as $(a,b)$, written instead as $a+bi$.

To see why, first observe that multiplication of a complex number, $(c,d)$, by a real number, $a$, can be defined as follows:

$$\begin{align}
a \cdot (c,d) &\cong (a,0) \cdot (c,d) \\
&= (ac - 0 \cdot d,\ ad + 0 \cdot c) \\
&= (ac,\ ad)
\end{align}$$

Now, we can use this definition to factor out the real number, $b$, from the complex number, $(0,b)$, to obtain $b \cdot (0,1)$, as shown below:

$$\begin{align}
(a,b) &= (a,0) + (0,b) \\
&\cong (a,0) + b \cdot (0,1) \\
&\cong a + bi
\end{align}$$


```python

```
