# Original build.sh script from ticket:
#   rm -fr build
#   mkdir build
#   cd build
#   cmake ..
#   make
#   gcovrtest

BASE_OS:=$(shell uname | cut -d'-' -f1)
ifeq ($(filter $(BASE_OS),MSYS_NT MINGW64_NT),)
  CMAKE_GENERATOR := "Unix Makefiles"
else
  CMAKE_GENERATOR := "MSYS Makefiles"
endif

all:
	@echo "CC:   ${CC} (origin: $(origin CC))"
	@echo "CXX:  ${CXX} (origin: $(origin CXX))"
	@echo "GCOV: ${GCOV} (origin: $(origin GCOV))"
	mkdir -p build
	cd build && cmake -G $(CMAKE_GENERATOR) -DCMAKE_BUILD_TYPE=PROFILE -DCMAKE_TOOLCHAIN_FILE=../Toolchain.cmake .. && make

run: txt xml html sonarqube coveralls

coverage.json:
	cd build && ./gcovrtest
	$(GCOVR) --json coverage.json

txt: coverage.json
	$(GCOVR) -a $< -o coverage.txt

xml: coverage.json
	$(GCOVR) -a $< -x -o coverage.xml

html: coverage.json
	$(GCOVR) -a $< --html-details -o coverage.html

coveralls: coverage.json
	$(GCOVR) -a $<  --coveralls coveralls.json

sonarqube: coverage.json
	$(GCOVR) -a $< --sonarqube -o sonarqube.xml

clean:
	rm -rf build
	rm -f coverage*.* sonarqube*.* coveralls.json
