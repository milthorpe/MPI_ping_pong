CUCOMP  = nvcc
CUFLAGS = -arch=sm_70

INCLUDES  = -I$(OMPI_DIR)/include
LIBRARIES = -L$(OMPI_DIR)/lib -lmpi_ibm

pp_cuda_aware: ping_pong_cuda_aware.o
	$(CUCOMP) $(CUFLAGS) $(LIBRARIES) ping_pong_cuda_aware.o -o pp_cuda_aware

ping_pong_cuda_aware.o: ping_pong_cuda_aware.cu
	$(CUCOMP) $(CUFLAGS) $(INCLUDES) -c ping_pong_cuda_aware.cu

HIPCC = hipcc
HIP_CFLAGS = -O3 
HIP_INCLUDES = -I${MPICH_DIR}/include
HIP_LDFLAGS = -L${MPICH_DIR}/lib -lmpi ${CRAY_XPMEM_POST_LINK_OPTS} -lxpmem ${PE_MPICH_GTL_DIR_amd_gfx90a} ${PE_MPICH_GTL_LIBS_amd_gfx90a}

pp_hip_aware: ping_pong_hip_aware.o
	$(HIPCC) $(HIP_CFLAGS) $(HIP_LDFLAGS) ping_pong_hip_aware.o -o $@

ping_pong_hip_aware.o: ping_pong_hip_aware.hip
	$(HIPCC) $(HIP_CFLAGS) $(HIP_INCLUDES) -c ping_pong_hip_aware.hip

ping_pong_hip_aware.hip: ping_pong_cuda_aware.cu
    ifeq (, $(shell which hipify-perl))
	    $(error hipify-perl is required, consider doing module load PrgEnv-amd)
    endif
	hipify-perl ping_pong_cuda_aware.cu > $@

.PHONY: clean

clean:
	rm -f pp_cuda_aware pp_hip_aware ping_pong_cuda_aware.hip *.o
