README for interactive vdw-df
DC 03/04/2020 WAKE FOREST UNIVERSITY, WINSTON SALEM, NC, USA.


We want to have qe6.5 program that can have variables for vdW-DF calculations interactively changable
from the input files. 

To do so, I have defined following parameters for the switching function:
a) vdw_h_switch : describes the switching functions, currently have three options, default is 'DION'
b) gamma_hfunc : determines the gamma value within the switching function. 
c) beta_hfunc : determines the beta value within the switching function. 
d) alpha_hfunc : determines the alpha value within the switching function. 

I have defined following exchange parameters as well:
A. Modifying optB88
a) mu_b88 : changes mu 
b) kappa_b88 : changes kappa
c) c_b88 : changes c value 
B. Modifying B86R
a) mu_b86r : changes mu 
b) beta_b86r : changes beta
c) power_b86r : changes the power of the denominator 

Following are the steps I have taken to implement them

I.   Put all of them as system namelist parameter in 'input_parameters.f90' and assigning an initial value.
II.  Put all of these parameters in 'read_namelists.f90' and broadcasted for all the codes within the QE package.
III. The vdw_df_switch is called in 'funct.f90' and a 'get_ihfun()' function is created.  
IV.  The options for two new functionals (OB8N, igcx=43 ; B86N, igcx=44 ) has been created in 'func.f90'.
V.   In 'xc_gga_drivers.f90' those two new functionals are assigned to proper subroutine (OB8N to 'pbex' and B86N to 'b86b').
VI.  In 'exchange_gga.f90' the parameters respective to OB8N and B86N are called and used to calculate the functional forms.
VII. In 'xc_vdw_df.f90' within 'phi_value' subroutine the get_ihfun and all the parameters for h-functions have been called to calculate.

PROBLEM: Although everything is compiling, but the 'read_namelists.f90' shows following error:

read_namelists.f90(943): error #6285: There is no matching specific subroutine for this generic subroutine call.   [MP_BCAST]
       CALL mp_bcast( gamma_hfunc,        ionode_id, intra_image_comm )
------------^

