
COMMON_H = $(shell ls ../common/*.h)

MT19937AR_OBJ = ../../3rdparty/mt19937ar/mt19937ar.o
MT19937AR_H = ../../3rdparty/mt19937ar/mt19937ar.h
MT19937AR_INCLUDE = -I../../3rdparty/mt19937ar

LIBCONFIG_H = $(shell ls ../../3rdparty/libconfig/*.h)
LIBCONFIG_AR = ../../3rdparty/libconfig/obj/libconfig.a
LIBCONFIG_INCLUDE = -I../../3rdparty/libconfig

TEST_SPINLOCK_OBJ=obj/test_spinlock.o
TEST_SPINLOCK_H=
TEST_SPINLOCK_DEPENDS=obj $(TEST_SPINLOCK_OBJ) ../common/obj_sql/common_sql.a ../common/obj_all/common.a $(MT19937AR_OBJ)



#####################################################################
.PHONY :all test clean

all: test

test: test_spinlock

clean:
	@echo "	CLEAN	test"
	@rm -rf *.o obj ../../test_spinlock

help:
	@echo "possible targets are 'all' 'test' 'clean' 'help'"
	@echo "'test'   - builds test_spinlock test atm"
	@echo "'all'    - builds all above targets"
	@echo "'clean'  - cleans builds and objects"
	@echo "'help'   - outputs this message"

#####################################################################

test_spinlock: $(TEST_SPINLOCK_DEPENDS)
	@echo "	LD	$@"
	@gcc  -march=i686 -L/usr/lib -o ../../test_spinlock $(TEST_SPINLOCK_OBJ) ../common/obj_sql/common_sql.a ../common/obj_all/common.a $(MT19937AR_OBJ) $(LIBCONFIG_AR) -lpcre -lrt -lm -lz  -ldl -rdynamic -L/usr/lib/mysql -lmysqlclient

# object directories

obj:
	@echo "	MKDIR	obj"
	@-mkdir obj

# login object files

obj/%.o: %.c $(COMMON_H) $(MT19937AR_H) $(LIBCONFIG_H)
	@echo "	CC	$<"
	@gcc -g -O2 -pipe -ffast-math -Wall -march=i686 -Wno-unused-parameter -Wno-clobbered -Wempty-body -Wno-switch -Wno-missing-field-initializers -fno-strict-aliasing $(MT19937AR_INCLUDE) $(LIBCONFIG_INCLUDE) -DWITH_SQL -I/usr/include/mysql  -DMAXCONN=16384 -I../common -DHAS_TLS -DHAVE_SETRLIMIT -DHAVE_STRNLEN  -I/usr/include -DHAVE_MONOTONIC_CLOCK -c $(OUTPUT_OPTION) $<

# missing object files
../common/obj_all/common.a:
	@$(MAKE) -C ../common sql
	
../common/obj_sql/common_sql.a:
	@$(MAKE) -C ../common sql

MT19937AR_OBJ:
	@$(MAKE) -C ../../3rdparty/mt19937ar

LIBCONFIG_AR:
	@$(MAKE) -C ../../3rdparty/libconfig
