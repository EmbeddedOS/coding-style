# C coding style

- "Good programming style begins with the effective organization of code. By using a clear and consistent organization of the components of your programs, you make them more efficient, readable, and maintainable."

- Purpose:
  - Organized.
  - Easy to read.
  - Easy to understand.
  - Maintainable.
  - Efficient.

## 1. General coding standards

## 2. Readability and maintainability

### 2.1. OOP mindset

#### 2.1.1. Object

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

#### 2.1.2. Encapsulation

- **Encapsulation** means grouping related elements:
  - Organize a program into files.
  - Organize files into data sections and function sections.
  - Organize functions into logically related groups within individual files.
  - Organize data into logical groups (data structures).

#### 2.1.3. Abstraction

- Try to hide implementation as much as possible.
- Expose only necessary APIs, otherwise make it `static` and definite in file scope.
- **AVOID** global variables, no `extern`.
- **AVOID** exposing data structures with opaque type, users hold pointers only.

```c
typedef struct private_object_i_t* private_object_t;

int private_object_method(private_object_t self);
```

#### 2.1.4. Polymorphism

- Expose generic abstract interfaces work on same based objects. For example, same `read()` API works on file descriptors, that can be normal file or a device driver.

```c
/* Read normal file. */
int file_fd = open("normal_file.txt");
read(file_fd, buf, sizeof(buf));

/* Read from a device. */
int device_fd = open("/dev/your_device");
read(device_fd, buf, sizeof(buf));
```

#### 2.1.5. Inheritance

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
