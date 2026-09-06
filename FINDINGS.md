# Findings

Each result below is stated, evidenced, and assessed. The assessment is deliberately
unsparing: some of these are substantial, some are corollaries, and one is a routine
numerical check reported for completeness. Knowing which is which is part of the work.

Notation throughout: $f$ is the bending moment permitted, as a fraction of what the
elliptical loading produces. $C_n$ denotes the bending influence coefficients — $I_n$
for root bending, $K_n$ for integrated bending.

---

## 1 · The trade-off is quadratic, in closed form

$$\frac{C_{D,i}}{C_{D,i,\text{ell}}} = 1 + c\,(1-f)^2,
\qquad c = \frac{C_1^2}{2\sum_{n\geq3} C_n^2/(2n)}$$

**Evidence.** Derived by substituting the stationarity condition
$A_n = \lambda C_n/(2n)$ into the objective, then eliminating $\lambda$ through the
constraint. Checked against 62 independently computed sweep rows — 31 per criterion —
with a maximum discrepancy of $4\times10^{-16}$.

**Assessment.** The strongest result here. It converts a numerical study into a closed
form: the entire trade-off, for any bending criterion, is one number. The two routes
to it are genuinely independent, since the sweep calls the optimiser row by row while
the formula does not, so their agreement is a real check rather than a restatement.

**What it does not claim.** It presumes the loading-level framework. It says nothing
about which $f$ a designer should choose, because that depends on structural weight
saved, which is outside this model.

---

## 2 · The two criteria differ by exactly 8/3

$$c_I = 8, \qquad c_K = 3, \qquad \frac{c_I}{c_K} = \frac{8}{3}$$

**Evidence.** $c_K = 3$ falls out algebraically: only $K_3$ survives the denominator
and $K_1 = K_3 = \pi/16$, so the $\pi$ cancels entirely. $c_I$ converges to 8 —
8.018182 truncated at $n = 7$, 8.0000000002 at $n = 801$, using the analytic form for
$I_n$ to carry the sum past anything numerical integration could resolve.

**Assessment.** Strong, and the most quotable number in the project. Two rational
constants arising from what look like unrelated integrals is the kind of result that
invites the question of why — and I do not have a clean answer to that, which is
stated here rather than glossed.

**Reading it physically.** $c$ is fixed obligation over available leverage: the bending
that the lift-pinned first harmonic forces, divided by what the free harmonics can
offer, each discounted by its drag cost $n$. Root bending's free harmonics are weak
levers, so relief under it is expensive.

---

## 3 · Prandtl's bell is not a distinguished shape

Under an integrated-bending constraint the optimum is

$$\frac{A_3}{A_1} = f - 1 \quad \text{exactly, for all } f$$

so the optima form a one-parameter family, and the bell is where $f = 2/3$ lands.

**Evidence.** Verified numerically at $f = 0.9,\ 2/3,\ 0.5$, returning $-0.1$,
$-1/3$, $-0.5$. Proved algebraically in two lines from $K_1 = K_3$ and $K_n = 0$ for
$n \geq 5$.

**Assessment.** The most interesting result here, and the one most likely to be new to
a reader. It reframes a canonical 1933 result as one point on a continuum rather than
a distinguished shape. The 2/3 carries no special aerodynamic status.

**Caveat, stated plainly.** This is a reframing, not a correction. Prandtl's result
remains exactly right for the problem he posed. Whether the observation is already
standard in the literature is not something I have been able to establish, and it is
possible it is well known and simply not stated in the introductory sources available
to me.

---

## 4 · Under root bending, the bell is beaten

At identical lift and identical root bending moment ($f = 0.80$, where the bell itself
sits under this criterion):

| loading | $C_{D,i}$ ratio | $e$ |
|---|---:|---:|
| Prandtl bell | 1.3333 | 0.750 |
| root-bending optimum | 1.3207 | 0.757 |

**Evidence.** The bell's position at $f = 0.80$ under root bending is exact:
$1 - \tfrac13 (I_3/I_1) = 1 - \tfrac13 \cdot \tfrac35$. The comparison is therefore a
fair one, not a convenient round number.

**Assessment.** Correct but small — about 1%, and invisible in a plot of the two
loadings. It is a corollary of finding 3 rather than an independent result: the bell
optimises integrated bending, so of course something else optimises root bending. It
is listed separately because it was the question that motivated the project, not
because it is among the stronger outcomes.

---

## 5 · Integrated bending is blind above the third harmonic

$$K_1 = K_3 = \frac{\pi}{16}, \qquad K_n = 0 \ \ \forall\, n \geq 5$$

**Evidence.** Computed to $n = 11$; every coefficient above the third returns at
$10^{-18}$, i.e. numerical zero rather than a small nonzero value.

**Assessment.** Structural rather than headline, but it explains three other findings
at once. It is why the bell has only two terms, why finding 3's family is
one-parameter, and why $c_K$ is a clean integer. Root bending has $I_5 = -1/21$ and
$I_7 = 1/45$, both nonzero, which is the entire origin of the difference between the
two criteria.

---

## 6 · The quadratic form is forced, not fitted

