#::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
#                                                                       :::
#                          LTRANS Makefile                              :::
#                                                                       :::
#::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::


#==========================================================================
#                         USER-DEFINED OPTIONS                            =
#==========================================================================




#------------------------------------------------
#    Set compiler and flags
#------------------------------------------------
#
#    Turn one of the following on:
IFORT_UMCES := on
GFORTRAN_UMCES :=
GFORTRAN_USGS :=
PGI_USGS   :=

ifdef IFORT_UMCES
  FC = ifort
  NETCDF_INCDIR = /opt/hpc/intel12/include
  NETCDF_LIBDIR = /opt/hpc/intel12/lib
  FFLAGS = -vec-report0 -fp-model precise -mcmodel=medium -I$(NETCDF_INCDIR) -openmp
endif

ifdef GFORTRAN_UMCES
  FC = gfortran
  NETCDF_INCDIR = /data/users/imitchell/local/include
  NETCDF_LIBDIR = /data/users/imitchell/local/lib
  FFLAGS = -march=k8 -ffast-math -fno-cx-limited-range -O3 -funroll-loops --param max-unroll-times=4 -ffree-line-length-none -I$(NETCDF_INCDIR)
endif

ifdef GFORTRAN_USGS
  FC = gfortran
  NETCDF_INCDIR = /usr/local/include
  NETCDF_LIBDIR = /usr/local/lib
  FFLAGS = -Wall  -Warray-bounds -fbacktrace -ffree-line-length-none  -I$(NETCDF_INCDIR)
endif

ifdef PGI_USGS
  FC = pgf90
  NETCDF_INCDIR = /share/apps/netcdf/include
  NETCDF_LIBDIR = /share/apps/netcdf/lib
  FFLAGS := -g -I$(NETCDF_INCDIR)
endif

#------------------------------------------------
#    Set NetCDF Library Locations.
#    If NetCDF was compiled with HDF5, set:
#        HDF5 := on
#    Otherwise, leave blank:
#        HDF5 :=
#------------------------------------------------

HDF5 := on

#==========================================================================
# End of user-defined options. Nothing should be changed below this point =
#==========================================================================

OBJS          = ./src/gridcell_module.o ./src/interpolation_module.o ./src/parameter_module.o \
				./src/point_in_polygon_module.o ./src/random_module.o ./src/tension_module.o  \
				./src/conversion_module.o ./src/hydrodynamic_module.o ./src/norm_module.o     \
				./src/boundary_module.o ./src/hor_turb_module.o ./src/settlement_module.o     \
				./src/ver_turb_module.o ./src/behavior_module.o

ifdef HDF5
   ifdef PGI_USGS
        LIBS      = -L$(NETCDF_LIBDIR) -lnetcdf -lnetcdff -L/share/apps/hdf5/lib -lhdf5_hl -lhdf5 -lz -lm -L/share/apps/szip/lib -lsz -lcurl
   else
	LIBS      = -L$(NETCDF_LIBDIR) -lnetcdff -lnetcdf -lhdf5_hl -lhdf5 -lcurl -L$(LD_LIBRARY_PATH)
   endif
else
	LIBS      = -L$(NETCDF_LIBDIR) -lnetcdff -lnetcdf -L$(LD_LIBRARY_PATH)
endif


LTRANS : $(OBJS)
	@echo "  Compiling LTRANS.f90"
	@$(FC) $(FFLAGS) -o LTRANS.exe ./src/LTRANS.f90 $(OBJS) $(LIBS)
	@\rm *.o *.mod
	@echo "  "
	@echo "  Compilation Successfully Completed"
	@echo "  "

%.o: %.f90
	@echo "  Compiling $<"
	@$(FC) $(FFLAGS) -c $< -o $<

clean:
	\rm *.o *.mod LTRANS.exe
