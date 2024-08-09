# C coding style

- "Good programming style begins with the effective organization of code. By using a clear and consistent organization of the components of your programs, you make them more efficient, readable, and maintainable."

- Purpose:
  - Organized.
  - Easy to read.
  - Easy to understand.
  - Maintainable.
  - Efficient.

## 1. Readability and maintainability

### 1.1. White space

- Adding white space in the form of blank lines, spaces, and indentation will significantly improve the readability of your code.

#### 1.1.1. Blank line

- Add blank lines between code "paragraphs" can greatly enhance readability by making the logical structure of a sequence of lines more obvious.

```c
int func()
{
    /* Paragraph 1: create objects. */
    int fd = 0;
    uint8_t buf[SIZE] = {0};

    /* Paragraph 2: Make the buffer. */
    get_buffer_from_network(buf, SIZE);
    parse_buffer(buf, SIZE);

    /* Paragraph 3: Write the buffer to file. */
    fd = open(FILE_NAME);
    write(fd, buf, SIZE);
}
```

- Using ony single blank line to separate two parts.

#### 1.1.2. Spacing

- Put a space after comma.
- Put a space before operators.

```c
int func(int a, int b, int c)
{
    int x, y, z;

    if (x == y)
    {
        x = y + z;
    }

    another_func(a, b, c);
}
```

#### 1.1.3. Indentation

- Using **Four spaces** for indentation. NOT **TAB**.

### 1.2. Comments

- At the program level, you can include a `README` file that provides a general description and explains its organization.
- At the file level, you can include a `file prolog` that explains the purpose of the file and provides other information.
- At the function level, a comment can serve as a `function prolog`.
- At the variable level, add a comments to explain purpose of the variables.

#### 1.2.1. Boxed comment prolog

- Using **Boxed comment prolog** for file, function, section description.

- Header files, and C files descriptions.

```c
/**
 * @file    - your file name.
 * @author  - your name (you@domain.com)
 * @brief   - Brief your file here, `purpose`, `what` and `why`.
 *
 * @version 0.1
 * @date 2023-08-14
 * 
 * @copyright Copyright (c) 2023
 */
```

- Functions and sections descriptions.

```c
/**
 * @brief   - function name - Brief your function.
 *
 * @param   - param_name - describe param.
 * @param   - param_name - describe param.
 *
 * @return  - What function return.
 * 
 * @note    - NOTE 1.
 * @note    - NOTE 2.
 */
```

#### 1.2.2. Block & Inline comment

- Use at the beginning of each major section of the code.

```c
int func()
{
    /* Perform case for either s/c position or velocity
     * vector request using the RSL routine. */
    start_section_code();
}
```

- Condition statement, closing braces with short description:

```c
int func()
{
    if (valid)
    { // Handle case valid.

    }
    else
    { // Handle case not valid.

    }
}
```

#### 1.2.3. short comment

- Write on the same line as the code or data definition they describe.

```c
#define MACRO_1 val     /* MACRO_1 description. */
#define MACRO_2 val     /* MACRO_2 description. */
#define MACRO_3 val     /* MACRO_3 description. */

struct number_t
{
    double number_1;    /* Value in double.     */
    int number_2;       /* Value in integer.    */
    int count;          /* Number of elements.  */
};
```

- Tab comment over far enough to separate it from code statements.
- If more than one short comment appears in a block of code or data definition, start all of them at the same tab position and end all at the same position.

### 1.3. Meaningful names

- Choose names for files, functions, constants, or variables that are meaningful and readable.
- Follow a uniform scheme when abbreviating names. For example, if you have a number of function associated with `watch dog`, you may want to prefix them: `wd_`.
- Use underscores within names to improve readability and clarify: `get_data_from_somewhere`.
- Do NOT rely on letter case to make a name unique.
- Do not assign variable with a `typedef` with the same name.

#### 1.3.1. Standard names

- Short names:
  - `c` - characters.
  - `i, j, k` - indices.
  - `n` - counters.
  - `p, q` - pointers.
  - `s` - strings.
- Standard suffix for variables:
  - `_ptr` - pointer.
  - `_file` - variable of type file.
  - `_fd` - file descriptor.

#### 1.3.2. Variable names

- Do NOT duplicate global variable name in functions.
- Global variables should be prepended with a `g_`, but avoid to use global variables.
- Low cases and separate by `_`.

#### 1.3.3. Capitalization

- Functions and variables use low cases and separate by `_`.
- Constants, MACRO, ENUM: Use upper-cases words separated by `_`.

#### 1.3.4. Type names

- Using low cases and separate by `_`.
- Express `what` instead of `why` or `how`.
- Prefer using suffix `_t`.

#### 1.3.5. Function names

- Start with module prefix (depends on your application), follow by a `verb` what it do, and finally is objects. For example `cloud_mgr` do `publish` a MQTT `heart_breath` message:

```c
int cloud_mgr_publish_heart_breath(int timestamp);
```

## 2. Program organization

