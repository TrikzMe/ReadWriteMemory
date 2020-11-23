# ReadWriteMemory
![PyPI - Status](https://img.shields.io/pypi/status/ReadWriteMemory)
![PyPI - Format](https://img.shields.io/pypi/format/ReadWriteMemory)
![GitHub](https://img.shields.io/github/license/vsantiago113/ReadWriteMemory)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/vsantiago113/ReadWriteMemory)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/ReadWriteMemory)

### Description
The ReadWriteMemory Class is made on Python for reading and writing to the memory of any process. This Class does not depend on any extra modules and only uses standard Python libraries like ctypes.

---

### [Documentation](https://vsantiago113.github.io/readwritememory.github.io/)

---

### Requirements
Python 3.4+<br />
OS: Windows 7, 8 and 10<br />

---

#### Windows API’s in this module:<br />
[EnumProcesses](https://docs.microsoft.com/en-us/windows/win32/api/psapi/nf-psapi-enumprocesses)<br />
[GetProcessImageFileName](https://docs.microsoft.com/en-us/windows/win32/api/psapi/nf-psapi-getprocessimagefilenamea)<br />
[OpenProcess](https://docs.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-openprocess)<br />
[Process Security and Access Rights](https://docs.microsoft.com/en-us/windows/win32/procthread/process-security-and-access-rights)<br />
[CloseHandle](https://docs.microsoft.com/en-us/windows/win32/api/handleapi/nf-handleapi-closehandle)<br />
[GetLastError](https://docs.microsoft.com/en-us/windows/win32/api/errhandlingapi/nf-errhandlingapi-getlasterror)<br />
[ReadProcessMemory](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-readprocessmemory)<br />
[WriteProcessMemory](https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-writeprocessmemory)<br />

---

## Usage

### Import and instantiate the Class
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()
```

### Get a Process by name
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
```

### Get a Process by ID
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_id(1337)
```

### Get the list of running processes ID's from the current system
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

processes_ids = rwm.enumerate_processes()
```

### Print the Process information
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
print(process.__dict__)
```

### Print the Process HELP docs
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
help(process)
```

### Exception: ReadWriteMemoryError
````python
from ReadWriteMemory import ReadWriteMemory
from ReadWriteMemory import ReadWriteMemoryError

rwm = ReadWriteMemory()
try:
    process = rwm.get_process_by_name('ac_client.exe')
except ReadWriteMemoryError as error:
    print(error)
````

### Open the Process
To be able to read or write to the process's memory first you need to call the open() method.
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
process.open()
```

### Set the pointers for example: to get health, ammo and grenades
The offsets must be a list in the correct order, if the address does not have any offsets then just pass the address. You need to pass two arguments, first the process address as hex and a list of offsets as hex.
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
process.open()

health_pointer = process.get_pointer(0x004e4dbc, offsets=[0xf4])
ammo_pointer = process.get_pointer(0x004df73c, offsets=[0x378, 0x14, 0x0])
grenade_pointer = process.get_pointer(0x004df73c, offsets=[0x35c, 0x14, 0x0])
```

### Read the values for the health, ammo and grenades from the Process's memory
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
process.open()

health_pointer = process.get_pointer(0x004e4dbc, offsets=[0xf4])
ammo_pointer = process.get_pointer(0x004df73c, offsets=[0x378, 0x14, 0x0])
grenade_pointer = process.get_pointer(0x004df73c, offsets=[0x35c, 0x14, 0x0])

health = process.read(health_pointer)
ammo = process.read(ammo_pointer)
grenade = process.read(grenade_pointer)
```

### Print the health, ammo and grenade values
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
process.open()

health_pointer = process.get_pointer(0x004e4dbc, offsets=[0xf4])
ammo_pointer = process.get_pointer(0x004df73c, offsets=[0x378, 0x14, 0x0])
grenade_pointer = process.get_pointer(0x004df73c, offsets=[0x35c, 0x14, 0x0])

health = process.read(health_pointer)
ammo = process.read(ammo_pointer)
grenade = process.read(grenade_pointer)

print({'Health': health, 'Ammo': ammo, 'Grenade': grenade})
```

### Write some random values for health, ammo and grenade to the Process's memory
```python
from ReadWriteMemory import ReadWriteMemory
from random import randint

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
process.open()

health_pointer = process.get_pointer(0x004e4dbc, offsets=[0xf4])
ammo_pointer = process.get_pointer(0x004df73c, offsets=[0x378, 0x14, 0x0])
grenade_pointer = process.get_pointer(0x004df73c, offsets=[0x35c, 0x14, 0x0])

process.write(health_pointer, randint(1, 100))
process.write(ammo_pointer, randint(1, 20))
process.write(grenade_pointer, randint(1, 5))
```

### Close the Process's handle when you are done using it.
```python
from ReadWriteMemory import ReadWriteMemory

rwm = ReadWriteMemory()

process = rwm.get_process_by_name('ac_client.exe')
process.open()

process.close()
```

### Add 2bytes support.
```python
# - Functions by my friend "L'In20Cible" to patch the c_uint to c_ushort in the original function
def ReadWithType(self, lp_base_address, value, type='c_uint'):
    """
    Read data from the process's memory.
    :param lp_base_address: The process's pointer
    :return: The data from the process's memory if succeed if not raises an exception.
    """
    try:
        read_buffer = ctypes.c_uint()
        lp_buffer = ctypes.byref(read_buffer)
        n_size = ctypes.sizeof(read_buffer)
        lp_number_of_bytes_read = ctypes.c_ulong(0)
        ctypes.windll.kernel32.ReadProcessMemory(self.handle, ctypes.c_void_p(lp_base_address), lp_buffer, n_size, lp_number_of_bytes_read)
        return read_buffer.value
    except (BufferError, ValueError, TypeError) as error:
        if self.handle:
            self.close()
        self.error_code = self.get_last_error()
        error = {'msg': str(error), 'Handle': self.handle, 'PID': self.pid,
                 'Name': self.name, 'ErrorCode': self.error_code}
        ReadWriteMemoryError(error)

def WriteWithType(self, lp_base_address, value, type='c_uint'):
    """
    Write data to the process's memory.
    :param lp_base_address: The process' pointer.
    :param value: The data to be written to the process's memory
    :return: It returns True if succeed if not it raises an exception.
    """
    try:
        write_buffer = getattr(ctypes, type)(value)
        lp_buffer = ctypes.byref(write_buffer)
        n_size = ctypes.sizeof(write_buffer)
        lp_number_of_bytes_written = ctypes.c_ulong(0)
        ctypes.windll.kernel32.WriteProcessMemory(self.handle, ctypes.c_void_p(lp_base_address), lp_buffer, n_size, lp_number_of_bytes_written)
        return True
    except (BufferError, ValueError, TypeError) as error:
        if self.handle:
            self.close()
        self.error_code = self.get_last_error()
        error = {'msg': str(error), 'Handle': self.handle, 'PID': self.pid,
                 'Name': self.name, 'ErrorCode': self.error_code}
        ReadWriteMemoryError(error)
```

### Add 64 bits addresses support.
```python
    def read64(self, lp_base_address: int) -> Any:
        """
        Read data from the process's memory.

        :param ctypes.c_void_p(lp_base_address): The process's pointer

        :return: The data from the process's memory if succeed if not raises an exception.
        """
        try:
            read_buffer = ctypes.c_uint()
            lp_buffer = ctypes.byref(read_buffer)
            n_size = ctypes.sizeof(read_buffer)
            lp_number_of_bytes_read = ctypes.c_ulonglong(0)
            ctypes.windll.kernel32.ReadProcessMemory(self.handle, ctypes.c_void_p(lp_base_address), lp_buffer,
                                                     n_size, lp_number_of_bytes_read)
            return read_buffer.value
        except (BufferError, ValueError, TypeError) as error:
            if self.handle:
                self.close()
            self.error_code = self.get_last_error()
            error = {'msg': str(error), 'Handle': self.handle, 'PID': self.pid,
                     'Name': self.name, 'ErrorCode': self.error_code}
            ReadWriteMemoryError(error)

    def write64(self, lp_base_address: int, value: int) -> bool:
        """
        Write data to the process's memory.

        :param ctypes.c_void_p(lp_base_address): The process' pointer.
        :param value: The data to be written to the process's memory

        :return: It returns True if succeed if not it raises an exception.
        """
        try:
            write_buffer = ctypes.c_uint(value)
            lp_buffer = ctypes.byref(write_buffer)
            n_size = ctypes.sizeof(write_buffer)
            lp_number_of_bytes_written = ctypes.c_ulonglong(0)
            ctypes.windll.kernel32.WriteProcessMemory(self.handle, ctypes.c_void_p(lp_base_address), lp_buffer,
                                                      n_size, lp_number_of_bytes_written)
            return True
        except (BufferError, ValueError, TypeError) as error:
            if self.handle:
                self.close()
            self.error_code = self.get_last_error()
            error = {'msg': str(error), 'Handle': self.handle, 'PID': self.pid,
                     'Name': self.name, 'ErrorCode': self.error_code}
            ReadWriteMemoryError(error)

```

### Examples
Check out the code inside the Test folder on the python file named testing_script.py.
The AssaultCube game used for this test is version v1.1.0.4 If you use a different version then you will have to use CheatEngine to find the memory addresses.
[https://github.com/assaultcube/AC/releases/tag/v1.1.0.4](https://github.com/assaultcube/AC/releases/tag/v1.1.0.4)<br />
For more examples check out the AssaultCube game trainer:
[https://github.com/vsantiago113/ACTrainer](https://github.com/vsantiago113/ACTrainer)
```python
from ReadWriteMemory import ReadWriteMemory
from random import randint

rwm = ReadWriteMemory()
process = rwm.get_process_by_name('ac_client.exe')
process.open()

print('\nPrint the Process information.')
print(process.__dict__)

health_pointer = process.get_pointer(0x004e4dbc, offsets=[0xf4])
ammo_pointer = process.get_pointer(0x004df73c, offsets=[0x378, 0x14, 0x0])
grenade_pointer = process.get_pointer(0x004df73c, offsets=[0x35c, 0x14, 0x0])
print(health_pointer)

health = process.read(health_pointer)
ammo = process.read(ammo_pointer)
grenade = process.read(grenade_pointer)

print('\nPrinting the current values.')
print({'Health': health, 'Ammo': ammo, 'Grenade': grenade})

process.write(health_pointer, randint(1, 100))
process.write(ammo_pointer, randint(1, 20))
process.write(grenade_pointer, randint(1, 5))

health = process.read(health_pointer)
ammo = process.read(ammo_pointer)
grenade = process.read(grenade_pointer)

print('\nPrinting the new modified random values.')
print({'Health': health, 'Ammo': ammo, 'Grenade': grenade})

process.close()

```
