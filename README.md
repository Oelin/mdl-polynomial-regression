# Polynomial Regression With MDL.

We apply crude MDL to efficiently regularize polynomial regression models.

An order $n$ polynomial can be unambiguously described using $n$ floating-point coefficients.  We use a simple concatenative code to represent this information:

#### $$C_M(M) = C_M(n,d,c_1,\dots,c_n) = C_U(d)||C_F(c_1)||\dots||C_F(c_n)$$ 

where:

* $c_1,\dots,c_n$ are the polynomial coefficients.
* $d$ is the precision of each coefficient.
* $C_U$ is a uniform binary code for unsigned integers.
* $C_F$ is a uniform binary code for floating-point numbers.

Then the model description length is given by

 $$L_M(M) = L_{N}(d) + nd$$
 
 Let $d \in [0, 2^k)$.
 
 Then $L_N(d) = k$ is constant, and we can safely exclude it from the description length, giving
 
 #### $$L_M(M) = nd$$

Once a polynomial has been specified, we can represent the data in a residual form: $(x, \epsilon)$ such that $\epsilon_i \sim \mathcal{N}(0, \sigma)$. We quantize $\epsilon$ to $d$ bits so that it's possible to compute exact probabilities for each residual. Then we can compress $\epsilon$ via arithmetic coding. So the data description length is given by

#### $$L_{D|M}(x, \epsilon) = -\log p_{D|M}(\epsilon) = -\sum \log \Phi\left(\epsilon_i \pm 0.5 \times 2^{-d}\right)$$

> The probability of $\epsilon$ is the product of individual residual probabilities. This follows from the assumption that residuals are i.i.d. Also note that we exclude $L(x)$ as it's constant.

Then the complete description length is given by 

#### $$L_{M,D}(n,d,c_1,\dots,c_n,x,y) = nd -\sum_{i=1}^n \log \Phi\left[(M(x_i) - y_i) \pm 0.5 \times 2^{-d}\right]$$
