# set environment variable VKM_INCLUDE_DIR to the location of valkeymodule.h
ifndef VKM_INCLUDE_DIR
	VKM_INCLUDE_DIR=../
endif

CFLAGS ?= -g -fPIC -lc -lm -O3 -std=gnu99 -I$(VKM_INCLUDE_DIR) -Wall -Wno-unused-function
uname_S := $(shell sh -c 'uname -s 2>/dev/null || echo not')
ifneq ($(uname_S),Linux)
	CFLAGS += -mmacosx-version-min=10.6
endif
export CFLAGS

CC:=$(shell sh -c 'type $(CC) >/dev/null 2>/dev/null && echo $(CC) || echo gcc')

OBJS=util.o strings.o sds.o vector.o heap.o priority_queue.o alloc.o periodic.o cmdparse.o

all: libvkmutil.a

clean:
	rm -rf *.o *.a

libvkmutil.a: $(OBJS)
	ar rcs $@ $^

test: all test_vector test_heap test_priority_queue test_periodic test_cmdparse

test_vector: test_vector.o vector.o
	$(CC) -Wall -o test_vector vector.o test_vector.o -lc -O0
	@(sh -c ./test_vector)

test_heap: test_heap.o heap.o vector.o
	$(CC) -Wall -o test_heap heap.o vector.o test_heap.o -lc -O0
	@(sh -c ./test_heap)

test_priority_queue: test_priority_queue.o priority_queue.o heap.o vector.o
	$(CC) -Wall -o test_priority_queue priority_queue.o heap.o vector.o test_priority_queue.o -lc -O0
	@(sh -c ./test_priority_queue)

test_periodic: test_periodic.o periodic.o
	$(CC) -Wall -o $@ $^ -lc -lpthread -O0
	@(sh -c ./$@)

test_cmdparse: test_cmdparse.o cmdparse.o
	$(CC) -Wall -o $@ $^ -lc -lpthread -O0
	@(sh -c ./$@)
