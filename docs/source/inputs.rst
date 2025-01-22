Input Parameters
----------------

!-- write stdout to file
    :code:`in_name = "spn"`
    - character(40)
    - simulation name/title, for post-processing identification
    :code:`in_comment = ""`
    - character(80)   
    - why did I run this simulation?

    
!-- parallelization
    :code:`in_nomp = 1`  
    - integer 
    - number of openmp threads

    
!-- grid geometry and dimensions
    :code:`in_grd_igeom = 0`
    - integer
    - geometry: 1=sph, 2=cyl, 3=car, 11=1Dsph
    :code:`in_ndim(3) = 1, 1, 1`
    - integer
    - number of x-direction cells
    :code:`in_isvelocity = t`
    - logical 
    - switch underlying grid between spatial+static to velocity+expanding


!-- read input structure file instead of specifying the stucture with input parameters
    :code:`in_voidcorners = f`
    - logical 
    - zero mass in cells outside central sphere in domain
    :code:`in_noreadstruct = f`
    - logical 

    
!-- static grid size
    :code:`in_str_lx = 0d0`  
    - real*8 
    - spatial length of x-direction
    :code:`in_str_ly = 0d0`  
    - real*8 
    - spatial length of y-direction
    :code:`in_str_lz = 0d0`  
    - real*8 
    - spatial length of z-direction

    
!-- parameterized input structure
    :code:`in_str_velout = 0d0`
    - real*8 
    - velocity of outer bound, [cm/s] 
    :code:`in_str_totmass = 0d0`
    - real*8 
    - in [g]
    :code:`in_str_dentype = 'none'`
    - character(4) 
    - options: 
        - :code:`'unif'` for uniform density, 
        - :code:`'mass'` for equal mass accross cells

        
!-- time step
    :code:`in_tsp_tfirst = 0d0`  
    - real*8 
    - first point in time evolution [sec]
    :code:`in_tsp_tlast = 0d0`   
    - real*8 
    - last point in time evolution [sec]
    :code:`in_tsp_nt = 0`   
    - integer           
    - number of time steps
    :code:`in_tsp_itrestart = -1`  
    - integer        
    - restart time step number
    :code:`in_tsp_gridtype = 'line'`
    - character(4) 
    - timestep grid type
    - options: 
        - :code:`'line'` for linear
        - :code:`'expo'` for exponential
        - :code:`'read'` for read-in 
  

!-- group structure
    :code:`in_grp_ng = -1`
    - integer 
    - number of groups: 
    - :code:`0` read wl-grid from file
    :code:`in_grp_ngs = 1`
    - integer 
    - number of subgroups per opacity group
    :code:`in_grp_wlmin = 100d-8`
    - real*8 
    - lower wavelength boundary [cm]
    :code:`in_grp_wlmax = 32000d-8`
    - real*8 
    - upper wavelength boundary [cm]
    :code:`in_grp_wldex = 0`
    - integer 
    - selects group grid from formatted group grid file


!-- outbound flux group and direction bins
    :code:`in_flx_ndim = 0, 1, 1`
    - integer
    :code:`in_flx_wlmin = 1000d-8`
    - real*8
    - lower wavelength flux boundary [cm]
    :code:`in_flx_wlmax = 32000d-8`
    - real*8
    - upper wavelength flux boundary [cm]
    :code:`in_flx_noobservertime = f`
    - logical
    - record flux in escape time instead of observer time


!-- particles
    :code:`in_prt_nmax = 0`
    - integer 
    - length of particle array
    :code:`in_prt_n2max = -1`
    - integer 
    - 2^n length of particle array