### 2.1. Program files

- A C program consists of one or more program files, one of which contains the `main()` function, which acts as the driver of the program.
- When your program is large enough to require several files, you should use encapsulation and data hiding techniques to group logically related functions and data structures into the same files. Organize your programs as follows:
  - 1. Create a README file to document what the program does.
  - 2. Group the main function with other logically related functions in a program file.
  - 3. Use module files to group logically related functions (not including the main function).
  - 4. Use header files to encapsulation definitions and declarations of variables and functions.
  - 5. Write a `Makefile` to make re-compiles more efficient.

- Maximum number of characters per lines is 80. Configure with vscode:

```json
{
    "editor.rulers": [80, 120, 140]
}
```

### 2.2. README files

- A README file should be used to explain `what` that the program does and `how` it is organized and to document issues for the program as a whole. For example, a README file might include:
  - All conditional compilation flags and their meanings.
  - Files that are machine dependent.
  - Paths to reused components.

- Generate the repository structure 2 layers with `tree -L 2`:

```text
.
├── docs                        # General documentations.
│   ├── *.md
│   ├── video                   # Documentations from videos, Youtube, etc.
│   │   └── *.md
│   └── wiki                    # https://wiki.osdev.org/ or Wikipedia, etc.
│       └── *.md
├── hw                          # Hardware code, emulator etc.
│   └── qemu                    # QEMU emulator hardware components.
│       └── *.c, *.h, */
├── kernel                      # Kernel modules, drivers, sub-systems. 
│   ├── examples                # Example code of basic features.
│   │   └── *.c, *.h, */
│   └── *.c, *.h, */
├── Makefile                    # Global make file to build system.
├── README.md                   # README for the repository.
└── usr                         # User application codes.
    ├── examples                # Example code for basic features.
    │   └── *.c, *.h, */
    └── *.c, *.h, */
```

### 2.3. Header files

- Header files are used to encapsulate logically related ideas;
  - For example the header file `time.h` defines two constants, three structures, and declares seven functions needed to process time.
- Header files may be selectively included in your program files to limit visibility to only those functions that need them.

- Use `#include <system_file>` for system include files.
- Use `#include "user_file"` for user include files.
- Contain in header files data definitions, declarations typedef, and enums that **ARE NEEDED** by more than one program.
- Organize headers by function.
- Put declarations for separate subsystems in separate header files.
- If a set of declarations is likely to change when code is ported from one platform to another, put those declarations in a separate header file.
- Include header files that declare functions or external variables in the file that defines the function or variable. By that way, the compiler can do type checking and the external declaration will always agree with the definition.
- Do not nested header files.

```c
/**
 * @file    - your file name.
 * @author  - your name (you@domain.com)
 * @brief   - Brief your file here, `purpose`, `what` and `why`.
 *
 * @version 0.1
 * @date 2023-08-14
 * 
 * @copyright Copyright (c) 2023
 */

#pragma once

#include <system_file>
#include <system_file>

#include "user_file"
#include "user_file"

/* Public defines ------------------------------------------------------------*/
#define MACRO_1 val     /* MACRO_1 description. */
#define MACRO_2 val     /* MACRO_2 description. */
#define MACRO_3 val     /* MACRO_3 description. */

/**
 * @brief   - This macro need to explain in multiple lines. Using block comment
 *            instead.
 */
#define MACRO_4 val

/* Public types --------------------------------------------------------------*/
/**
 * @brief   - struct number_t - Describe your structure here.
 * @param   - number_1 - Here is number 1.
 * @param   - number_2 - Here is number 2.
 * 
 * @note    - Some notes when manipulate with the structure.
 */
struct number_t
{
    double number_1;    /* Value in double.     */
    int number_2;       /* Value in integer.    */
    int count;          /* Number of elements.  */
};

/* Public function prototypes ------------------------------------------------*/
/**
 * @brief   - calculate_number - Brief your function.
 *
 * @param   - a - describe param.
 * @param   - b - describe param.
 *
 * @return  - What function return.
 * 
 * @note    - NOTE 1.
 */
int calculate_number(int a, int b);
```

### 2.4. Module files

- A module file contains the logically related functions, constants, types, data definitions and declarations, and functions. Modules are similar to a program file except that they don't contain the `main()` function.

- Example of module file:

