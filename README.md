# **DEPRECATION**
## This repository is being superseeded by `pyRemoteData`!

# rclonemountpy
This repository contains code for mounting and unmounting a remote with rclone and ssh.

**OBS**: Be aware that this script only works if you have set up SSH properly, and activated the `ssh-agent`. Please verify that you are able to connect to your remote before using the script (nothing bad will happen if you don't, it just won't work).

**TODO**: 
* Properly package this as a module, to massively decrease user friction.
* Test if it is possible to use the `RCLONE` `sftp-key-file` and `sftp-key-use-agent` arguments to remove the need for the `ssh-agent`.
* Create better default `RCLONE` options.

## Usage
Since this is not packaged as a proper module, it is a bit tricky to import it. There are two options:

### **Option 1**: The easy way

Clone this repository as a subdirectory of the directory where your python script is in. Then you can simply import the module as usual:
```py
from rclonemountpy.utils.mount import *

successful_unmount = unmount()
successful_mount = mount()

print("\n")
print("Unmount successful:", successful_unmount)
print("Mounting successful:", successful_mount)
```

### **Option 2**: the hard way

If you do not want to clone this repository as a subdirectory of the directory of your script, then you must hack around the relative path importing constraints of python:
```py
__package__ = "RELATIVE_PATH"
sys.path.append(os.path.abspath(__package__))

from rclonemountpy.utils.mount import *

successful_unmount = unmount()
successful_mount = mount()

print("\n")
print("Unmount successful:", successful_unmount)
print("Mounting successful:", successful_mount)
```
Here "RELATIVE_PATH" is the relative path to the parent directory of the `rclonemountpy` e.g. if you have the following structure:

>..\
>| some_directory\
>____| your_working_directory\
>________ your_script.py <-- YOU ARE HERE**\
>________ config.yaml <-- CONFIG LOCATION IF IMPORTING THE HARD WAY!\
>____| some_other_directory\
>| rclonemountpy\
>____ ..\
>____ config.yaml <-- DEFAULT CONFIG LOCATION

Then "RELATIVE_PATH" should be "../..". I think you may also be able to use absolute paths, but I haven't tested this.

**OBS**:
If you use the second option, then you must copy the config file as described above (into your working directory).


## Configuration
In the top-level of the `rclonemountpy` directory is a YAML-file (`config.yaml`) for configuration of `rclone mount`. You must change the name of the remote, to the one you use (as defined in `.ssh/config`). 

### `RCLONE` options
There are some defaults `rclone mount` options, but you should probably change these to better match your configuration. See [the `RCLONE` website](https://rclone.org/commands/rclone_mount/) for details.
