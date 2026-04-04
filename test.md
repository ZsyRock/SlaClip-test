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


html <h3>Toy pipeline example (C<sub>t</sub> = 5, K = 5)</h3> <p> Below is a toy step-by-step illustration of the full pipeline. The SlaClip column shows how the same Gaussian release can carry both the clipped gradient and a cumulative indicator, whereas the Adap-Clip column uses a separate private query for the clipping statistic. </p> <table> <thead> <tr> <th>Steps</th> <th>Vanilla DP-SGD</th> <th>SlaClip</th> <th>Adap-Clip</th> </tr> </thead> <tbody> <tr> <td><b>Step I: Gradient computation</b></td> <td> g<sub>14</sub> = (14)<br> g<sub>13</sub> = (13)<br> g<sub>12</sub> = (12)<br> g<sub>11</sub> = (11)<br> g<sub>10</sub> = (10)<br> g<sub>9</sub> = (9)<br> g<sub>8</sub> = (8)<br> g<sub>7</sub> = (7)<br> g<sub>6</sub> = (6)<br> g<sub>5</sub> = (5)<br> g<sub>4</sub> = (4)<br> g<sub>3</sub> = (3)<br> g<sub>2</sub> = (2)<br> g<sub>1</sub> = (1)<br> g<sub>0</sub> = (0) </td> <td> g′<sub>14</sub> = (14; <b>0,0,0,0,0</b>)<br> g′<sub>13</sub> = (13; <b>0,0,0,0,0</b>)<br> g′<sub>12</sub> = (12; <b>0,0,0,0,0</b>)<br> g′<sub>11</sub> = (11; <b>0,0,0,0,0</b>)<br> g′<sub>10</sub> = (10; <b>0,0,0,0,0</b>)<br> g′<sub>9</sub> = (9; <b>0,0,0,0,0</b>)<br> g′<sub>8</sub> = (8; <b>0,0,0,0,0</b>)<br> g′<sub>7</sub> = (7; <b>0,0,0,0,0</b>)<br> g′<sub>6</sub> = (6; <b>0,0,0,0,0</b>)<br> g′<sub>5</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>4</sub> = (4; <b>0,0,0,0,0</b>)<br> g′<sub>3</sub> = (3; <b>0,0,0,0,0</b>)<br> g′<sub>2</sub> = (2; <b>0,0,0,0,0</b>)<br> g′<sub>1</sub> = (1; <b>0,0,0,0,0</b>)<br> g′<sub>0</sub> = (0; <b>0,0,0,0,0</b>) </td> <td> Same raw gradients as Vanilla DP-SGD.<br> The clipping statistic will be obtained by a separate private query later. </td> </tr> <tr> <td><b>Step II: Per-sample l<sub>2</sub> clipping</b></td> <td> g<sub>14</sub> = (5)<br> g<sub>13</sub> = (5)<br> g<sub>12</sub> = (5)<br> g<sub>11</sub> = (5)<br> g<sub>10</sub> = (5)<br> g<sub>9</sub> = (5)<br> g<sub>8</sub> = (5)<br> g<sub>7</sub> = (5)<br> g<sub>6</sub> = (5)<br> g<sub>5</sub> = (5)<br> g<sub>4</sub> = (4)<br> g<sub>3</sub> = (3)<br> g<sub>2</sub> = (2)<br> g<sub>1</sub> = (1)<br> g<sub>0</sub> = (0) </td> <td> g′<sub>14</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>13</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>12</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>11</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>10</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>9</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>8</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>7</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>6</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>5</sub> = (5; <b>0,0,0,0,0</b>)<br> g′<sub>4</sub> = (4; <b>1,0,0,0,0</b>)<br> g′<sub>3</sub> = (3; <b>1,1,0,0,0</b>)<br> g′<sub>2</sub> = (2; <b>1,1,1,0,0</b>)<br> g′<sub>1</sub> = (1; <b>1,1,1,1,0</b>)<br> g′<sub>0</sub> = (0; <b>1,1,1,1,1</b>) </td> <td> Same clipped gradients as Vanilla DP-SGD.<br> The clipping statistic is not encoded into the same release; it will be queried separately. </td> </tr> <tr> <td><b>Step III: Aggregate, noise and release.</b></td> <td> Mean clipped gradient:<br> g<sub>bar</sub> = (5×10 + 4 + 3 + 2 + 1 + 0) / 15 = 60 / 15<br><br> Release:<br> g<sub>tild</sub> = g<sub>bar</sub> + z<sub>g</sub> </td> <td> Mean extended vector:<br> g′<sub>bar</sub> = (60/15; <b>5/15, 4/15, 3/15, 2/15, 1/15</b>)<br><br> Same-query release:<br> g′<sub>tild</sub> = g′<sub>bar</sub> + z </td> <td> Gradient release:<br> g<sub>tild</sub> = g<sub>bar</sub> + z<sub>g</sub><br><br> Separate private query for unclipped ratio:<br> q = 5 / 15<br> q<sub>tild</sub> = q + z<sub>q</sub> </td> </tr> <tr> <td><b>Step IV: Privacy accounting and model update.</b></td> <td> Privacy accountant is applied to the gradient release only.<br> Update model using g<sub>tild</sub>.<br> C<sub>t</sub> stays fixed. </td> <td> Privacy accountant is applied once to the same-query extended release.<br> Use the original gradient part of g′<sub>tild</sub> to update the model.<br> Use the extra 5 coordinates as an indicator:<br> <b>(5/15, 4/15, 3/15, 2/15, 1/15)</b><br> For example, if the target unclipped ratio is 1/2 but the current estimate is about 5/15,<br> then increase C<sub>t</sub> at the next step. </td> <td> Privacy accountant composes two releases:<br> (i) gradient release and (ii) separate clipping-statistic query.<br> Update model using g<sub>tild</sub>.<br> Update C<sub>t</sub> using q<sub>tild</sub>. </td> </tr> </tbody> </table>