Every coefficient scales with $(f-1)$ by a common factor, so the optimum travels along
a **fixed ray** in coefficient space: $f$ sets the distance travelled, the criterion
sets the direction. A quadratic form evaluated along a straight line is a parabola.

**Evidence.** Follows directly from $A_n = \lambda C_n/(2n)$ with
$\lambda \propto (f-1)$. Visible in the sweep tables, where $A_3/A_1$ is linear in $f$
under both criteria.

**Assessment.** Not a new number, but the reason finding 1 has the shape it does. The
quadratic form could have been predicted before any code ran. Its practical
consequence is that marginal cost rises linearly — the first few percent of structural
relief are nearly free, the last few are ruinous — which is what gives the trade-off a
natural stopping point rather than making it a matter of taste.

---

## 7 · Truncation at seven harmonics is defensible

| top $n$ | $c_I$ |
|---:|---:|
| 7 | 8.018182 |
| 13 | 8.001894 |
| 51 | 8.000010 |
| 801 | 8.000000 |

Truncating at $n = 7$ overestimates $c_I$ by **0.227%**.

**Evidence.** Computed both by numerical integration up to $n = 51$ and by the analytic
$I_n$ beyond that. Coefficient magnitudes fall roughly two decades between $n = 3$ and
$n = 13$, since $I_n \sim n^{-2}$ and each contributes $C_n^2/(2n) \sim n^{-5}$.

**Assessment.** Routine, and reported because it must be. A truncated series without a
convergence statement is not a result. This is the check that turns "seven seemed
enough" into a number.

---

## 8 · The influence coefficients have a closed form

$$I_n = -\frac{(-1)^{(n-1)/2}}{n^2 - 4}$$

**Evidence.** Derived by product-to-sum on $\sin n\theta \sin 2\theta$; the upper
integration limit vanishes for odd $n$ and only $\theta = \pi/2$ contributes. Matches
`np.trapezoid` to better than $10^{-9}$ across $n = 1$ to $13$.

**Assessment.** Minor in itself, but it does two jobs. It validates the numerical
integration independently rather than against supplied targets, and it makes finding 2
exact rather than approximate by allowing the truncation sum to run arbitrarily far.

**A detail worth noting.** The sign appears to break at $n = 1$: the values run
$+,+,-,+,-$. It does not. $n^2 - 4$ is negative only at $n = 1$, and that minus sign
absorbs the apparent exception. The formula holds with no special case.

---

## 9 · Span efficiency lands where real aircraft do

| loading | $e$ |
|---|---:|
| elliptical (unconstrained) | 1.000 |
| integrated bending, $f = 0.80$ | 0.893 |
| root bending, $f = 0.80$ | 0.757 |
| Prandtl bell | 0.750 |

**Evidence.** Computed from $\delta = \sum_{n\geq2} n(A_n/A_1)^2$ and $e = 1/(1+\delta)$
directly from the coefficients, then cross-checked against the drag ratio — the two
routes agree to $10^{-12}$ on every row.

**Assessment.** Weak as evidence, useful as a sanity check. Real wings run
$e \approx 0.7$–$0.85$, and the structurally constrained optimum sits inside that band
while the unconstrained textbook answer does not. That is suggestive that the
constraint is roughly the right size of effect. It is not a validation, since real $e$
values also absorb fuselage interference, and no attempt is made here to separate
those contributions.

---

## 10 · The result is Mach-invariant below $M_{cr}$

**Evidence.** Neither the objective nor the constraint contains $M_\infty$ or aspect
ratio; $AR$ enters only as a prefactor that cancels in every ratio. Verified by
rescaling $AR \to \beta AR$ with $\beta = \sqrt{1-M_\infty^2}$ for $M$ up to 0.8: drag
ratios unchanged to $2\times10^{-16}$.

**Assessment.** Cheap to obtain and honestly bounded. It confirms the premise the
argument rests on — that nothing in the optimisation chain sees aspect ratio — rather
than proving a compressibility result.

**The sharper half of the claim.** The optimal *loading* is Mach-invariant; the *wing*
producing it is not. Section lift-curve slope varies with $\beta$, so the twist and
planform needed to realise a given $\Gamma(y)$ shift with Mach even though the target
$\Gamma(y)$ does not. Above $M_{cr}$, shocks and wave drag break the potential-flow
framework the whole argument rests on, and nothing here applies.

---

## Where this stops

Every finding above is a statement about **spanwise loadings**, not about wings.
Recovering a planform and twist distribution that produce a given $\Gamma(y)$ is the
inverse problem: chord and local lift coefficient are two free functions with one
equation between them, so infinitely many wings produce any target loading, and
choosing among them requires constraints this model does not contain.

Bending moment is also a proxy for structural cost rather than structural cost itself.
Whether root or integrated bending is the more honest criterion depends on whether the
spar is sized by peak stress at the root or by total material along the span — a real
engineering question that this work poses rather than answers.

**In strength order:** findings 1, 2 and 3 are the substantive ones. Findings 5, 6 and
8 are structural, explaining why the others come out as they do. Findings 4, 7, 9 and
10 are checks and corollaries, reported for completeness.
