# Makefile for olcGimmeHead project
# Build configuration for C++ project

# Compiler settings
CXX := g++
CXXFLAGS := -std=c++2a -Wall -Wextra -O2
DEBUG_FLAGS := -g -O0 -DDEBUG

# Directory structure
SRC_DIR := ../src_cpp
BUILD_DIR := build
BIN_DIR := bin

# Source files
SOURCES := $(wildcard $(SRC_DIR)/*.cpp)
OBJECTS := $(SOURCES:$(SRC_DIR)/%.cpp=$(BUILD_DIR)/%.o)

# Target executable
TARGET := gimme-head
TARGET_PATH := $(BIN_DIR)/$(TARGET)

# Default target
.PHONY: all
all: $(TARGET_PATH)

# Create directories if they don't exist
$(BUILD_DIR):
	@mkdir -p $(BUILD_DIR)

$(BIN_DIR):
	@mkdir -p $(BIN_DIR)

# Compile object files
$(BUILD_DIR)/%.o: $(SRC_DIR)/%.cpp | $(BUILD_DIR)
	@echo "Compiling $<..."
	@$(CXX) $(CXXFLAGS) -c $< -o $@

# Link executable
$(TARGET_PATH): $(OBJECTS) | $(BIN_DIR)
	@echo "Linking $(TARGET)..."
	@$(CXX) $(CXXFLAGS) $(OBJECTS) -o $@
	@echo "Build complete: $(TARGET_PATH)"

# Debug build
.PHONY: debug
debug: CXXFLAGS += $(DEBUG_FLAGS)
debug: $(TARGET_PATH)

# Clean build artifacts
.PHONY: clean
clean:
	@echo "Cleaning build artifacts..."
	@rm -rf $(BUILD_DIR)
	@rm -rf $(BIN_DIR)
	@echo "Clean complete"

# Install (optional - copies to system path)
.PHONY: install
install: $(TARGET_PATH)
	@echo "Installing $(TARGET) to /usr/local/bin..."
	@cp $(TARGET_PATH) /usr/local/bin/$(TARGET)
	@chmod +x /usr/local/bin/$(TARGET)
	@echo "Installation complete"

# Uninstall
.PHONY: uninstall
uninstall:
	@echo "Removing $(TARGET) from /usr/local/bin..."
	@rm -f /usr/local/bin/$(TARGET)
	@echo "Uninstall complete"

# Run the program (requires arguments)
.PHONY: run
run: $(TARGET_PATH)
	@echo "Usage: make run ARGS='template_file output_file'"
	@echo "Example: make run ARGS='template.h output.h'"
	@if [ "$(ARGS)" ]; then \
		echo "Running: $(TARGET_PATH) $(ARGS)"; \
		$(TARGET_PATH) $(ARGS); \
	else \
		echo "Please provide ARGS parameter"; \
	fi

# Generate compile_commands.json for better IntelliSense
.PHONY: compile-commands
compile-commands:
	@echo "Generating compile_commands.json..."
	@echo '[' > compile_commands.json
	@first=true; \
	for src in $(SOURCES); do \
		if [ "$$first" = true ]; then \
			first=false; \
		else \
			echo ',' >> compile_commands.json; \
		fi; \
		echo '  {' >> compile_commands.json; \
		echo '    "directory": "$(shell pwd)",' >> compile_commands.json; \
		echo '    "command": "$(CXX) $(CXXFLAGS) -c '"$$src"' -o $(BUILD_DIR)/'$$(basename $$src .cpp)'.o",' >> compile_commands.json; \
		echo '    "file": "'"$$src"'"' >> compile_commands.json; \
		echo '  }' >> compile_commands.json; \
	done
	@echo ']' >> compile_commands.json
	@echo "compile_commands.json generated"

# Show help
.PHONY: help
help:
	@echo "Available targets:"
	@echo "  all             - Build the project (default)"
	@echo "  debug           - Build with debug symbols"
	@echo "  clean           - Remove build artifacts"
	@echo "  install         - Install to /usr/local/bin"
	@echo "  uninstall       - Remove from /usr/local/bin"
	@echo "  run             - Run the program (use ARGS='...' to pass arguments)"
	@echo "  compile-commands - Generate compile_commands.json for IntelliSense"
	@echo "  help            - Show this help message"
	@echo ""
	@echo "Examples:"
	@echo "  make"
	@echo "  make debug"
	@echo "  make clean"
	@echo "  make run ARGS='template.h output.h'"
	@echo "  make compile-commands"

# Print variables (for debugging makefile)
.PHONY: print-vars
print-vars:
	@echo "CXX: $(CXX)"
	@echo "CXXFLAGS: $(CXXFLAGS)"
	@echo "SRC_DIR: $(SRC_DIR)"
	@echo "BUILD_DIR: $(BUILD_DIR)"
	@echo "BIN_DIR: $(BIN_DIR)"
	@echo "SOURCES: $(SOURCES)"
	@echo "OBJECTS: $(OBJECTS)"
	@echo "TARGET_PATH: $(TARGET_PATH)"
