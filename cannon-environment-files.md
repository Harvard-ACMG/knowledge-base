# Loading software modules on Cannon

The GEOS-Chem Support Team has created several **environment files** that you can use to load the various required software packages (such as compilers, netCDF library, etc.) into your Cannon login environment.

We provide links to sample environment files below.  These are contained in our [cannon-env](https://github.com/Harvard-ACMG/cannon-env) repository,

To load software modules for GEOS-Chem Classic using the GNU Compiler Collection 10.2.0, type:
```
source gcc_cmake.gfortran102_cannon.env
```
etc. for other environment files.

## Environment files for GEOS-Chem Classic

### With the GNU compilers

  - GNU 10.2.0: [`gcc_cmake.gfortran102_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/blob/main/envs/gcc_cmake.gfortran102_cannon.env)

  - GNU 9.3.0: [`gcc_cmake.gfortran93_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/blob/main/envs/gcc_cmake.gfortran93_cannon.env)

  - GNU 8.2.0: [`gcc_cmake.gfortran83_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/commit/0ec06a48ce658468b10b6d12879b9da01199ed4c)

### With the Intel compilers

  - Intel 19.0.4: [`gcc_cmake.ifort19_openmpi_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/blob/main/envs/gcc_cmake.ifort19_openmpi_cannon.env)

  - Intel 18.0.5: [`gcc_cmake.ifort19_openmpi_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/commit/0ec06a48ce658468b10b6d12879b9da01199ed4c)

  - Intel 17.0.4 [`gcc_cmake.ifort17_openmpi_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/blob/main/envs/gcc_cmake.ifort17_openmpi_cannon.env)

## Environment files for GCHP

### With the GNU compilers

  - GNU 9.3.0: [`gchpctm.gfortran93_openmpi4_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/blob/main/envs/gchpctm.gfortran93_openmpi4_cannon.env)

  - GNU 8.3.0: [`gchpctm.gfortran83_openmpi4_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/blob/main/envs/gchpctm.gfortran83_openmpi4_cannon.env)

### With the Intel compilers

  - Intel 19.0.5: [`gchpctm.ifort19_openmpi4_cannon.env`](https://github.com/Harvard-ACMG/cannon-env/blob/main/envs/gchpctm.ifort19_openmpi4_cannon.env)
