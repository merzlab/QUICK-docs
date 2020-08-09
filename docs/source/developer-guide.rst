Developer Guide
===============

QUICK API
---------

Starting from version 20.06, QUICK build system compiles the source code and creates static or 
shared object libraries. Such libraries are then linked to the main QUICK program. If the user
does not specify a prefix during the configure script run, libraries will be located inside 
*$QUICK_HOME/lib/$buildtype* where *$QUICK_HOME* is the QUICK home directory and *$buildtype*
could be *serial*, *mpi* or *cuda*. Required .mod or header files can be found inside *$QUICK_HOME/include/$buildtype*.
If the user specifies a prefix, above material are also located inside the installation directory.

It is possible to link QUICK libraries into external programs and obtain HF/DFT energies, gradients
and point charge gradients through the Fortran 90 QUICK API. Perhaps the best way to explain the usage of API
is using an example. 

Let us consider a simple system containing a water molecule surrounded by 3 point charges. We now create the
following fortran module (test_module.f90) and store atomic coordinates and charges for 5 snapshots. Furthermore, we implement
several subroutines to load test data and print data retrieved from QUICK. 

::

	! Test module for QUICK library API
	module test_module

	  implicit none
	  private

	  ! Interfaces for cuda/serial version
	  public :: loadTestData, printQuickOutput

	! Interfaces for MPI version 
	#ifdef MPI
	  public :: initializeMPI, printQuickMPIOutput
	#endif

	  

	  ! Store the atomic coordinates of of water molecule for 5 snapshots. Store the same 
	  ! information for external point charges. Note that we follow x, y, z, charge format
	  ! for the latter.
	  double precision, dimension(1:45) :: all_coords
	  double precision, dimension(1:60) :: all_extchg

	  data all_coords &
	  /-0.778803, 0.000000, 1.132683, &
	   -0.666682, 0.764099, 1.706291, &
	   -0.666682,-0.764099, 1.706290, &
	   -0.678803, 0.000008, 1.232683, &
	   -0.724864, 0.755998, 1.606291, &
	   -0.724862,-0.756005, 1.606290, &
	   -0.714430, 0.000003, 1.267497, &
	   -0.687724, 0.761169, 1.624424, &
	   -0.687723,-0.761172, 1.624427, &
	   -0.771504, 0.000000, 1.167497, &
	   -0.669068, 0.763767, 1.697008, &
	   -0.669068,-0.763767, 1.697008, &
	   -0.771372, 0.000000, 1.162784, &
	   -0.668845, 0.767538, 1.698983, &
	   -0.668845,-0.767538, 1.698982/

	  data all_extchg &
	  /1.6492, 0.0000,-2.3560, -0.8340, &
	   0.5448, 0.0000,-3.8000,  0.4170, &
	   0.5448, 0.0000,-0.9121,  0.4170, &
	   1.6492, 0.0000,-2.3560, -0.8360, &
	   0.5448, 0.0000,-3.8000,  0.4160, &
	   0.5448, 0.0000,-0.9121,  0.4160, &
	   1.6492, 0.0000,-2.3560, -0.8380, &
	   0.5448, 0.0000,-3.8000,  0.4150, &
	   0.5448, 0.0000,-0.9121,  0.4150, &
	   1.6492, 0.0000,-2.3560, -0.8400, &
	   0.5448, 0.0000,-3.8000,  0.4140, &
	   0.5448, 0.0000,-0.9121,  0.4140, &
	   1.6492, 0.0000,-2.3560, -0.8420, &
	   0.5448, 0.0000,-3.8000,  0.4130, &
	   0.5448, 0.0000,-0.9121,  0.4130/

	  interface loadTestData
	    module procedure load_test_data
	  end interface loadTestData

	  interface printQuickOutput
	    module procedure print_quick_output
	  end interface printQuickOutput

	#ifdef MPI
	  interface initializeMPI
	    module procedure mpi_initialize
	  end interface initializeMPI

	  interface printQuickMPIOutput
	    module procedure print_quick_mpi_output
	  end interface printQuickMPIOutput
	#endif

	contains

	  subroutine load_test_data(frame, natoms, nxt_charges, coord, xc_coord)

	    implicit none

	    integer, intent(in)             :: frame, natoms, nxt_charges
	    double precision, intent(inout) :: coord(3, natoms)
	    double precision, intent(out)   :: xc_coord(4, nxt_charges)
	    integer :: i, j, k

	    k=natoms*3*(frame-1) + 1
	    do i=1,natoms
	      do j=1,3
	        coord(j,i) = all_coords(k)
	        k=k+1
	      enddo
	    enddo

	    if(nxt_charges>0) then
	      k=nxt_charges*4*(frame-1) + 1
	      do i=1,nxt_charges
	        do j=1,4
	          xc_coord(j,i) = all_extchg(k)
	          k=k+1
	        enddo
	      enddo
	    endif

	  end subroutine load_test_data

	  subroutine print_quick_output(natoms, nxt_charges, atomic_numbers, totEne, gradients, ptchg_grad)

	    implicit none

	    integer, intent(in)          :: natoms, nxt_charges
	    integer, intent(in)          :: atomic_numbers(natoms)
	    double precision, intent(in) :: totEne
	    double precision, intent(in) :: gradients(3,natoms)
	    double precision, intent(in) :: ptchg_grad(3,nxt_charges)
	    integer :: i, j

	    ! print energy  
	    write(*,*) ""
	    write(*,*) "*** TESTING QUICK API ***"
	    write(*,*) ""
	    write(*,*) "PRINTING ENERGY"
	    write(*,*) "---------------"
	    write(*,*) ""
	    write(*, '(A14, 3x, F14.10, 1x, A4)') "TOTAL ENERGY =",totEne,"A.U."

	    ! print gradients
	    write(*,*) ""
	    write(*,*) "PRINTING GRADIENTS"
	    write(*,*) "------------------"
	    write(*,*) ""
	    write(*, '(A14, 3x, A6, 10x, A6, 10x, A6)') "ATOMIC NUMBER","GRAD-X","GRAD-Y","GRAD-Z"

	    do i=1,natoms
	      write(*,'(6x, I5, 2x, F14.10, 2x, F14.10, 2x, F14.10)') atomic_numbers(i), gradients(1,i), gradients(2,i), gradients(3,i)
	    enddo

	    ! print point charge gradients
	    if(nxt_charges>0) then
	      write(*,*) ""
	      write(*,*) "PRINTING POINT CHARGE GRADIENTS"
	      write(*,*) "-------------------------------"
	      write(*,*) ""
	      write(*, '(A14, 3x, A6, 10x, A6, 10x, A6)') "CHARGE NUMBER","GRAD-X","GRAD-Y","GRAD-Z"

	      do i=1,nxt_charges
	        write(*,'(6x, I5, 2x, F14.10, 2x, F14.10, 2x, F14.10)') i, ptchg_grad(1,i), ptchg_grad(2,i), ptchg_grad(3,i)
	      enddo
	    endif

	    write(*,*) ""

	  end subroutine print_quick_output


	#ifdef MPI
	  ! initialize mpi library and save mpirank and mpisize
	  subroutine mpi_initialize(mpisize, mpirank, master, mpierror)

	    implicit none

	    integer, intent(inout) :: mpisize, mpirank, mpierror
	    logical, intent(inout) :: master

	    include 'mpif.h'

	    call MPI_INIT(mpierror)
	    call MPI_COMM_RANK(MPI_COMM_WORLD,mpirank,mpierror)
	    call MPI_COMM_SIZE(MPI_COMM_WORLD,mpisize,mpierror)
	    call MPI_BARRIER(MPI_COMM_WORLD,mpierror)

	    if(mpirank .eq. 0) then
	      master = .true.
	    else
	      master = .false.
	    endif

	  end subroutine mpi_initialize

	  ! prints mpi output sequentially
	  subroutine print_quick_mpi_output(natoms, nxt_charges, atomic_numbers, totEne, gradients, ptchg_grad, mpirank)

	    implicit none

	    integer, intent(in)          :: natoms, nxt_charges, mpirank
	    integer, intent(in)          :: atomic_numbers(natoms)
	    double precision, intent(in) :: totEne
	    double precision, intent(in) :: gradients(3,natoms)
	    double precision, intent(in) :: ptchg_grad(3,nxt_charges)

	    write(*,*) ""
	    write(*,'(A11, 1X, I3, 1x, A3)') "--- MPIRANK", mpirank, "---"
	    write(*,*) ""

	    call printQuickOutput(natoms, nxt_charges, atomic_numbers, totEne, gradients, ptchg_grad)

	  end subroutine print_quick_mpi_output

	#endif

	end module test_module

