# REGULAR-EXT C Device Interface
This document describes a set of data structures in the C programming language designed to allow the development of devices for REGULAR-EXT VMs that are compatible with all conformant implementations. Currently, the interface is provided on a source-level basis, meaning that no guaranteed ABI or means of dynamic loading exists. This document is currently in an unfinished, draft state.

## Device Lifecycle
REGULAR-EXT VMs consume devices in the form of the `rge_device` struct described below:

```C
typedef struct rge_device_s {
  void* (*attach)(rge_vm_interface* interface);
  void (*receive)(void* device, rge_vm_interface* interface, uint32_t data);
  void (*detach)(void* device, rge_vm_interface* interface);
} rge_device;
```

When a device is added to the system, the REGULAR-EXT implementation should allocate a device ID, then call the `attach` method of the corresponding `rge_device` struct. 

Once a device has been added to the system, whenever a `snd` instruction whose device ID matches that of the device is executed, the implementation should call the `recieve` method in the corresponding `rge_device` struct, passing the pointer returned by `attach` as `device`, and the value of the register specified by the third argument provided to the `snd` instruction as `data`.

When a device is being removed from the system, the REGULAR-EXT implementation should call the `detach` method in the corresponding `rge_device` struct, passing the pointer returned by that device's `attach` call as `device`. Once this call has been made, the implementation **must not** call `recieve` or `detach` with the same `device` pointer again.

## VM Interface
In order for devices to provide any useful functionality, they must be able to interact with VM resources in some manner. This is achieved by calling a set of functions that operate of the the `rge_vm_interface` struct. All REGULAR-EXT implementations must provide a set of functions and types matching the declarations provided below. Implementations *may* provide additional functions as well.

```C
#include <stddef.h>
#include <stdint.h>

struct rge_vm_interface_s;
typedef struct rge_vm_interface_s rge_vm_interface;

// Read words from VM memory. If srcAddr is not aligned to a word boundary, the result is implementation defined.
void vmiReadWords(rge_vm_interface* vm, uint32_t srcAddr, uint32_t* buffer, size_t words);

// Write words to VM memory. If dstAddr is not aligned to a word boundary, the result is implementation defined.
void vmiWriteWords(rge_vm_interface* vm, uint32_t dstAddr, uint32_t* data, size_t words);

// Read bytes from VM memory.
void vmiReadBytes(rge_vm_interface* vm, uint32_t srcAddr, uint8_t* buffer, size_t bytes);

// Write bytes to VM memory.
void vmiWriteBytes(rge_vm_interface* vm, uint32_t dstAddr, uint8_t* data, size_t bytes);

// Get the device ID of the calling device.
uint32_t vmiDeviceId(rge_vm_interface* vm);
```

