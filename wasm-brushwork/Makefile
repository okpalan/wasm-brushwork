CC=gcc
AR=ar
AR_FLAGS=rcs
CFLAGS= -Wall -Wextra -Wpedantic -I$(INCLUDE_DIR) -MMD -std=c99 -lm

SRC_DIR=./src
OBJ_DIR=./obj
INCLUDE_DIR=./src/include
LIB_DIR=./lib
LIB=$(LIB_DIR)/libbrushwork.a

SOURCES := $(wildcard $(SRC_DIR)/*.c)
OBJECTS := $(SOURCES:$(SRC_DIR)/%.c=$(OBJ_DIR)/%.o)

DEPS_DIR=./deps
DEPSFILES := $(OBJECTS:.o=.d)

WASM_DIR=./wasm-*
WASM_SRC_DIR=$(WASM_DIR)/src
WASM_OBJ_DIR=$(WASM_DIR)/obj

WASM_SOURCES := $(wildcard $(WASM_SRC_DIR)/*.c)

WASM_OBJECTS := $(WASM_SOURCES:$(WASM_SRC_DIR)/%.c=$(WASM_OBJ_DIR)/%.o)
OUTPUT_DIR=./bin

all: $(LIB) $(OUTPUT_DIR)/wasm-raytracer $(OUTPUT_DIR)/wasm-rigging

clean:
	rm -rf $(OBJ_DIR) $(LIB_DIR) $(WASM_OBJ_DIR) $(OUTPUT_DIR)

build: all

$(LIB): $(OBJECTS)
	mkdir -p $(LIB_DIR)
	$(CC) $(CFLAGS) -c $(SOURCES) -o $(OBJECTS)
	$(AR) $(AR_FLAGS) $@ $(OBJECTS)


$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c
	mkdir -p $(OBJ_DIR)

$(OUTPUT_DIR)/wasm-raytracer: $(WASM_OBJECTS) src/main.c | $(WASM_SOURCES)
	mkdir -p $(OUTPUT_DIR)
	$(CC) -o $@ $^


$(OUTPUT_DIR)/wasm-rigging: $(WASM_OBJECTS) src/main.c  | $(WASM_SOURCES)
	mkdir -p $(OUTPUT_DIR)
	$(CC) -o $@ $^

post-build:
	@mkdir -p $(DEPS_DIR)
	@mv $(DEPSFILES) $(DEPS_DIR)

$(WASM_OBJ_DIR)/%.o: $(WASM_SRC_DIR)/%.c
	mkdir -p $(WASM_OBJ_DIR)
	$(CC) $(CFLAGS) -c $< -o $@

$(WASM_DIRS)/build/%: $(WASM_OBJECTS)
	mkdir -p $(WASM_DIRS)/build
	$(CC) -o $@ $^

-include $(DEPSFILES)
