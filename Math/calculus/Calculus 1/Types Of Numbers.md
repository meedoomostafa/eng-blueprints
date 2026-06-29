## 1. The Number Hierarchy

- **Natural Numbers ($\mathbb{N}$):** Counting numbers starting from 1.$$\mathbb{N} = \{1, 2, 3, 4, \dots\}$$
    - _Limitation:_ Lacks $0$, negative numbers, and fractions.

- **Whole Numbers ($W$):** Natural numbers plus zero.$$W = \{0, 1, 2, 3, \dots\}$$
- **Integers ($\mathbb{Z}$):** Whole numbers plus negative numbers.$$\mathbb{Z} = \{\dots, -2, -1, 0, 1, 2, \dots\}$$
- **Rational Numbers ($\mathbb{Q}$):** Any number expressible as a fraction $\frac{p}{q}$ (where $q \neq 0$). Includes terminating decimals ($1.5$) and repeating decimals ($0.333\dots$).

## 2. Irrational Numbers & The Real Line

- **Irrational Numbers:** Numbers that cannot be written as a fraction. Their decimals never end and never repeat.
    - _Examples:_ $\pi$, $e$, $\sqrt{2}$
        
- **The Conflict:** They physically exist in geometry (e.g., the hypotenuse of a $1 \times 1$ right triangle is $\sqrt{2}$), but the standard 12 arithmetic properties cannot determine or define their exact values.
    
- **Real Numbers ($\mathbb{R}$):** The complete set formed by combining all Rational and Irrational numbers together.

## 3. Proof: $\sqrt{2}$ is Irrational (Contradiction)

1. Assume $\sqrt{2}$ **is** rational, written in its simplest form as $\frac{p}{q}$.
2. Square both sides: $2 = \frac{p^2}{q^2} \implies p^2 = 2q^2$. This means $p^2$ is even, so **$p$ must be even**.
3. Let $p = 2k$. Substitute it back: $(2k)^2 = 2q^2 \implies 4k^2 = 2q^2 \implies 2k^2 = q^2$. This means $q^2$ is even, so **$q$ must be even**.
4. **Contradiction:** If both $p$ and $q$ are even, the fraction $\frac{p}{q}$ was not in its simplest form. Therefore, the assumption is false, and $\sqrt{2}$ is irrational.

## 4. Key Takeaway

Algebraic properties alone cannot fix the gaps in the number line. To actually define and understand the continuum of Real Numbers ($\mathbb{R}$), we must transition from basic arithmetic to the study of **Functions** and **Limits**.

## 5. Summary
![[NumbersType.png]]