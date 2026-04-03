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
