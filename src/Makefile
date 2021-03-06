#
# Makefile for the RedBase project.
#
# Author: Hyunjung Park (hyunjung@cs.stanford.edu)
#


#
# Global variables
#
CC             = g++
BUILD_DIR      = ../build/
LIB_DIR        = ../lib/
INC_DIRS       = -I.
AR             = ar -rc
RANLIB         = ranlib
YACC           = bison -dy
LEX            = flex

# -g - Debugging information
# -O1 - Basic optimization
# -Wall - All warnings
# -DDEBUG_PF - This turns on the LOG file for lots of BufferMgr info
CFLAGS         = -g -O1 -Wall $(STATS_OPTION) $(INC_DIRS) --std=c++0x

# The STATS_OPTION can be set to -DPF_STATS or to nothing to turn on and
# off buffer manager statistics.  The student should not modify this
# flag at all!
STATS_OPTION   = -DPF_STATS

#
# Students: Please modify SOURCES variables as needed.
#
PF_SOURCES     = pf_buffermgr.cc pf_error.cc pf_filehandle.cc \
                 pf_pagehandle.cc pf_hashtable.cc pf_manager.cc \
                 pf_statistics.cc statistics.cc
RM_SOURCES     = rm_manager.cc rm_error.cc rm_filehandle.cc \
                 rm_filescan.cc rm_rid.cc rm_record.cc
IX_SOURCES     = ix_manager.cc ix_indexhandle.cc ix_indexscan.cc \
		 		 ix_error.cc
SM_SOURCES     = sm_manager.cc sm_error.cc printer.cc
QL_SOURCES     = ql_manager.cc ql_operators.cc ql_error.cc
EX_SOURCES	   = ex_commlayer.cc ex_error.cc
UTILS_SOURCES  = dbcreate.cc dbdestroy.cc redbase.cc
PARSER_SOURCES = scan.c parse.c nodes.c interp.c
TESTER_SOURCES = pf_test1.cc pf_test2.cc pf_test3.cc rm_test.cc
#TESTER_SOURCES = parser_test.cc pf_test1.cc pf_test2.cc pf_test3.cc rm_test.cc ix_test.cc ix_testkpg.cc

PF_OBJECTS     = $(addprefix $(BUILD_DIR), $(PF_SOURCES:.cc=.o))
RM_OBJECTS     = $(addprefix $(BUILD_DIR), $(RM_SOURCES:.cc=.o))
IX_OBJECTS     = $(addprefix $(BUILD_DIR), $(IX_SOURCES:.cc=.o))
SM_OBJECTS     = $(addprefix $(BUILD_DIR), $(SM_SOURCES:.cc=.o))
QL_OBJECTS     = $(addprefix $(BUILD_DIR), $(QL_SOURCES:.cc=.o))
EX_OBJECTS     = $(addprefix $(BUILD_DIR), $(EX_SOURCES:.cc=.o))
UTILS_OBJECTS  = $(addprefix $(BUILD_DIR), $(UTILS_SOURCES:.cc=.o))
PARSER_OBJECTS = $(addprefix $(BUILD_DIR), $(PARSER_SOURCES:.c=.o))
TESTER_OBJECTS = $(addprefix $(BUILD_DIR), $(TESTER_SOURCES:.cc=.o))
OBJECTS        = $(PF_OBJECTS) $(RM_OBJECTS) $(TESTER_OBJECTS)
#$(RM_OBJECTS) $(IX_OBJECTS) \
                 $(SM_OBJECTS) $(QL_OBJECTS) $(EX_OBJECTS) \
                 $(PARSER_OBJECTS) $(TESTER_OBJECTS) $(UTILS_OBJECTS)