!-- source
    :code:`in_src_ns = 0`
    - integer
    - number of source particles generated per time step (total over all ranks)
    :code:`in_src_n2s = -1`
    - integer
    - 2^n source particles generated per time step (total over all ranks)
    :code:`in_src_nsinit = 0`
    - integer
    - number of initial particles at in_tsp_tf irst
    :code:`in_src_n2sinit = -1`
    - integer
    - 2^n number of initial particles at in_tsp_tfirst
    :code:`in_novolsrc = f`
    - logical
    - switch to turn off any volume source (could be useful for debugs)
    :code:`in_srcepwr = 1d0`
    - real*8
    - source particle number-energy slope
    - :code:`1` is linear, equal number of packets per erg.

    
!-- analytic power-law source terms
    :code:`in_gas_srccoef = 0d0`
    - real*8 
    :code:`in_gas_srcrpwr = 0d0`
    - real*8 
    :code:`in_gas_srctpwr = 0d0`
    - real*8 
    :code:`in_gas_srctimepwr = 0d0`
    - real*8 
        

!-- external source structure
    :code:`in_srctype = 'none'`
    - character(4) 
    - external source structure type
    - options: :code:`'none'` | :code:`'heav'` | :code:`'strt'` | :code:`'manu'` | :code:`'surf'` 
    :code:`in_surfsrcloc = 'out'`
    - character(4) 
    - surface source location
    - options: :code:`'in'` | :code:`'out'` | :code:`'up'` | :code:`'down'` | :code:`'top'` | :code:`'botm'`
    :code:`in_surfsrcmu = 'isot'`
    - character(4)  
    - surface source direction distribution
    - options: :code:`'isot'` | :code:`'beam'`
    :code:`in_nheav = 0`
    - integer   
    - outer cell bound if heaviside (:code:`'heav'`) source
    :code:`in_theav = 0d0`
    - real*8  
    - duration of heaviside source
    :code:`in_srcmax = 0d0`
    - real*8   
    - peak source strength

    
!-- external power-law gamma-ray source
    :code:`in_sgamcoef = 0d0`
    - real*8  
    :code:`in_sgamrpwr = 0d0`
    - real*8   
    :code:`in_sgamtpwr = 0d0`
    - real*8   
    :code:`in_sgamtimepwr = 0d0`
    - real*8 
        


!-- transport
    :code:`in_trn_errorfatal = t`
    - logical 
    - stop on transport error, disable for production runs
    :code:`in_trn_tauddmc = 5d0`
    - real*8 
    - number of mean free paths per cell required for DDMC
    :code:`in_taulump = 10d0`
    - real*8 
    - number of of mean free paths needed to lump DDMC groups
    :code:`in_puretran = f`
    - logical 
    - use IMC only instead of IMC+DDMC hybrid
    :code:`in_trn_isimcanlog = f`
    - logical 
    - use analog IMC tally if true
    :code:`in_trn_isddmcanlog = t`
    - logical 
    - use analog DDMC tally if true
    :code:`in_trn_noamp = t`
    - logical 
    - disable amplification factor
    :code:`in_alpha = 1d0`
    - real*8 
    - time centering control parameter [0,1]
    :code:`in_trn_nolumpshortcut = f`
    - logical 
    - disable approximation for large emitlump that sampling outside the lump collapses to the single most likely groupc-- time dependence of :code:`in_trn_tauddmc` and :code:`in_taulump`
    :code:`in_trn_tauvtime = 'unif'`
    - character(4) 
    - options:
        - :code:`'unif'` for constant
        - :code:`'incr'` for limiting (s-curve) to more conservative constant
    :code:`in_ismodimc = t`
    - logical 
    - Gentile-Fleck factor switch


!-- gas grid parameters
    :code:`in_gas_gastempinit = 0d0`
    - real*8  
    - non-zero will not read temp from file. units: K
    :code:`in_gas_radtempinit = 0d0`
    - real*8  
    - initial radiation temperature.  Use :code:`grd_temp` by default

    
!-- analytic heat capacity terms
    :code:`in_gas_cvcoef = 1d7`
    - real*8  
    - power law heat capacity coefficient
    :code:`in_gas_cvtpwr = 0d0`
    - real*8  
    - power law heat capacity temperature exponent
    :code:`in_gas_cvrpwr = 1d0`
    - real*8  
    - power law heat capacity density exponent 
    