html <h3>Toy pipeline example (C<sub>t</sub> = 5, K = 5)</h3>

<p>
Below is a toy step-by-step illustration of the full pipeline.
The SlaClip column shows how the same Gaussian release can carry both the clipped gradient and a cumulative indicator,
whereas the Adap-Clip column uses a separate private query for the clipping statistic.
</p>
<table>
  <thead>
    <tr>
      <th>Steps</th>
      <th>Vanilla DP-SGD</th>
      <th>SlaClip</th>
      <th>Adap-Clip</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>Step I: Gradient computation</b></td>
      <td>
        g<sub>14</sub> = (14)<br>
        g<sub>13</sub> = (13)<br>
        g<sub>12</sub> = (12)<br>
        g<sub>11</sub> = (11)<br>
        g<sub>10</sub> = (10)<br>
        g<sub>9</sub> = (9)<br>
        g<sub>8</sub> = (8)<br>
        g<sub>7</sub> = (7)<br>
        g<sub>6</sub> = (6)<br>
        g<sub>5</sub> = (5)<br>
        g<sub>4</sub> = (4)<br>
        g<sub>3</sub> = (3)<br>
        g<sub>2</sub> = (2)<br>
        g<sub>1</sub> = (1)<br>
        g<sub>0</sub> = (0)
      </td>
      <td>
        g′<sub>14</sub> = (14; <b>0,0,0,0,0</b>)<br>
        g′<sub>13</sub> = (13; <b>0,0,0,0,0</b>)<br>
        g′<sub>12</sub> = (12; <b>0,0,0,0,0</b>)<br>
        g′<sub>11</sub> = (11; <b>0,0,0,0,0</b>)<br>
        g′<sub>10</sub> = (10; <b>0,0,0,0,0</b>)<br>
        g′<sub>9</sub> = (9; <b>0,0,0,0,0</b>)<br>
        g′<sub>8</sub> = (8; <b>0,0,0,0,0</b>)<br>
        g′<sub>7</sub> = (7; <b>0,0,0,0,0</b>)<br>
        g′<sub>6</sub> = (6; <b>0,0,0,0,0</b>)<br>
        g′<sub>5</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>4</sub> = (4; <b>0,0,0,0,0</b>)<br>
        g′<sub>3</sub> = (3; <b>0,0,0,0,0</b>)<br>
        g′<sub>2</sub> = (2; <b>0,0,0,0,0</b>)<br>
        g′<sub>1</sub> = (1; <b>0,0,0,0,0</b>)<br>
        g′<sub>0</sub> = (0; <b>0,0,0,0,0</b>)
      </td>
      <td>
        Same raw gradients as Vanilla DP-SGD.<br>
        A counter is maintained to count unclipped samples and estimate the clipping ratio.<br>
        (In this toy example, the counter value is 5.)
      </td>
    </tr>
    <tr>
      <td><b>Step II: Per-sample l<sub>2</sub> clipping</b></td>
      <td>
        g<sub>14</sub> = (5)<br>
        g<sub>13</sub> = (5)<br>
        g<sub>12</sub> = (5)<br>
        g<sub>11</sub> = (5)<br>
        g<sub>10</sub> = (5)<br>
        g<sub>9</sub> = (5)<br>
        g<sub>8</sub> = (5)<br>
        g<sub>7</sub> = (5)<br>
        g<sub>6</sub> = (5)<br>
        g<sub>5</sub> = (5)<br>
        g<sub>4</sub> = (4)<br>
        g<sub>3</sub> = (3)<br>
        g<sub>2</sub> = (2)<br>
        g<sub>1</sub> = (1)<br>
        g<sub>0</sub> = (0)
      </td>
      <td>
        g′<sub>14</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>13</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>12</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>11</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>10</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>9</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>8</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>7</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>6</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>5</sub> = (5; <b>0,0,0,0,0</b>)<br>
        g′<sub>4</sub> = (4; <b>1,0,0,0,0</b>)<br>
        g′<sub>3</sub> = (3; <b>1,1,0,0,0</b>)<br>
        g′<sub>2</sub> = (2; <b>1,1,1,0,0</b>)<br>
        g′<sub>1</sub> = (1; <b>1,1,1,1,0</b>)<br>
        g′<sub>0</sub> = (0; <b>1,1,1,1,1</b>)
      </td>
      <td>
        Same clipped gradients as Vanilla DP-SGD.<br>
        The clipping statistic is not encoded into the same release; it will be queried separately.
      </td>
    </tr>
    <tr>
      <td><b>Step III: Aggregate, noise and release.</b></td>
      <td>
        g<sub>avg</sub> = (4)<br>
        Release:<br>
        g<sub>noise</sub> = (4.01)
      </td>
      <td>
        g′<sub>avg</sub> = (4; <b>5/15, 4/15, 3/15, 2/15, 1/15</b>)<br>
        Release:<br>
        g′<sub>noise</sub> = (4.01; <b>5/15, 4/15, 3/15, 2/15, 1/15</b>) + N′
      </td>
      <td>
        Gradient release:<br>
        g<sub>noise</sub> = g<sub>avg</sub> + z<sub>g</sub><br><br>
        Separate private query for unclipped ratio:<br>
        q = 5 / 15<br>
        q<sub>tild</sub> = q + z<sub>q</sub>
      </td>
    </tr>
    <tr>
      <td><b>Step IV: Privacy accounting and model update.</b></td>
      <td>
        Update model using g<sub>noise</sub>, C<sub>t</sub> stays fixed.
      </td>
      <td>
        Update model using g<sub>noise</sub>, C<sub>t</sub> adjusted by <b>(5/15, 4/15, 3/15, 2/15, 1/15)</b>.
      </td>
      <td>
        Privacy accountant composes two releases:<br>
        (i) gradient release and (ii) separate clipping-statistic query.<br>
        Update model using g<sub>noise</sub>.<br>
        Update C<sub>t</sub> using q<sub>tild</sub>.
      </td>
    </tr>
  </tbody>
</table>
