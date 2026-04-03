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


html <h3>Toy example of Vanilla DP vs. SlaClip (C = 1, K = 4, λ = 1/2)</h3> <table> <thead> <tr> <th>Step</th> <th>vanillaDP</th> <th>SlaClip</th> </tr> </thead> <tbody> <tr> <td><b>1. Calculate ‖g‖</b></td> <td> g₁ = (0,0,0,0,0), ‖g₁‖ = 0<br> g₂ = (1,0,0,0,0), ‖g₂‖ = 1<br> g₃ = (1,1,0,0,0), ‖g₃‖ = √2<br> g₄ = (1,1,1,0,0), ‖g₄‖ = √3<br> g₅ = (1,1,1,1,0), ‖g₅‖ = 2 </td> <td> g₁ = (0,0,0,0,0,0,0,0,0)<br> g₂ = (1,0,0,0,0,0,0,0,0)<br> g₃ = (1,1,0,0,0,0,0,0,0)<br> g₄ = (1,1,1,0,0,0,0,0,0)<br> g₅ = (1,1,1,1,0,0,0,0,0) </td> </tr> <tr> <td><b>2. Clip (C = 1) + Slack encoding</b></td> <td> g₁ = (0,0,0,0,0)<br> g₂ = (1,0,0,0,0)<br> g₃ = (1/√2, 1/√2, 0, 0, 0)<br> g₄ = (1/√3, 1/√3, 1/√3, 0, 0)<br> g₅ = (1/2, 1/2, 1/2, 1/2, 0) </td> <td> g₁ = (0,0,0,0,0, 1/2,1/2,1/2,1/2)<br> g₂ = (1,0,0,0,0, 0,0,0,0)<br> g₃ = (1/√2, 1/√2, 0, 0, 0, 0,0,0,0)<br> g₄ = (1/√3, 1/√3, 1/√3, 0, 0, 0,0,0,0)<br> g₅ = (1/2, 1/2, 1/2, 1/2, 0, 0,0,0,0) </td> </tr> <tr> <td><b>3. Aggregate (before noise)</b></td> <td> ḡ = (<br> &nbsp;&nbsp;(1 + 1/√2 + 1/√3 + 1/2)/5,<br> &nbsp;&nbsp;(1/√2 + 1/√3 + 1/2)/5,<br> &nbsp;&nbsp;(1/√3 + 1/2)/5,<br> &nbsp;&nbsp;1/10,<br> &nbsp;&nbsp;0) </td> <td> ē = (<br> &nbsp;&nbsp;(1 + 1/√2 + 1/√3 + 1/2)/5,<br> &nbsp;&nbsp;(1/√2 + 1/√3 + 1/2)/5,<br> &nbsp;&nbsp;(1/√3 + 1/2)/5,<br> &nbsp;&nbsp;1/10,<br> &nbsp;&nbsp;0,<br> &nbsp;&nbsp;1/10, 1/10, 1/10, 1/10) </td> </tr> <tr> <td><b>4. Add Gaussian noise</b></td> <td> release: g̃ = ḡ + z<sub>g</sub> </td> <td> release: ẽ = ē + z<br> (same Gaussian query on the extended vector) </td> </tr> <tr> <td><b>5. Normalize extra coordinates<br>(for illustration, before noise)</b></td> <td> — </td> <td> slack part of ē = (1/10, 1/10, 1/10, 1/10)<br> divide by λ = 1/2:<br> ŝ = (1/5, 1/5, 1/5, 1/5) </td> </tr> <tr> <td><b>6. Adaptive clipping update</b></td> <td> no adaptive clipping signal </td> <td> The estimated cumulative signal near C is small.<br> If the target unclipped ratio is 1/2 but the current estimate is about 1/5,<br> then increase C at the next step. </td> </tr> </tbody> </table> 
