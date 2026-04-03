| Sample | Original gradient \(g_i\) | \(\|g_i\|_2\) | Clipped gradient \(\mathrm{Clip}_C(g_i)\) |
|---|---|---:|---|
| 1 | \((2,2,2,2,0)\) | \(4\) | \((1,1,1,1,0)\) |
| 2 | \((2,2,2,0,0)\) | \(2\sqrt{3}\approx 3.464\) | \((\tfrac{2}{\sqrt3},\tfrac{2}{\sqrt3},\tfrac{2}{\sqrt3},0,0)\approx(1.1547,1.1547,1.1547,0,0)\) |
| 3 | \((2,2,0,0,0)\) | \(2\sqrt{2}\approx 2.828\) | \((\sqrt2,\sqrt2,0,0,0)\approx(1.4142,1.4142,0,0,0)\) |
| 4 | \((2,0,0,0,0)\) | \(2\) | \((2,0,0,0,0)\) |
| 5 | \((0,0,0,0,0)\) | \(0\) | \((0,0,0,0,0)\) |


| Sample | \(\max(C-\|g_i\|,0)\) | Extra 4-d slack vector \(s_i^{(4)}\) | Extended vector \([\mathrm{Clip}_C(g_i);\, s_i^{(4)}]\) |
|---|---:|---|---|
| 1 | \(0\) | \((0,0,0,0)\) | \((1,1,1,1,0;\ 0,0,0,0)\) |
| 2 | \(0\) | \((0,0,0,0)\) | \((\tfrac{2}{\sqrt3},\tfrac{2}{\sqrt3},\tfrac{2}{\sqrt3},0,0;\ 0,0,0,0)\) |
| 3 | \(0\) | \((0,0,0,0)\) | \((\sqrt2,\sqrt2,0,0,0;\ 0,0,0,0)\) |
| 4 | \(0\) | \((0,0,0,0)\) | \((2,0,0,0,0;\ 0,0,0,0)\) |
| 5 | \(2\) | \((1,1,1,1)\) | \((0,0,0,0,0;\ 1,1,1,1)\) |

| Aggregated quantity | Exact value | Decimal approximation |
|---|---|---|
| Mean clipped gradient (dim 1) | \(\frac{1+\frac{2}{\sqrt3}+\sqrt2+2}{5}\) | \(1.1138\) |
| Mean clipped gradient (dim 2) | \(\frac{1+\frac{2}{\sqrt3}+\sqrt2}{5}\) | \(0.7138\) |
| Mean clipped gradient (dim 3) | \(\frac{1+\frac{2}{\sqrt3}}{5}\) | \(0.4309\) |
| Mean clipped gradient (dim 4) | \(\frac{1}{5}\) | \(0.2000\) |
| Mean clipped gradient (dim 5) | \(0\) | \(0.0000\) |
| Mean slack coord 1 | \(\frac{1}{5}\) | \(0.2000\) |
| Mean slack coord 2 | \(\frac{1}{5}\) | \(0.2000\) |
| Mean slack coord 3 | \(\frac{1}{5}\) | \(0.2000\) |
| Mean slack coord 4 | \(\frac{1}{5}\) | \(0.2000\) |



