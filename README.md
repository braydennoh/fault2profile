# fault2profile

At first order, topography is a product of tectonic uplift and erosion; therefore, there must be some coupling between slip on a fault and river topography. River topography has a signal-to-noise ratio that is orders of magnitude higher than that of fault geometry (and is freely available as DEM, unlike physical sensors such as scattered-wave imaging), so some kind of inversion scheme might help constrain the fault geometry.

Consider the stream powre ewquation dz/dt = U - KA^M S^N, in which the change of elevation is the net difference between uplift and erosion, which is commonly proxied by KAMSN, where K is some erodibility coefficient, A is ddrainage area and S is slope, 

## Stream Power Inversion Formulation

We adopt the steady-state stream power equation

$$
\frac{\partial z}{\partial t} = U - K A^m S^n,
$$

where $U$ is rock uplift, $K$ is an erodibility coefficient, $A$ is drainage area, and $S = \partial z / \partial s$ is channel slope along the streamwise coordinate $s$. At steady state,

$$
U = K A^m S^n.
$$

Rather than prescribing $K$, we treat uplift $U(s)$ as the quantity predicted from fault kinematics and solve analytically for the topographic profile implied by steady incision.

Rearranging,

$$
S(s) = \left( \frac{U(s)}{K A(s)^m} \right)^{1/n}.
$$

Integrating from the outlet ($s = 0$),

$$
z(s) = z_0 + \int_0^s
\left( \frac{U(\xi)}{K A(\xi)^m} \right)^{1/n} d\xi.
$$

Define the geometry-only integral

$$
I(s) = \int_0^s
\left( \frac{U(\xi)}{A(\xi)^m} \right)^{1/n} d\xi,
$$

which depends only on uplift and drainage geometry. Then

$$
z(s) = z_0 + K^{-1/n} I(s).
$$

Thus, for any candidate fault geometry:

1. Fault control points define a $C^2$ continuous surface.
2. Structural slip is converted to vertical uplift $U(x)$.
3. Uplift is projected onto the channel and $I(s)$ is computed.
4. $K$ is solved analytically by least squares:

$$
a = \frac{I \cdot (z_{\text{obs}} - z_0)}{I \cdot I},
\qquad
K = a^{-n}.
$$

The inversion therefore does **not** search over $K$.  
It searches over fault geometry (and optionally $m,n$), computes the implied uplift field, constructs the steady-state stream power profile, and fits $K$ in closed form.

The misfit minimized in the MCMC stage is

$$
\chi^2 = \sum_i w_i
\left( \frac{z_{\text{obs},i} - z_{\text{pred},i}}{\sigma_z} \right)^2,
$$

where $w_i$ reflects node spacing along the channel.

In summary, the stream power equation enters the inversion as a steady-state forward operator mapping uplift (predicted from fault geometry) to river profile shape, while erodibility $K$ is solved analytically for each trial geometry.