!-- debugging
    :code:`in_noeos = f`
    - logical 
    - do not use the EOS


!-- physical opacities
    :code:`in_opcapgam = .06d0`
    - real*8 
    - extinction coefficient for gamma radiation, [cm^2/g] 
    :code:`in_noplanckweighting = f`
    - logical
    - disable planck weighting of rosseland opacities within group
    :code:`in_opacmixrossel = 0d0`
    - real*8
    - mix rosseland with planck average
    - :code:`1` is pure rosseland

!-- physical opacities debuging
    :code:`in_nobbopac = f`
    - logical    
    - turn off bound-bound opacity
    :code:`in_nobfopac = f`
    - logical    
    - turn off bound-bound opacity
    :code:`in_noffopac = f`
    - logical    
    - turn off bound-bound opacity
    :code:`in_nothmson = f`
    - logical   
    - turn off thomson scattering

!-- fontes tabular opacity switch
    :code:`in_notbopac = f`
    - logical   
    - turn on tabular opacity
    :code:`in_notbbbopac = f`
    - logical   
    - turn off bound-bound opacity
    :code:`in_notbbfopac = f`
    - logical   
    - turn off bound-bound opacity
    :code:`in_notbffopac = f`
    - logical   
    - turn off bound-bound opacity
    :code:`in_notbthmson = f`
    - logical   
    - turn off thomson scattering


!-- analytic opacities
    :code:`in_opacanaltype = 'none'`
    - character(4)
    - group opacity structure type
    - options: :code:`'none'` | :code:`'grey'` | :code:`'mono'` | :code:`'pick'` | :code:`'line'` 

    
!-- picket fence specific group structure
    :code:`in_suol = 'tsta'`
    - character(4)
    - Su&Olson picket fence (pick) test cases
    - options: :code:`'tsta'` | :code:`'tstb'` | :code:`'tstc'`  
    :code:`in_suolpick1 = 1d0`
    - real*8 
    - probability of being at first picket, in [0,1]

    
!-- line specific group structure
    :code:`in_ldisp1 = 1d0`
    - real*8 
    - loosely speaking, the analytic odd group line strength
    :code:`in_ldisp2 = 1d0`
    - real*8 
    - loosely speaking, the analytic even group line strength

    
!-- scattering terms:
    :code:`in_gas_sigcoef = 0d0`
    - real*8 
    - power law absorption opacity coefficient
    :code:`in_gas_sigtpwr = 0d0`
    - real*8 
    - power law absorption opacity temperature exponent
    :code:`in_gas_sigrpwr = 0d0`
    - real*8 
    - power law absorption opacity density exponent

    
!-- absorption terms:
    :code:`in_gas_capcoef = 0d0`
    - real*8 
    - power law absorption opacity coefficient
    :code:`in_gas_captpwr = 0d0`
    - real*8 
    - power law absorption opacity temperature exponent
    :code:`in_gas_caprpwr = 0d0`
    - real*8 
    - power law absorption opacity density exponent

    
!-- output
    :code:`in_io_grabstdout = f`
    - logical
    - write stdout to file
    :code:`in_io_dogrdtally = f`
    - logical
    - write transport tallies per grid cell
    :code:`in_io_nogriddump = f`
    - logical
    - do not write grid cell variables
    :code:`in_io_nogridgroupdump = f`
    - logical
    - do not write group and cell-dependent variables
    :code:`in_io_opacdump = 'off'`
    - character(4)
    - write opacity data to file
    - options: :code:`'off'` | :code:`'one'` | :code:`'each'` | :code:`'all'`
    :code:`in_io_pdensdump = 'off'`
    - character(4)
    - write partial densities to file
    - options: :code:`'off'` | :code:`'one'` | :code:`'each'`
