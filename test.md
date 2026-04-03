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


<h3>Toy example of Vanilla DP, SlaClip, and Adap-Clip (C = 3/2, K = 4, λ = 3/4)</h3>

<table>
  <thead>
    <tr>
      <th>Step</th>
      <th>Vanilla DP toy gradient</th>
      <th>SlaClip toy gradient + indicator</th>
      <th>Adap-Clip toy gradient + extra query</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>1. Sorted toy batch</b></td>
      <td>
        g₁ = (1/2,0,0,0,0), ‖g₁‖ = 1/2<br>
        g₂ = (1,1/2,0,0,0), ‖g₂‖ = √5/2<br>
        g₃ = (1,1,1/2,0,0), ‖g₃‖ = 3/2<br>
        g₄ = (1,1,1,1/2,0), ‖g₄‖ = √13/2<br>
        g₅ = (1,1,1,1,1/2), ‖g₅‖ = √17/2
      </td>
      <td>
        Same toy batch, with C = 3/2, K = 4, λ = 3/4.
      </td>
      <td>
        Same toy batch. A separate private query will later be used for the clipping statistic.
      </td>
    </tr>

    <tr>
      <td><b>2. Clip at C = 3/2</b></td>
      <td>
        g₁ = (1/2,0,0,0,0)<br>
        g₂ = (1,1/2,0,0,0)<br>
        g₃ = (1,1,1/2,0,0)<br>
        g₄ = (3/√13, 3/√13, 3/√13, 3/(2√13), 0)<br>
        g₅ = (3/√17, 3/√17, 3/√17, 3/√17, 3/(2√17))
      </td>
      <td>
        Same clipped gradients.
      </td>
      <td>
        Same clipped gradients.
      </td>
    </tr>

    <tr>
      <td><b>3. Slack encoding / extra-query step</b></td>
      <td>
        No extra coordinates.
      </td>
      <td>
        Slack values are δ = max(C − ‖g‖, 0):<br>
        δ₁ = 1<br>
        δ₂ = (3 − √5)/2<br>
        δ₃ = 0<br>
        δ₄ = 0<br>
        δ₅ = 0<br><br>

        Using K = 4 and λ = 3/4, the ordered slack vectors are:<br>
        s₁ = (3/4, 1/4, 0, 0)<br>
        s₂ = ((3 − √5)/2, 0, 0, 0)<br>
        s₃ = (0, 0, 0, 0)<br>
        s₄ = (0, 0, 0, 0)<br>
        s₅ = (0, 0, 0, 0)<br><br>

        Extended vectors:<br>
        e₁ = (1/2,0,0,0,0; 3/4,1/4,0,0)<br>
        e₂ = (1,1/2,0,0,0; (3−√5)/2,0,0,0)<br>
        e₃ = (1,1,1/2,0,0; 0,0,0,0)<br>
        e₄ = (3/√13, 3/√13, 3/√13, 3/(2√13), 0; 0,0,0,0)<br>
        e₅ = (3/√17, 3/√17, 3/√17, 3/√17, 3/(2√17); 0,0,0,0)
      </td>
      <td>
        Keep the clipped gradients, and issue a separate private query for the clipping statistic.<br>
        For example, the unclipped fraction can be queried separately.
      </td>
    </tr>

    <tr>
      <td><b>4. Aggregate (before noise)</b></td>
      <td>
        ḡ = (1/5) Σ Clip(gᵢ)<br>
        = ( (3/2 + 3/√13 + 3/√17)/5,<br>
        &nbsp;&nbsp;(3/2 + 3/√13 + 3/√17)/5,<br>
        &nbsp;&nbsp;(1/2 + 3/√13 + 3/√17)/5,<br>
        &nbsp;&nbsp;(3/(2√13) + 3/√17)/5,<br>
        &nbsp;&nbsp;3/(10√17) )
      </td>
      <td>
        ē = (1/5) Σ eᵢ<br>
        = ( (3/2 + 3/√13 + 3/√17)/5,<br>
        &nbsp;&nbsp;(3/2 + 3/√13 + 3/√17)/5,<br>
        &nbsp;&nbsp;(1/2 + 3/√13 + 3/√17)/5,<br>
        &nbsp;&nbsp;(3/(2√13) + 3/√17)/5,<br>
        &nbsp;&nbsp;3/(10√17);<br>
        &nbsp;&nbsp;9/20 − √5/10, 1/20, 0, 0 )
      </td>
      <td>
        Gradient mean is the same as Vanilla DP,<br>
        together with a separate private query for the clipping statistic.
      </td>
    </tr>

    <tr>
      <td><b>5. Add Gaussian noise</b></td>
      <td>
        Release: g̃ = ḡ + z<sub>g</sub>
      </td>
      <td>
        Release: ẽ = ē + z<br>
        (same Gaussian query on the extended vector)
      </td>
      <td>
        Release: g̃ = ḡ + z<sub>g</sub><br>
        and separately q̃ = q + z<sub>q</sub>
      </td>
    </tr>

    <tr>
      <td><b>6. Normalize extra coordinates<br>(for illustration, before noise)</b></td>
      <td>
        —
      </td>
      <td>
        Slack part of ē = (9/20 − √5/10, 1/20, 0, 0)<br>
        Divide by λ = 3/4:<br>
        ŝ = (3/5 − 2√5/15, 1/15, 0, 0)
      </td>
      <td>
        The extra query statistic is released separately.
      </td>
    </tr>

    <tr>
      <td><b>7. Adaptive clipping update</b></td>
      <td>
        No adaptive clipping signal.
      </td>
      <td>
        The released cumulative signal can be plugged into Eq. (13)<br>
        to determine whether the current threshold C is too small or too large,<br>
        and then update C for the next step.
      </td>
      <td>
        The separate private query can also be used to update C,<br>
        but it requires an additional private release.
      </td>
    </tr>
  </tbody>
</table>