Next, we implement the following example program (example.f90) that would use above module and call QUICK through the API.

::

	! Example program for demonstrating QUICK API usage
	  program example_program

	    ! Use subroutines from test_module
	    use test_module, only : loadTestData, printQuickOutput

	    ! Use subroutines from QUICK API
	    use quick_api_module, only : setQuickJob, getQuickEnergy, getQuickEnergyGradients, deleteQuickJob 

	#ifdef MPI
	    ! Use MPI specific subroutines 
	    use test_module, only : initializeMPI, printQuickMPIOutput
	    use quick_api_module, only : setQuickMPI
	#endif
	
	    implicit none
	
	#ifdef MPI
	    include 'mpif.h'
	#endif
	
	    ! i, j are some integers useful for loops, frames is the number of
	    ! test snapshots
	    integer :: i, j, frames
	   
	    ! Number of atoms, number of atom types, number of external point charges
	    integer :: natoms, nxt_charges

	    ! Atom type ids, atomic numbers, atomic coordinates, point charges and
	    ! coordinates
	    integer, allocatable, dimension(:)            :: atomic_numbers 
	    double precision, allocatable, dimension(:,:) :: coord          
	    double precision, allocatable, dimension(:,:) :: xc_coord       
	
	    ! Name of the quick output file
	    character(len=80) :: fname
	
	    ! QUICK job card (a string of keywords).
	    character(len=200) :: keywd
	
	    ! Total QM energy, gradients and point charge gradients
	    double precision :: totEne
	    double precision, allocatable, dimension(:,:) :: gradients         
	    double precision, allocatable, dimension(:,:) :: ptchgGrad      
	
	#ifdef MPI
	    ! MPI specific variables  
	    integer :: mpierror = 0
	    integer :: mpirank  = 0
	    integer :: mpisize  = 1
	    logical :: master   = .true.
	#endif
	
	
	#ifdef MPI
	    ! Initialize MPI library and get mpirank, mpisize
	    call initializeMPI(mpisize, mpirank, master, mpierror)
	
	    ! Setup QUICK MPI, called only once
	    call setQuickMPI(mpirank,mpisize)
	#endif
	
	    ! Set molecule size. Recall that we consider a water molecule surounded by 3 point
	    ! charges. 
	    natoms      = 3
	    nxt_charges = 3    
	
	    ! We also consider 5 snapshots of this test system.
	    frames = 5
	
	    ! Allocate memory for input and output arrays. Recall that in xc_coord array,
	    ! the first 3 columns are the xyz coordinates of the point charges. The
	    ! fourth column is the charge. 
	    if ( .not. allocated(atomic_numbers)) allocate(atomic_numbers(natoms)) 
	    if ( .not. allocated(coord))          allocate(coord(3,natoms))
	    if ( .not. allocated(xc_coord))       allocate(xc_coord(4,nxt_charges))
	    if ( .not. allocated(gradients))         allocate(gradients(3,natoms))
	    if ( .not. allocated(ptchgGrad))      allocate(ptchgGrad(3,nxt_charges))
	
	    fname           = 'water'
	    keywd           = 'HF BASIS=6-31G CUTOFF=1.0D-10 DENSERMS=1.0D-6 GRADIENT EXTCHARGES'
	
	    atomic_numbers(1)  = 8
	    atomic_numbers(2)  = 1
	    atomic_numbers(3)  = 1
	
	    ! Set result vectors and matrices to zero.
	    gradients = 0.0d0
	    ptchgGrad = 0.0d0
	
	    ! Initialize QUICK, required only once.
	    call setQuickJob(fname, keywd, natoms, atomic_numbers, nxt_charges)
	
	    do i=1, frames
	
	      ! Load coordinates and external point charges for ith snapshot
	      call loadTestData(i, natoms, nxt_charges, coord, xc_coord)
	
	      ! Compute required quantities, call only a or b. 
	      ! a. Compute energy.
	      ! call getQuickEnergy(coord, xc_coord, totEne)
	
	      ! b. Compute energies, gradients and point charge gradients.
	      call getQuickEnergyGradients(coord, xc_coord, totEne, gradients, ptchgGrad)    
	
	      ! print values obtained from quick library
	#ifdef MPI
	      ! A naive trick to print output from each core sequentially.
	      call MPI_BARRIER(MPI_COMM_WORLD,mpierror)
	
	      do j=0, mpisize-1
	        if(j .eq. mpirank) then
	          call printQuickMPIOutput(natoms, nxt_charges, atomic_numbers, totEne, gradients, ptchgGrad, mpirank)
	        endif
	        call MPI_BARRIER(MPI_COMM_WORLD,mpierror)
	      enddo 
	#else
	      call printQuickOutput(natoms, nxt_charges, atomic_numbers, totEne, gradients, ptchgGrad)
	#endif
	
	    enddo
	
	    ! Finalize QUICK, required only once
	    call deleteQuickJob()
	
	    ! Deallocate memory
	    if ( allocated(atomic_numbers)) deallocate(atomic_numbers)
	    if ( allocated(coord))          deallocate(coord)
	    if ( allocated(xc_coord))       deallocate(xc_coord)
	    if ( allocated(gradients))      deallocate(gradients)
	    if ( allocated(ptchgGrad))      deallocate(ptchgGrad)
	
	  end program example_program

