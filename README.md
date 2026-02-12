# fault2profile

At first order, topography is a product of tectonic uplift and erosion; therefore, there must be some coupling between slip on a fault and river topography. River topography has a signal-to-noise ratio that is orders of magnitude higher than that of fault geometry (and is freely available as DEM, unlike physical sensors such as scattered-wave imaging), so some kind of inversion scheme might help constrain the fault geometry.

Consider the stream power equation:

$$
\frac{\partial z}{\partial t} = U - K A^m S^n,
$$

where $U$ is rock uplift, $K$ is an erodibility coefficient, $A$ is drainage area, and $S = \partial z / \partial s$ is channel slope along the streamwise coordinate $s$. At steady state,

$$
U = K A^m S^n.
$$

$K$ is a parameter whose interpretation depends on assumptions about the relative importance of lithologic variability. However, for large rivers (length scales >100 km), the characteristic wavelength of channel profiles  exceeds lithologic heterogeneity. Empirically, large rivers often exhibit profile geometries that correlate more strongly with tectonic forcing than with mapped lithologic variations (e.g., G. Roberts). While lithologic maps could be used to spatially constrain erodibility, doing so requires strong assumptions about stratigraphic continuity, which will introduce greater prior bias and misplaced confidence than adopting a relatively uninformative prior on $K$.

Therefore, $K$ is not treated as a primary inversion parameter. Instead, for each candidate uplift field predicted from fault geometry, $K$ is solved analytically. Rearranging the steady-state condition,

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

$$
a = \frac{I \cdot (z_{\text{obs}} - z_0)}{I \cdot I},
\qquad
K = a^{-n}.
$$

The inversion therefore does search over $K$.  
It searches over fault geometry (and optionally $m,n$), computes the implied uplift field, constructs the steady-state stream power profile, and fits $K$ in closed form.

Some form of misfit minimized for an inversion model is:

$$
\chi^2 = \sum_i w_i
\left( \frac{z_{\text{obs},i} - z_{\text{pred},i}}{\sigma_z} \right)^2,
$$

where $w_i$ reflects node spacing along the channel.
