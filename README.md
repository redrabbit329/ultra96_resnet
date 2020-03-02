# ultra96_resnet
Ultra96 v2 Board Resnet50 Example Analysis

CC=<컴파일러>
CFLAGS=<컴파일 옵션>
LDFLAGS=<링크 옵션>
LDLIBS=<링크 라이브러리 목록>
OBJS=<Object 파일 목록>
TARGET=<빌드 대상 이름>
 
all: $(TARGET)
 
clean:
    rm -f *.o
    rm -f $(TARGET)
 
$(TARGET): $(OBJS)
$(CC) -o $@ $(OBJS)

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