Assuming we configured QUICK serial version without a prefix and compiled using intel compiler toolchain, 
we can compile above source files and link QUICK libraries as follows.

::

	ifort -cpp test_module.f90 example_program.f90 -o example_program -I$QUICK_HOME/build/include/serial/ 
	-L$QUICK_HOME/build/lib/serial/ -lquick -lblas -lxc -lstdc++	

MPI version of the libraries can be linked as follows.

::

	mpiifort -cpp -DMPI test_module.f90 example_program.f90 -o example_program -I$QUICK_HOME/build/include/mpi/ 
	-L$QUICK_HOME/build/lib/mpi/ -lquick -lblas -lxc -lstdc++

CUDA version of the libraries can be linked as follows.

::

	ifort -cpp test_module.f90 example_program.f90 -o example_program -I$PWD/build/include/cuda/ 
	-L$PWD/build/lib/cuda/ -L$CUDA_HOME/lib64 -lcuda -lm -lcudart -lcublas -lcusolver -lquick -lxc -lstdc++

Running serial or CUDA executable should produce `this output <https://raw.githubusercontent.com/merzlab/QUICK-docs/master/resources/api-serial.txt>`_. 
A `similar output <https://raw.githubusercontent.com/merzlab/QUICK-docs/master/resources/api-mpi.txt>`_ may be obtained by running MPI version with 2 cores. 

*Last updated by Madu Manathunga on 08/08/2020.*

