# Keystone Driver Project - Complete Setup and Usage Guide

## 📦 Overview

This project demonstrates how to build, install, test, and clean up a simple Linux kernel module named **keystone_driver**. It's designed for users with **no prior kernel module experience**. This guide walks through creating the driver source code, Makefile, compiling, testing, and cleaning up — step by step on a **single-node** Linux system.

---

## 🧰 Step 1: Prerequisites

### ✅ Required Tools (Install These First)

```bash
sudo apt update
sudo apt install build-essential linux-headers-$(uname -r) git
```

### 🔍 Confirm Setup

```bash
gcc --version
make --version
uname -r
ls /usr/src/linux-headers-$(uname -r)
```

---

## 📁 Step 2: Create Project Directory

```bash
mkdir ~/keystone-driver && cd ~/keystone-driver
```

---

## 🧾 Step 3: Write the Driver Code

Create a file called `keystone-driver.c`:

```c
#include <linux/init.h>
#include <linux/module.h>
#include <linux/kernel.h>

MODULE_LICENSE("GPL");
MODULE_AUTHOR("Your Name");
MODULE_DESCRIPTION("A simple Keystone Linux kernel module");
MODULE_VERSION("0.1");

static int __init keystone_init(void) {
    printk(KERN_INFO "Keystone driver loaded\n");
    return 0;
}

static void __exit keystone_exit(void) {
    printk(KERN_INFO "Keystone driver unloaded\n");
}

module_init(keystone_init);
module_exit(keystone_exit);
```

---

## 🛠️ Step 4: Create the Makefile

Create a file called `Makefile`:

```make
obj-m += keystone-driver.o

all:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

---

## 🏗️ Step 5: Build the Kernel Module

```bash
make
```

### ✅ Expected Output

You should now see a file named:

```text
keystone-driver.ko
```

---

## 📥 Step 6: Install (Insert) the Module

```bash
sudo insmod keystone-driver.ko
```

### 🔍 Verify It's Loaded

```bash
lsmod | grep keystone_driver
```

---

## 🧪 Step 7: Test with dmesg

```bash
dmesg | tail -20
```

### Expected output:

```
Keystone driver loaded
```

---

## ❌ Step 8: Unload the Module

```bash
sudo rmmod keystone_driver
```

### Verify unload:

```bash
dmesg | tail -20
```

### Expected:

```
Keystone driver unloaded
```

---

## 🧹 Step 9: Clean Up

```bash
make clean
```

This deletes all generated files except the source code.

---

## 🧠 Notes

* This driver is for learning and demonstration purposes.
* You must recompile the module if you change kernel versions.
* No `/dev` entry is created, as this is not a character/block device driver.
* Module taint warnings are normal for out-of-tree modules.

---

## 📁 Project Structure (After Build)

```
keystone-driver/
├── keystone-driver.c          # Kernel driver source code
├── Makefile                   # Build script for the module
├── keystone-driver.ko         # Compiled kernel module
├── keystone-driver.o          # Object file
├── keystone-driver.mod.c      # Module metadata source
├── keystone-driver.mod.o      # Metadata object
├── modules.order              # Build order file
├── Module.symvers             # Symbols file
└── README.md                  # This file
```

---

## 📝 License

This project is licensed under the **GNU GPL v2 or later**.

---

## 👨‍💻 Author

**Developed by**: Your Name  
**Email**: your.email@example.com