```c
#include <system_file>
#include <system_file>

#include "user_file"
#include "user_file"

#include "corresponding_header_file"

/* Private defines -----------------------------------------------------------*/
#define DEVICE_NAME_MAX_LENGTH 50

/* Private types -------------------------------------------------------------*/
typedef int device_id_t;

/**
 * @brief   - device_t - This structure represent a device.
 *
 * @param   - name - device name.
 * @param   - id - device id.
 * @param   - device interrupt number.
 */
struct device_t
{
    const char *name;
    device_id_t id;
    int interrupt_number;
};

/* Private function prototypes -----------------------------------------------*/
/**
 * @brief   - device_enable_interrupt - Brief your function.
 *
 * @param   - self - self object.
 * @return  - What function return.
 * 
 * @note    - NOTE 1.
 */
static int device_enable_interrupt(device_t *self);

/**
 * @brief   - device_calculate_id - Brief your function.
 *
 * @param   - self - self object.
 * @return  - What function return.
 * 
 * @note    - NOTE 1.
 */
static inline int device_calculate_id(device_t *self);

/* Private functions ---------------------------------------------------------*/
static int device_enable_interrupt(device_t *self)
{
    return 0;
}

static inline int device_calculate_id(device_t *self)
{
    return 0;
}

/* Public functions ----------------------------------------------------------*/
int device_write(device_t *self, uint8_t *buf, uint16_t length)
{
    return 0;
}
```

### 2.5. Makefile

- Makefile are used on some systems to provide a mechanism for efficiently re-compiling C code. With `Makefile`, the `make` utility re-compiles files that have been changed since the last compilation.
- The Make file:
  - Lists all file that are to be included as part of the program.
  - Contains comments documenting what files are part of libraries.
  - Demonstrates dependencies, e.g., source files and associated headers using implicit and explicit rules.

```makefile
# Makefile for UIX testing ..
#
# Cong. Programmer
#
# This makefile can build 2 different executables. The executables share some
# of the same code and share libraries.

#
# Object code for executables
#
INIT_OBJS = seq_init.o seq_drv.o

LOAD_OBJS = load_drv.o \
    seq_load.o \
    print_seq.o

#
# These are included in all executables.
#
OBJS = main.o stubs.o data.o

#
# Include paths.
#
INCDIR = -I/u/home/cong/dev/include \
        -I./include

INTERNAL_DEFINES = -DTEST_NO_DATA
DEFINES = 
DEBUG = -g
CUSTOM_FLAGS = -posix -W3 -DENABLE_IPV6
CFLAGS = $(DEBUG) $(CUSTOM_FLAGS) $(INCDIR) $(DEFINES) \
    $(INTERNAL_DEFINES)

#
# Libraries.
#
PROT_LIBS = -lmqtt -lrc -lsocket
LIBS = -lpthread
LIB_DIR = -L/home/cong/mqtt/ -L/usr/lib/custom_path

#
# Compilation for the executables ..o
#
module_init:
    $(CC) $(CFLAGS) -o module_init $(INIT_OBJS) $(OBJS) $(PROT_LIBS)

module_load:
    $(CC) $(CFLAGS) -o module_init $(INIT_OBJS) $(OBJS) $(PROT_LIBS)

all: module_init module_load

clean:
    rm $(INIT_OBJS) $(LOAD_OBJS)

depend:
    makedepend -- $(CFLAGS) -- "/bin/is *.c"

# DO NOT DELETE THIS LINE -- make depends on it.

# [a jillion lines that are dependencies generated by makedepend go here]
```

## 4. Program behavior

## 5. OOP mindset

### 5.1. Object

- `Object` mindset, software entities have:
  - Life-cycle: construction - destruction. Everything that is created, need to be destroyed.
  - Properties: group related elements to become structures.
  - Behaviors: methods act on objects.

- Object context:

```c
/* Create and destroy object. */
int object_init(object_t *self);
int object_deinit(object_t *self);
```

- Function context:

```c
/* Create and destroy function context. */
int func()
{
    int res = 0;

    /* Do create context. */
    res = create_step_1();
    if (res < 0)
    {
        goto handle_step_1_failure;
    }

    res = create_step_2();
    if (res < 0)
    {
        goto handle_step_2_failure;
    }

    /* Do release context. */
handle_step_2_failure:
    release_step_1();
handle_step_1_failure:
    return res;
};
```

### 5.2. Encapsulation

- **Encapsulation** means grouping related elements:
  - Organize a program into files.
  - Organize files into data sections and function sections.
  - Organize functions into logically related groups within individual files.
  - Organize data into logical groups (data structures).

### 5.3. Abstraction

- Try to hide implementation as much as possible.
- Expose only necessary APIs, otherwise make it `static` and definite in file scope.
- **AVOID** global variables, no `extern`.
- **AVOID** exposing data structures with opaque type, users hold pointers only.

```c
typedef struct private_object_i_t* private_object_t;

int private_object_method(private_object_t self);
```

### 5.4. Polymorphism

- Expose generic abstract interfaces work on same based objects. For example, same `read()` API works on file descriptors, that can be normal file or a device driver.

```c
/* Read normal file. */
int file_fd = open("normal_file.txt");
read(file_fd, buf, sizeof(buf));

/* Read from a device. */
int device_fd = open("/dev/your_device");
read(device_fd, buf, sizeof(buf));
```

### 5.5. Inheritance

- Don't duplicate properties on objects, make a base structure and inherit it.

```c
struct device_t
{
    const char *name;
    int interrupt_number;
};

struct network_device_t
{
    struct device_t dev;
    queue_t rx _queue;
};
```
