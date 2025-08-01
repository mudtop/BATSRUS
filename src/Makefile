#  Copyright (C) 2002 Regents of the University of Michigan, 
#  portions used with permission 
#  For more information, see http://csem.engin.umich.edu/tools/swmf

SHELL =/bin/sh

include ../Makefile.def

SEARCHDIR = -I${SHAREDIR} \
	-I../srcBATL -I${MAGNETOGRAMDIR} -I${DEMTDIR} -I${EMPIRICALEEDIR} \
	-I${EMPIRICALSCDIR} -I${CRASHDIR}

include ../Makefile.conf
-include Makefile.DEPEND
-include Makefile.RULES

# OBJECT FILES FOR LIBRARY / EXECUTABLE

OBJECTS = \
	ModAdvance.o \
	ModAdvanceExplicit.o \
	ModAdvectPoints.o \
	ModAMR.o \
	ModB0.o \
	ModBatsrusMethods.o \
	ModBatsrusUtility.o \
	ModBatlInterface.o \
	ModBlockData.o \
	ModBorisCorrection.o \
	ModBoundaryGeometry.o \
	ModBuffer.o\
	ModCalcSource.o \
	ModCellBoundary.o \
	ModCellGradient.o \
	ModCharacteristic.o \
	ModChromosphere.o \
	ModCleanDivB.o \
	ModCoarseAxis.o \
	ModConservative.o \
	ModConserveFlux.o \
	ModConstrainDivB.o \
	ModCoronalHeating.o \
	ModCurrent.o \
	ModEnergy.o \
	ModElectricField.o \
	ModEquation.o \
	ModExtraVariables.o \
	ModFaceBoundary.o \
	ModFaceFlux.o \
	ModFaceGradient.o \
	ModFaceValue.o \
	ModFieldLineThread.o \
	ModFieldTrace.o \
	ModFieldTraceFast.o \
	ModFixAxisCells.o \
	ModGeometry.o \
	ModGroundMagPerturb.o \
	ModHallResist.o \
	ModHdf5.o \
	ModHeatConduction.o \
	ModHeatFluxCollisionless.o \
	ModIeCoupling.o \
	ModImCoupling.o \
	ModImplicit.o \
	ModImplHypre.o \
	ModIO.o \
	ModIonElectron.o \
	ModLaserHeating.o \
	ModLoadBalance.o \
	ModLocalTimeStep.o \
	ModMain.o \
	ModMessagePass.o \
	ModMultiFluid.o \
	ModMultiIon.o \
	ModNodes.o \
	ModOptimizeParam.o \
	ModParallel.o \
	ModParticleFieldLine.o \
	ModParticleMover.o \
	ModParticles.o \
	ModPartImplicit.o \
	ModPartSteady.o \
	ModPhysicalFlux.o \
	ModPhysics.o \
	ModPIC.o \
	ModPlotBox.o \
	ModPlotScalars.o \
	ModPlotShell.o \
	ModPlotShock.o \
	ModPointImplicit.o \
	ModProjectDivB.o \
	ModPUI.o \
	ModRadDiffusion.o \
	ModRadiativeCooling.o \
	ModRadioWaveImage.o \
	ModResistivity.o \
	ModRestartFile.o \
	ModReverseField.o \
	ModSatelliteFile.o \
	ModSemiImplicit.o \
	ModSetInitialCondition.o \
	ModSetParameters.o \
	ModSingleFluid.o \
	ModSize.o \
	ModSolarwind.o \
	ModSpectrum.o \
	ModStreamAligned.o \
	ModThreadedLC.o \
	ModTimeStepControl.o \
	ModTimewarp.o \
	ModTurbulence.o \
	ModUaCoupling.o \
	ModUserEmpty.o \
	ModUserInterface.o \
	ModUser.o \
	ModViscosity.o \
	ModWaves.o \
	ModWriteTecplot.o \
	ModUpdateStateFast.o \
	ModUpdateState.o \
	ModWriteLogSatFile.o \
	ModWritePlot.o \
	ModWritePlotIdl.o \
	ModWritePlotLos.o \
	ModWritePlotRadioWave.o \
	ModWriteProgress.o \
	user_interface.o

# Default options
INSTALLFILES = ModEquation.f90 ModUser.f90 show_git_info.h \
	ModOptimizeParam.f90 ModHdf5.f90 ModImplHypre.f90 

install: ${INSTALLFILES}

