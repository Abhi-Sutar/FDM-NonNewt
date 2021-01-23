# Finite Difference Method Non-newtonian Truncated Power Law fluid
This Notebook solves the momentum equations for a variable viscosity fluid in two dimensions in a pressure driven channel flow or poiseuille flow. The solver is an explicit one. In this case, the viscosity is computed for a truncated power law fluid. The Pressure Poisson equation has been accelerated by leveraging the GPU using the numba library  

The equations and their discretized form are given in the PDF file in the repository. We have used the basic idea of 2D FDM solvers from the [12 steps to Navier-Stokes](https://github.com/barbagroup/CFDPython) by Prof. Lorena Barba.
## Usage
The Main function is  

~~~python
    chn_sim_run(n, nx, udiff_target)
~~~  

Which takes 'n' as the power law index, 'nx' as the number of grid points along each axis, and 'udiff_target' which is the convergence target, which is mostly set to 1e-6.
This returns u, v, p, vel_anal, nu1, gamma_dot, S11, S12, S22 and plotpdiff. Where u and v ae x and y components of viscosity. p is the pressure field,
and nu1 is the final array of kinematic viscosity at each grid point. gamma_dot is the strain rate, and S11, S22, S12 are the elements of the strain rate tensor.
plotpdiff is the array of convergence calculation outputs for the pressure field calculations and is only there for testing purposes.

### Changing grid size
Changing grid size is quite simple as you directly specify it in the function call. It may however require you to change the blockdim and griddim to improve occupancy or to have the block size and grid(the GPU grid **not** FDM) size as divisor of the FDM gridsize. Use **nx -2** as the Boundary conditions are computed separately.

## Details
The grid is 2 by 2 units. The Reynold's number is kept at Re = 40 and the pressure gradient required is computed by the function 
~~~python
nonNewtanalytical(n, H,ny)
~~~
H is two here and n is the power law index, ny is the number of grid points along the y axis.
YOu can change the Re as per your requirement in this function
## Issues
The biggest issue with this code is the we can only run shear thinning fluid, i.e., those with power law index(n) less than 1 for a rather coarse 34 by 34 grid and only for n more than 0.7 or thereabouts. There is still some instability in the solver and the solution blows up for finer grids. 
The secondis notso much an issue as just a heads up, the notebook was written in Google's Colaboratory. The tab indentation as half of what the standard jupyter notebook editor uses so you may have to adjust the indentation if you execute the notebook in jupyter notebook. If you want to save yourself the hassle, you might as well run it in Colaboratory.

## Contributing
Pull requests are welcome. It might be a good idea to give some explanation as to what the changes proposed are exactly doing.

## License

A slightly modified version of the [MIT](https://choosealicense.com/licenses/mit/) License that you will find here.
Copyright &copy; [2020] [Abhijeet Sutar and Yash Kala]
Our emails:
* Abhijeet : <sutar.2@iitj.ac.in>
* Yash : <kala.1@iitj.ac.in>  

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

* The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
* Wherever you use this code or some derivative of it, kindly attribute the Authors mentioned in the copyright.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
 
