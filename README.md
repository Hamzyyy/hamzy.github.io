# Google Summer of Code 2024

### Contributor: *Mohamed Hassan*
## Description 
- **Year**: 2024
- **Organization**: The Real-Time Executive for Multiprocessor ([RTEMS](https://www.rtems.org/))

![gsoc_cern_banner_flat](https://github.com/Hamzyyy/hamzy.github.io/assets/48621542/af1a84ff-0099-4b37-a27c-4d17cc5c4e7c)
![rtemsorg300x160](https://github.com/Hamzyyy/hamzy.github.io/assets/48621542/a203e4b4-e9dc-40d5-ba8a-c4e5b184f879)

- **Project Proposal**: [Make Stack Checker Error Handler Configurable](https://docs.google.com/document/u/0/d/1Kn02yQQNI9qHwup5kuGEhj-9l-dpnwYgYvFceXD-BxA/mobilebasic?disco=AAABJ92rhcM)
- **Project Size**: Small
## Project Background
RTEMS real-time operating system utilizes a stack checker to detect stack overflows during execution. While this feature is essential for system stabilit the current RTEMS error handler provides limited options for customization. This project aims to enhance the RTEMS stack checker by developing a configurable error handler.

## Project Milestone
This project is divided into several milestones, each milstone there is a set of tasks. In this blog we are discussing the first milestone.
### Community Bonding Period
During this milestone I studied extensively the existing stack check utilities provided by RTEMS code base to understand the underlaying functionalities. The stack checker could be found in the following path of RTEMS code base.
```
/cpukit/libmisc/stackchk/check.c
```
Wh