show_git_info.h:
	cp show_git_info.h.orig show_git_info.h

ModEquation.f90:
	cp ../srcEquation/ModEquationMhd.f90 ModEquation.f90

ModUser.f90:
	cp ModUserDefault.f90 ModUser.f90

ModOptimizeParam.f90: ModOptimizeParam_orig.f90
	cp ModOptimizeParam_orig.f90 ModOptimizeParam.f90

ModHdf5.f90: ModHdf5_empty.f90
	cp -f ModHdf5_empty.f90 ModHdf5.f90

ModImplHypre.f90: ModImplHypre_empty.f90
	 cp -f ModImplHypre_empty.f90 ModImplHypre.f90

############################################################################

DEPEND: ${INSTALLFILES}
	perl ${SCRIPTDIR}/depend.pl ${SEARCHDIR} ${OBJECTS}

#
#	Making executables and libraries
#

#
# Library for framework and also for stand alone code
#

MY_LIB  = libBATSRUS.a
LIBBATL = ../srcBATL/libBATL.a

LIB:	DEPEND
	$(MAKE) ${MY_LIB}
	@echo
	@echo ${MY_LIB} has been brought up to date.
	@echo

${MY_LIB}: ${LIBBATL} ${OBJECTS}
	cp -f ${LIBBATL} ${MY_LIB}
	${AR} ${MY_LIB} ${OBJECTS}

# Object files not included into the library
OBJECTS_EXE = main.o

# Other requireed libraries
LIBSHARE  = ${LIBDIR}/libSHARE.a
LIBTIMING = ${LIBDIR}/libTIMING.a

# Libraries should be compiled first, because modules are used in main.
${OBJECTS_EXE}: ${LIBSHARE} ${LIBTIMING} ${MY_LIB}

EXE = ${BINDIR}/BATSRUS.exe

BATSRUS:
	$(MAKE) ${EXE}
	@echo ' '
	@echo Program BATSRUS has been brought up to date.
	@echo ' '	

${EXE}: ${OBJECTS_EXE}
	rm -rf Tmp_; mkdir Tmp_
	cd Tmp_; \
		ar -x ../${MY_LIB}; \
		ar -x ${LIBDIR}/libMAGNETOGRAM.a; \
		ar -x ${LIBDIR}/libDEMT.a; \
		ar -x ${LIBDIR}/libEMPIRICALEE.a; \
		ar -x ${LIBDIR}/libEMPIRICALSC.a; \
		ar -x ${LIBTIMING}; \
		ar -x ${LIBSHARE}
	${LINK.f90} -o ${EXE} ${OBJECTS_EXE} Tmp_/*.o \
		${LBLAS} ${HDFLIB} ${LflagMpi} ${HYPRELIB} ${SPICELIB}

# Compilation of CRASH ############

# Other required libraries
LIBCRASH  = ${LIBDIR}/libCRASH.a

# Libraries should be compiled first, because modules are used in main.
${CRASH_OBJECT_EXE}: ${LIBSHARE} ${LIBTIMING} ${LIBCRASH} ${MY_LIB}

CRASHEXE = ${BINDIR}/CRASH.exe

CRASH:
	$(MAKE) ${CRASHEXE}
	@echo ' '
	@echo Program CRASH has been brought up to date.
	@echo ' '	

${CRASHEXE}: ${OBJECTS_EXE} ${LIBCRASH}
	rm -rf Tmp_; mkdir Tmp_
	cd Tmp_; \
		ar -x ../${MY_LIB}; \
		ar -x ${LIBDIR}/libMAGNETOGRAM.a; \
		ar -x ${LIBDIR}/libDEMT.a; \
		ar -x ${LIBDIR}/libEMPIRICALEE.a; \
		ar -x ${LIBDIR}/libEMPIRICALSC.a; \
		ar -x ${LIBTIMING}; \
		ar -x ${LIBCRASH}; \
		ar -x ${LIBSHARE}
	${LINK.f90} -o ${CRASHEXE} ${OBJECTS_EXE} Tmp_/*.o \
		${LBLAS} ${HDFLIB} ${LflagMpi} ${HYPRELIB}

#########################################################################

#
#	cleaning
#

clean: cleanfiles
	rm -rf Tmp_

distclean: clean
	rm -f ModUser.f90.safe
	-mv ModUser.f90 ModUser.f90.safe
	rm -f ${INSTALLFILES}