| Step | Vanilla DP toy gradient | SlaClip toy gradient + indicator | Adap-Clip toy gradient + extra query |
|---|---|---|---|
| **1. Sorted toy batch** | \(g_1=(2,2,2,2,0),\ \|g_1\|_2=4\)  \newline \(g_2=(2,2,2,0,0),\ \|g_2\|_2=2\sqrt3\)  \newline \(g_3=(2,2,0,0,0),\ \|g_3\|_2=2\sqrt2\)  \newline \(g_4=(2,0,0,0,0),\ \|g_4\|_2=2\)  \newline \(g_5=(0,0,0,0,0),\ \|g_5\|_2=0\) | Same toy batch, with \(C=2,\ K=4,\ \lambda=1\). | Same toy batch. A separate private query will later be used for the clipped-ratio statistic. |
| **2. Clip at \(C=2\)** | \(\mathrm{Clip}_C(g_1)=(1,1,1,1,0)\)  \newline \(\mathrm{Clip}_C(g_2)=\left(\frac{2}{\sqrt3},\frac{2}{\sqrt3},\frac{2}{\sqrt3},0,0\right)\)  \newline \(\mathrm{Clip}_C(g_3)=(\sqrt2,\sqrt2,0,0,0)\)  \newline \(\mathrm{Clip}_C(g_4)=(2,0,0,0,0)\)  \newline \(\mathrm{Clip}_C(g_5)=(0,0,0,0,0)\) | Same clipped gradients. | Same clipped gradients. |
| **3. Slack encoding / extra-query step** | No extra coordinates. | Clipped norms are \(2,2,2,2,0\), so the slacks are \(0,0,0,0,2\).  \newline With the ordered slack decomposition:  \newline \(s_1=(0,0,0,0)\)  \newline \(s_2=(0,0,0,0)\)  \newline \(s_3=(0,0,0,0)\)  \newline \(s_4=(0,0,0,0)\)  \newline \(s_5=(1,1,0,0)\)  \newline Hence the extended vectors are  \newline \(e_1=(1,1,1,1,0;\ 0,0,0,0)\)  \newline \(e_2=\left(\frac{2}{\sqrt3},\frac{2}{\sqrt3},\frac{2}{\sqrt3},0,0;\ 0,0,0,0\right)\)  \newline \(e_3=(\sqrt2,\sqrt2,0,0,0;\ 0,0,0,0)\)  \newline \(e_4=(2,0,0,0,0;\ 0,0,0,0)\)  \newline \(e_5=(0,0,0,0,0;\ 1,1,0,0)\) | Keep the clipped gradients, and issue a separate clipped-ratio query, e.g.  \newline \(q_t=\frac15\sum_{i=1}^5 \mathbf 1[\|g_i\|_2<C]=\frac15.\) |
| **4. Aggregate before noise** | \(\bar g=\frac15\sum_{i=1}^5 \mathrm{Clip}_C(g_i)\)  \newline \(=\left(\frac{1+\frac{2}{\sqrt3}+\sqrt2+2}{5},\ \frac{1+\frac{2}{\sqrt3}+\sqrt2}{5},\ \frac{1+\frac{2}{\sqrt3}}{5},\ \frac15,\ 0\right)\) | \(\bar e=\frac15\sum_{i=1}^5 e_i\)  \newline \(=\left(\frac{1+\frac{2}{\sqrt3}+\sqrt2+2}{5},\ \frac{1+\frac{2}{\sqrt3}+\sqrt2}{5},\ \frac{1+\frac{2}{\sqrt3}}{5},\ \frac15,\ 0;\ \frac15,\frac15,0,0\right)\) | Gradient mean is the same as Vanilla DP, together with the separate query value \(q_t=\frac15\). |
| **5. Add Gaussian noise and form the released signal** | Release \(\tilde g=\bar g+z_g\). | Release \(\tilde e=\bar e+z\).  \newline The auxiliary part before noise is \(\left(\frac15,\frac15,0,0\right)\).  \newline Since \(\lambda=1\), the pre-noise Slack Indicator is  \newline \(\hat s_t=\left(\frac15,\frac15,0,0\right)\). | Release \(\tilde g=\bar g+z_g\) and, separately, \(\tilde q_t=q_t+z_q\). |
| **6. Adaptive update** | No adaptive clipping update. | The released cumulative signal is very small near the current threshold, meaning that the below-threshold mass is very small. If the target unclipped ratio is \(1/2\), then the controller should **increase \(C\)** at the next step. | Since \(q_t=\frac15<\frac12\), the extra query also says that the current threshold is too small, so \(C\) should be increased. |
