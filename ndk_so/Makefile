ANDROID_NDK = /Users/milinddeore/Library/Android/sdk/ndk/21.2.6472646
TOOLCHAIN = $(ANDROID_NDK)/toolchains/llvm/prebuilt/darwin-x86_64

CXX = $(TOOLCHAIN)/bin/aarch64-linux-android28-clang++
$(info $(CXX))
CPPFLAGS = -std=c++11 -c -v -g -fPIC -Wall -Wextra -pthread -march=armv8-a -fcoroutines-ts -fvisibility=hidden --sysroot=$(TOOLCHAIN)/sysroot

ODIR = obj
LIBS = lib
DEPS = camera_engine.h camera_manager.h camera_utils.h native_debug.h
SOURCES = android_main.cpp camera_engine.cpp camera_listeners.cpp camera_manager.cpp camera_utils.cpp
CPPFLAGS += -I./cv-sdk-343/include
LDFLAGS += -shared -L$(ANDROID_NDK)/platforms/android-28/arch-arm64/usr/lib -ldl -lmediandk -lcamera2ndk -lm -llog -landroid -lc++_shared -L./cv-sdk-343/staticlibs/arm64-v8a -lopencv_core -lopencv_imgproc 

_OBJ = $(SOURCES:.cpp=.o)
OBJ = $(patsubst %,$(ODIR)/%,$(_OBJ))

$(ODIR)/%.o : %.cpp $(DEPS)
	@mkdir -p $(@D)
	$(CXX) -o $@ $< $(CPPFLAGS)

$(LIBS)/libndksamplecam.so: $(OBJ)
	$(CXX) -o $@ $^ $(LDFLAGS)

clean:
	rm -rf $(ODIR)  $(LIBS)/libndksamplecam.so
