# Specification: boundary volume-flux diagnostic

Complete and commit this document BEFORE supplying it to a coding agent.
This is a student template; the existing `SCIENTIFIC_SPEC.md` describes the
instructor's separate derivative-verification example.

## Physical problem

Compute net outward volume flux per unit depth around a rectangular 2-D 
velocity field. Explain its connection to incompressible mass conservation
and why zero global flux does not imply zero local divergence.

Student explanation: zero global flux does not imply local divergence. 
Because Q is the integral of the divergence, any positive and negative divergences will cancel. 
This will give Q=0 but not del dot u = zero.

## Inputs and outputs

Function: `net_volume_flux(x, y, u, v) -> float`.
Coordinates: finite, strictly increasing 1-D arrays with at least two points.
Velocities: finite arrays of shape `(len(y), len(x))`; `field[y,x]` semantics.
Output: signed outward boundary integral using composite trapezoidal quadrature.
Write the four signed boundary integrals explicitly: 

Student answer: 

right =  u[:, -1], y ; 
left = u[:, 0], y ; 
top = v[-1, :], x ; 
bottom = v[0, :], x

## Units and nondimensionalization

For x,y in metres and u,v in m/s, volume flux per depth has units m squared/s.
With x*=x/L and u*=u/U, Q*=Q/(U L). Multiplication by a constant density gives
mass flux per unit depth. This function does not integrate variable density.
State the units and characteristic U,L for every proposed test: 

Student answer: Q has units m^2/s, L has units m, and U has units m/s.

## Validity

Axis-aligned rectangular domains, collocated boundary velocities, fixed mesh.
Composite trapezoidal quadrature; increasing nonuniform coordinates allowed.
No moving-boundary, compressible mass-flux, curved or unstructured-grid claim.
Explain discretization error for nonlinear boundary traces: 

Student explanation: Discretization error for nonlinear boundary traces is the 
numerical error accumulated from approximating the average rate of change from 
a set of sample points in a system with curved or moving boundaries.


## Frozen numerical acceptance criteria

Use absolute error <=1e-12 for the exactly integrable normalized reference
cases in ASSIGNMENT.md. Existing Week 1.1 acceptance gates must still pass.
For your additional nonuniform/rectangular test, derive its exact reference
and justify a numerical tolerance before implementation: 

Student explanation: (done by hand) 

using u = 3x + y and v = x +2y, and sample points: 
[0, 0.2, 2.4, 5]for x and [0, 0.6, 2.2, 3.2] for y

using calculus, Q = 80, taking the integral of the divergence theorem, 
the gradient of u is 5 and the area is 16, Q is again = to 80. 

I originally chose a tolerance of 1e-12 for no particular reason other 
than it was used for the example provided in ASSIGNMENT.MD. I wanted my 
example to follow the same conditions as the given example.

## Failure cases

Raise ValueError for incompatible shapes, nonfinite inputs, repeated or
decreasing coordinates, or fewer than two points in a direction.
Test wrong outward signs and swapped axes with non-square domains.
Explain how the boundary perturbation exposes a function that always returns
zero: 

Student Explanation: A function that always returns zero will pass every test 
whose expected flux is zero. A boundary perturbation works as a loophole to this. 
If you multiply the edge of a boundary by any factor, then the boundary flux must 
also be raised by that number. 


## Verification plan and revision history

List separate unit, regression, invariant and independent-reference tests.
Record specification commit, later revisions and reasons in PROCESS_LOG.md.
Do not select thresholds after viewing candidate results.


(taken from ASSIGNMENT.MD)


1. Unit test:	
  Reject wrong shapes, nonfinite data and non-increasing coordinates;
  check the scalar return and outward signs.
3. Numerical regression:
  Run the existing Week 1.1 tests and compare the retained acceptance record;
  preserve the cavity archive hash.
5. Physical invariant: 
  A constant velocity field and the closed Re=100 cavity have zero net outward
  volume flux within 1e-12. Add 0.1 to u on the entire right boundary of the cavity:
  flux must increase by 0.1 on the unit square. This is a deliberately altered field,
  not a new CFD solution.
7. Baseline/reference:
  For u=x, v=y on the unit square, exact flux is 2. For u=x squared, v=0, it is 1.
Test rectangular and nonuniform coordinates against independently derived boundary integrals.







