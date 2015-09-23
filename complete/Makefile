CXX=c++
CXXFLAGS=-std=gnu++0x

CLANG_PREFIX=$(shell (cd /usr/local/include/clang-c/ && cd ../.. && pwd) 2> /dev/null || (cd /usr/lib/llvm-3.8/ && pwd) 2> /dev/null || (cd /usr/lib/llvm-3.7/ && pwd) 2> /dev/null || (cd /usr/lib/llvm-3.6/ && pwd) 2> /dev/null || (cd /usr/lib/llvm-3.5/ && pwd) 2> /dev/null || (cd /usr/lib/llvm-3.4/ && pwd) 2> /dev/null || (cd /usr/lib/llvm-3.3/ && pwd) 2> /dev/null || (cd /usr/local/opt/llvm && pwd) 2> /dev/null)
CLANG_FLAGS=-I$(CLANG_PREFIX)/include/
CLANG_LIBS=-L$(CLANG_PREFIX)/lib/ -lclang

FLAGS=$(CLANG_FLAGS)
LIBS=$(CLANG_LIBS)

LIB_NAME=libcomplete
SRC=complete

UNAME_S := $(shell uname -s)
ifeq ($(UNAME_S),Linux)
    COMPLETE_LDFLAGS = -shared -Wl,-soname,$(LIB_NAME).so -o $(LIB_NAME).so $(SRC).o $(LIBS) -Wl,-rpath=$(CLANG_PREFIX)/lib
endif
ifeq ($(UNAME_S),Darwin)
    COMPLETE_LDFLAGS = -dynamiclib -Wl,-install_name,$(LIB_NAME).dylib -o $(LIB_NAME).dylib $(SRC).o $(LIBS)
    # CROOT - needed to add the -lstdc++.6 to appropriately link against that lib. Not sure why
    # found the solution here:
    # http://mathematica.stackexchange.com/questions/34692/mathlink-linking-error-after-os-x-10-9-mavericks-upgrade
    # LDFLAGS = -lstdc++.6 -dynamiclib -Wl,-install_name,$(LIB_NAME).dylib -o $(LIB_NAME).dylib $(SRC).o $(LIBS)
endif

all:
	$(CXX) $(CXXFLAGS) $(FLAGS) -DNDEBUG -Os -c -fPIC complete.cpp -o $(SRC).o
	$(CXX) $(COMPLETE_LDFLAGS) $(LDFLAGS)
debug:
	$(CXX) $(CXXFLAGS) $(FLAGS) -g -c -fPIC -DCLANG_COMPLETE_LOG $(SRC).cpp -o $(SRC).o
	$(CXX) $(COMPLETE_LDFLAGS) $(LDFLAGS)
log:
	$(CXX) $(CXXFLAGS) $(FLAGS) -Os -c -fPIC -DCLANG_COMPLETE_LOG $(SRC).cpp -o $(SRC).o
	$(CXX) $(COMPLETE_LDFLAGS) $(LDFLAGS)
