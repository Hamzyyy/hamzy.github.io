# Google Summer of Code 2024

### Contributor: *Mohamed Hassan*
## Description 
- **Year**: 2024
- **Organization**: The Real-Time Executive for Multiprocessor ([RTEMS](https://www.rtems.org/))

![gsoc_cern_banner_flat](https://github.com/Hamzyyy/hamzy.github.io/assets/48621542/af1a84ff-0099-4b37-a27c-4d17cc5c4e7c)
![rtemsorg300x160 1](https://github.com/Hamzyyy/hamzy.github.io/assets/48621542/2cff10ea-3d7b-43d0-8c80-c933e8ad4172)


- **Project Proposal**: [Make Stack Checker Error Handler Configurable](https://docs.google.com/document/u/0/d/1Kn02yQQNI9qHwup5kuGEhj-9l-dpnwYgYvFceXD-BxA/mobilebasic?disco=AAABJ92rhcM)
- **Project Size**: Small
## Project Background
RTEMS real-time operating system utilizes a stack checker to detect stack overflows during execution. While this feature is essential for system stability the current RTEMS error handler provides limited options for customization. This project aims to enhance the RTEMS stack checker by developing a configurable error handler.

## Project Milestones
This project is divided into several milestones, each milstone has a set of tasks. In this blog we are discussing the first milestone.
### Community Bonding Period
During this milestone I studied extensively the existing stack check utilities provided by RTEMS code base to understand the underlaying functionalities. The stack checker could be found in the following path of RTEMS code base.
```
rtems/cpukit/libmisc/stackchk/check.c
```
When inspecting the check.c we will find that there are few services it provides to check for stack overflows. These services use helper fuctions to do smaller task. For example:
```
void rtems_stack_checker_iterate( rtems_stack_checker_visitor visit, void *arg )
```
this function iterate through all tasks to get stack usage for every task. This function is built on top of another helper function that get the stack usage which is:
```
static void Stack_check_Visit_stack(const Stack_Control *stack, const void *current, const char *name, rtems_id id, rtems_stack_checker_visitor visit, void *arg)
```
There are also services that are used for generating reports of stack stack usage, such as:
```
void rtems_stack_checker_report_usage( void )
```
This function uses two other functions for iterating through tasks stacks and for printing their usage:
for printing texsts
```
void rtems_print_printer_printk(rtems_printer *printer)
```
And for getting stacks info like task name, ID, stack base address, high water mark , used and available storage, it uses another function:
```
void rtems_stack_checker_report_usage_with_plugin(const rtems_printer* printer)
```
In addition, RTEMS provide test suites for the stack check error handler. The test cases cover the main services. The test suites can be found in
```
rtems/testsuites/libtests/
```
There are three directories related to the stack checker:
```
rtems/testsuites/libtests/stackchk
rtems/testsuites/libtests/stackchk01
rtems/testsuites/libtests/stackchk02
```
- At stackchk we can find three test files: `blow.c` which has only one function called:
```
void blow_stack(void)
```
This function is responsible for corrupting the stack content by overwriting the last and first four words of the stack. Then it calls `rtems_stack_checker_report_usage()`. Apparently it tests if the stack checker will detect corrupting the stack by overwriting the predefined sanity pattern. There is also `task1.c` which also has only one function called:
```
rtems_task Task_1_through_3(rtems_task_argument argument)
```
This function retrieves the date and time. Then after a delay of 15 seconds it calls `blow_stack()`. Finally, there is `init.c` which has two functions:
```
rtems_task Init(rtems_task_argument argument)
```
Which create three tasks with the default stack size, default mode and give them priorities equals to their names. Then it calls `Task_1_through_3()`. Another function in `init.c` is
```
void Fatal_extension(rtems_fatal_source source, bool always_set_to_false, rtems_fatal_code   error)
```
Which detects if the source of error is the stack checker or not, then it ends the test safely.
- stackchk01 has only one file `init.c` it contains two functions:
```
static void stack_visitor( const rtems_stack_checker_info *info, void *arg )
```
a private function that retrieves the task id and used stack. The other function is
```
static rtems_task Init( rtems_task_argument argument )
```
This function performs three tasks, it calls `rtems_stack_checker_is_blown()`, it calls `rtems_stack_checker_iterate()` and it prints the stack usage and limit then checks if usage is less than the limit. Finally it ends the test.
- stackchk02 has only one function:
```
rtems_task Init(rtems_task_argument argument)
```
which tests `rtems_stack_checker_report_usage()`.

### What Next
In the next milestone I'm planning to start add configuration option for the existing stack checking services. As proposed at the project proposal. There are several features that could be added to enhance configurability. For the first part I may add a feature that enables the user to add their desired sentinel value. After that 
