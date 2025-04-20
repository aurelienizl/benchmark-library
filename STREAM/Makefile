# Makefile for STREAM MPI benchmark with configurable parameters

# Compiler and flags
CC       = mpicc
CFLAGS   = -O2 
LDFLAGS  =

# Enable OpenMP support (set OPENMP=1 on make command line)
ifeq ($(OPENMP),1)
    CFLAGS  += -fopenmp
    LDFLAGS += -fopenmp
    CFLAGS  += -Wno-unknown-pragmas
endif

# STREAM parameters (override on command line: e.g. make STREAM_ARRAY_SIZE=100000000)
STREAM_ARRAY_SIZE ?= 20000000
NTIMES            ?= 30
SCALAR            ?= 0.42
OFFSET            ?= 0
STREAM_TYPE       ?= double

# Enable tuned kernels (set TUNED=1 on make command line)
ifdef TUNED
	CFLAGS   += -DTUNED
endif

# Preprocessor definitions
DEFS = \
	-DSTREAM_ARRAY_SIZE=$(STREAM_ARRAY_SIZE) \
	-DNTIMES=$(NTIMES) \
	-DSCALAR=$(SCALAR) \
	-DOFFSET=$(OFFSET) \
	-DSTREAM_TYPE=$(STREAM_TYPE)

# Sources and target
SRC     = stream_mpi.c
TARGET  = stream_mpi

.PHONY: all clean run info help

all: $(TARGET)

$(TARGET): $(SRC)
	$(CC) $(CFLAGS) $(DEFS) $< -o $@ $(LDFLAGS)

clean:
	rm -f $(TARGET) *.o

# Run the benchmark with mpirun (override NP for process count)
run: $(TARGET)
	mpirun -np $(NP ? 1) ./$(TARGET)

# Show current configuration
info:
	@echo "Configuration:"
	@echo "  CC:               $(CC)"
	@echo "  CFLAGS:           $(CFLAGS)"
	@echo "  LDFLAGS:          $(LDFLAGS)"
	@echo "  STREAM_ARRAY_SIZE $(STREAM_ARRAY_SIZE)"
	@echo "  NTIMES:           $(NTIMES)"
	@echo "  SCALAR:           $(SCALAR)"
	@echo "  OFFSET:           $(OFFSET)"
	@echo "  STREAM_TYPE:      $(STREAM_TYPE)"
	@echo "  OPENMP:           $(OPENMP)"
	@echo "  TUNED:            $(TUNED)"

help:
	@echo "Usage: make [TARGET] [VARIABLES]"
	@echo ""
	@echo "Targets:"
	@echo "  all      Build the STREAM MPI executable (default)"
	@echo "  clean    Remove executable and objects"
	@echo "  run      Launch benchmark with mpirun (use NP to set number of ranks)"
	@echo "  info     Display build configuration"
	@echo "  help     Show this message"
	@echo ""
	@echo "Variables:"
	@echo "  CC                MPI compiler (default: mpicc)"
	@echo "  CFLAGS            Compiler flags"
	@echo "  LDFLAGS           Linker flags"
	@echo "  OPENMP            Set to enable OpenMP flags"
	@echo "  TUNED             Set to enable tuned kernels"
	@echo "  STREAM_ARRAY_SIZE Number of elements per array (default: 20000000)"
	@echo "  NTIMES            Number of iterations (default: 30)"
	@echo "  SCALAR            Scalar coefficient (default: 0.42)"
	@echo "  OFFSET            Array offset (default: 0)"
	@echo "  STREAM_TYPE       Data type (float or double, default: double)"
	@echo "  NP                Number of MPI processes for run"