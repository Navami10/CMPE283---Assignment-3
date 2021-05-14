# CMPE283-Assignment-3

# Group -

Devki Sanat Desai - 015328339

Navami Murthy Sanjaynagar - 015278172

# Assignment description:Instrumentation-through-Hypercall

Your assignment is to modify the CPUID emulation code in KVM to report back additional information when special CPUID leaf nodes are requested.

For CPUID leaf node %eax=0x4FFFFFFE:

Return the number of exits for the exit number provided (on input) in %ecx.

This value should be returned in %eax

For leaf nodes 0x4FFFFFFE, if %ecx (on input) contains a value not defined by the SDM, return 0 in all %eax, %ebx, %ecx registers and return 0xFFFFFFFF in %edx. For exit types not enabled in KVM, return 0s in all four registers.

# Devki Sanat Desai:

- Compiled the code after making the necessary changes to it.

- Debugged complilation errors.

- Made required changes to the cpuid.c and vmx.c files.

- Documentation.

# Navami Murthy Sanjaynagar:

- Made necessary changes in cpuid.c and vmx.c files.

- Researched how to perform testing on kernel code.

- Wrote the test program and shell script to execute cpuid command on all the exit numbers


# Steps followed for this assignment:

The environment is set up as per Assignment 2

1 - We changed the kernel code as per the requirement of Assignment 3. The cpuid.c and vmx.c files were modified.

2 - After file modification, we built the kernel.

3 - We built the KVM module only, since the changes made were all related to the KVM only, using the below command - 

sudo make -j 8 modules M=arch/x86/kvm

4 - First, we removed the older modules using the commands below -

sudo rmmod kvm_intel

sudo rmmod kvm

5 - After this, we inserted the newly built modules using the below commands - 

sudo insmod arch/x86/kvm/kvm.ko

sudo insmod arch/x86/kvm/kvm-intel.ko

6 - Next, we reboot the inner vm. The rebooting took the updated changes only and thus the reboot was successful.

7 - Once the innerVM rebooted, we ensured that the cpuid is installed in the inner VM and ran the test script.

8 - We gave various exit numbers and checked if the desired output is obtained.

9 - We wrote a shell script which would run for all the exit numbers and captured the output. The output of shell script can be seen in the "Output" Folder.

# Questions:

Q1. Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?

- The frequency of exits i.e the number of exits does not increase at a stable rate. During certain vm operations more exits are performed. For example in our system, for exit no. 48 the exits performed was the highest. i.e 0x0005b605 in hexadecimal which corresponds to 374277 exits in decimal value. A full vm reboot in our system took approximately around 120,000 exits. This can be observed from the attached screenshot as well.


Q2. Of the exit types defined in the SDM, which are the most frequent? Least?

- According to the output:

The most frequent were 

#### Exit number 48 - EPT Violation
#### Exit Number 7 - Interrupt window

The least frequest were

#### Exit number 29 - MOV DR
#### Exit number 54 - WBINVD
#### Exit number 55 - XSETBV
