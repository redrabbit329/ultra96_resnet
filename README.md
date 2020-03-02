# ultra96_resnet
Ultra96 v2 Board Resnet50 Example Analysis



  PROJECT = resnet50
  SYSROOT = ${SDKTARGETSYSROOT}

  CXX       :=   aarch64-xilinx-linux-g++
  CC        :=   aarch64-xilinx-linux-gcc
  OBJ       :=   main.o

  # linking libraries of OpenCV
  LDFLAGS   +=   $(shell pkg-config --libs opencv)
  # linking libraries of DNNDK
  LDFLAGS   +=  -lhineon -ln2cube -ldputils -lpthread

  CUR_DIR =   $(shell pwd)
  SRC     =   $(CUR_DIR)/src
  BUILD   =   $(CUR_DIR)/build
  MODEL   =   $(CUR_DIR)/model
  VPATH   =   $(SRC)

  CFLAGS  :=  -O2 -Wall -Wpointer-arith -std=c++11 -ffast-math -mcpu=cortex-a53
  CFLAGS  += --sysroot=${SYSROOT}

  MODEL = $(CUR_DIR)/model/dpu_resnet50_0.elf

  .PHONY: all clean

  all: $(BUILD) $(PROJECT)

  $(PROJECT) : $(OBJ)
          $(CXX) $(CFLAGS) $(addprefix $(BUILD)/, $^) $(MODEL) -o $(BUILD)/$@ $(LDFLAGS)

  %.o : %.cc
          $(CXX) -c $(CFLAGS) $< -o $(BUILD)/$@

  clean:
          $(RM) -rf $(BUILD)

  $(BUILD) :
          -mkdir -p $@
