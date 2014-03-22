PROJ_NAME = Juan

CC_LIN = gcc
CC_WIN = i686-w64-mingw32-gcc
C_FLAGS = -c -Wall -O3 -static-libgcc -I./inc/
B_FLAGS = -Wall -O3
LIB_FLAGS = -lm -L../lib64/ -lcsfml-audio -lcsfml-graphics -lcsfml-window -lcsfml-system

SOURCES_OBJ = libcsfml-sys.o func.o mod.o main.o
BIN_PATH = ../bin/$(PROJ_NAME)
DIR_SHARED_LIBS = ../lib64

DIR_CLOUD = ~/Ubuntu\ One/Development
DIR_VBOX_VMS = ~/Virtual*
CLOUD_SYNC_CMD = u1sdtool --start && u1sdtool --connect && echo "> U1SD started; be sure to --quit it once the sync is over."

GRANT_ALL_RIGHTS = chmod a+rwx


build_linux:    links libsfsys.o func.o mod.o main.o 
	$(CC_LIN) $(B_FLAGS) $(SOURCES_OBJ) -o $(BIN_PATH) $(LIB_FLAGS)

build_windows:  libsfsys.o func.o mod.o main.o 
	$(CC_WIN) $(B_FLAGS) $(SOURCES_OBJ) -o $(BIN_PATH) $(LIB_FLAGS)


links: ../data/ ../lib64/
	cd ../bin && ln -s --force ../data data && ln -s --force ../lib64 lib64


libsfsys.o: libcsfml-sys.c
	$(CC) $(C_FLAGS) libcsfml-sys.c

func.o: func.c
	$(CC) $(C_FLAGS) func.c

mod.o: mod.c
	$(CC) $(C_FLAGS) mod.c

main.o: main.c
	$(CC) $(C_FLAGS) main.c


Installer: Installer.c
	$(CC) $(B_FLAGS) -static-libgcc -o ../Installer Installer.c


play:
	export LD_LIBRARY_PATH="$(DIR_SHARED_LIBS)" && cd ../bin && ./$(PROJ_NAME)

ldd:
	export LD_LIBRARY_PATH="$(DIR_SHARED_LIBS)" && cd ../bin && ldd ./$(PROJ_NAME)


CMD_MISSING_SHARED_LIBS_FINDER =  make ldd | awk '$$1 !~ /^[.][/]$(DIR_SHARED_LIBS)/ {print $$3}' | grep ^/

shared_lib64:                $(DIR_SHARED_LIBS)
	test -n "`$(CMD_MISSING_SHARED_LIBS_FINDER)`"  &&  echo "> Copying shared libs to $(DIR_SHARED_LIBS)"  &&  cp `$(CMD_MISSING_SHARED_LIBS_FINDER)` $(DIR_SHARED_LIBS)/

set_shared_libs_permissions: $(DIR_SHARED_LIBS)
	$(GRANT_ALL_RIGHTS) $(DIR_SHARED_LIBS)/*

check_missing_shared_libs:   $(DIR_SHARED_LIBS)
	$(CMD_MISSING_SHARED_LIBS_FINDER)


copy_to_cloud:    ../../*$(PROJ_NAME)*/     ../../*$(PROJ_NAME)*.geany
	@echo "> Copying test iso"
	cp ../../$(PROJ_NAME).iso $(DIR_CLOUD)
	@echo "> Deleting old tar.gz"
	touch $(DIR_CLOUD)/$(PROJ_NAME).tar.gz
	rm $(DIR_CLOUD)/$(PROJ_NAME).tar.gz
	@echo "-----------------------"	
	@echo "> Updating tar"
	tar -uvf $(DIR_CLOUD)/$(PROJ_NAME).tar ../../*$(PROJ_NAME)*/ ../../*$(PROJ_NAME)*.geany
	@echo "-----------------------"
	@echo "> Gzipping tar"
	gzip -9 $(DIR_CLOUD)/$(PROJ_NAME).tar
	@echo "-----------------------"
	@echo "> Starting U1 Sync Daemon"
	$(CLOUD_SYNC_CMD)	

copy_to_iso:    ../../*$(PROJ_NAME)*/
	touch ../../$(PROJ_NAME).iso.d && rm -r ../../$(PROJ_NAME).iso.d
	cp -r ../../LD28-$(PROJ_NAME) ../../$(PROJ_NAME).iso.d
	rm -r ../../$(PROJ_NAME).iso.d/src/WIP && mkisofs -r -J -o ../../$(PROJ_NAME).iso ../../$(PROJ_NAME).iso.d
	cp ../../$(PROJ_NAME).iso $(DIR_VBOX_VMS)


clean:
	rm *.o
#	rm *.o *.os ./$(PROJ_NAME)