LIBRARY_PF     = $(LIB_DIR)libpf.a
LIBRARY_RM     = $(LIB_DIR)librm.a
LIBRARY_IX     = $(LIB_DIR)libix.a
LIBRARY_SM     = $(LIB_DIR)libsm.a
LIBRARY_QL     = $(LIB_DIR)libql.a
LIBRARY_EX     = $(LIB_DIR)libex.a
LIBRARY_PARSER = $(LIB_DIR)libparser.a
LIBRARIES      = $(LIBRARY_PF) $(LIBRARY_RM)
#$(LIBRARY_RM) $(LIBRARY_IX) \
                 $(LIBRARY_SM) $(LIBRARY_QL) $(LIBRARY_EX) $(LIBRARY_PARSER)

UTILS          = $(UTILS_SOURCES:.cc=)
TESTS          = $(TESTER_SOURCES:.cc=)
EXECUTABLES    = $(UTILS) $(TESTS)

<<<<<<< HEAD
LIBS           =  -lrm -lpf
=======
LIBS           = -lrm -lpf 
>>>>>>> f26b82ca9ec6ae3d1942a9e608a5d571f55a91c0
#LIBS           = -lparser -lql -lsm -lix -lrm -lpf -lex

#
# Build targets
#
all: $(LIBRARIES) #$(UTILS)

clean:
	rm -f $(BUILD_DIR)*.o $(BUILD_DIR)*.d y.output y.tab.h parse.c $(LIBRARIES) $(EXECUTABLES)

testers: all $(TESTS)

#
# Libraries
#
$(LIBRARY_PF): $(PF_OBJECTS)
	$(AR) $(LIBRARY_PF) $(PF_OBJECTS)
	$(RANLIB) $(LIBRARY_PF)

$(LIBRARY_RM): $(RM_OBJECTS)
	$(AR) $(LIBRARY_RM) $(RM_OBJECTS)
	$(RANLIB) $(LIBRARY_RM)

$(LIBRARY_IX): $(IX_OBJECTS)
	$(AR) $(LIBRARY_IX) $(IX_OBJECTS)
	$(RANLIB) $(LIBRARY_IX)

$(LIBRARY_SM): $(SM_OBJECTS)
	$(AR) $(LIBRARY_SM) $(SM_OBJECTS)
	$(RANLIB) $(LIBRARY_SM)

$(LIBRARY_QL): $(QL_OBJECTS)
	$(AR) $(LIBRARY_QL) $(QL_OBJECTS)
	$(RANLIB) $(LIBRARY_QL)

$(LIBRARY_EX): $(EX_OBJECTS)
	$(AR) $(LIBRARY_EX) $(EX_OBJECTS)
	$(RANLIB) $(LIBRARY_EX)

$(LIBRARY_PARSER): $(PARSER_OBJECTS)
	$(AR) $(LIBRARY_PARSER) $(PARSER_OBJECTS)
	$(RANLIB) $(LIBRARY_PARSER)

#
# Parser
#
y.tab.h: parse.c

parse.c: parse.y
	$(YACC) parse.y; mv y.tab.c parse.c

scan.c: scan.l scanhelp.c y.tab.h
	$(LEX) scan.l; mv lex.yy.c scan.c

$(BUILD_DIR)parse.o: parse.c

$(BUILD_DIR)scan.o: scan.c y.tab.h

$(BUILD_DIR)nodes.o: nodes.c

$(BUILD_DIR)interp.o: interp.c

#
# Rules
#
-include $(OBJECTS:.o=.d)

$(BUILD_DIR)%.d: %.cc
	@set -e; \
	 rm -f $@; \
	 $(CC) $(CFLAGS) -MM -MT $(@:.d=.o) $< > $@.$$$$; \
	 sed 's,\($*\)\.o[ :]*,\1.o $@: ,g' $@.$$$$ > $@; \
	 rm -f $@.$$$$

$(OBJECTS): %.o:
	$(CC) $(CFLAGS) -c $< -o $@

$(EXECUTABLES): %: $(BUILD_DIR)%.o $(LIBRARIES)
	$(CC) $(CFLAGS) $< -o $@ -L$(LIB_DIR) $(LIBS)

