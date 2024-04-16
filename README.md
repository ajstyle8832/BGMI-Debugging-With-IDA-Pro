# IDA-Pro-BGMI-Debugging

This README details how to connect IDA Pro 7.7 dbgsrv for debugging an emulator game like BGMI (Battlegrounds Mobile India) and provides a Python script to help make the debugging process undetectable.

## Getting Started

These instructions will guide you through the setup and execution of a remote debugging session using IDA Pro 7.7 on a game running inside an emulator.

### Prerequisites

- IDA Pro 7.7 with dbgsrv
- Android emulator running BGMI
- ADB (Android Debug Bridge) setup on your computer

### Connecting IDA Pro 7.7 dbgsrv for Debugging

1. **Start the Emulator & Launch BGMI:**
   - Ensure that the BGMI game is properly installed and running on your emulator.

2. **Configure IDA Pro:**
   - Open IDA Pro and navigate to `Debugger` in the menu.
   - Select `Remote Linux GDB Server`.
   - Enter the IP address of your computer and the port number `23946`.

3. **Start the dbgsrv Server:**
   - Click on `Start Server` in IDA Pro to initiate the dbgsrv server.

4. **Forward Debugging Traffic:**
   - On your computer, open a command prompt or terminal.
   - Execute the command: `adb forward tcp:23946 tcp:23946`
   - This command forwards the debugging traffic from the emulator to your computer.

5. **Attach IDA Pro to the BGMI Process:**
   - In IDA Pro, open the APK file of BGMI.
   - Navigate to `Debugger` and select `Attach to process`.
   - To find the process of the game, run the following command in the emulator: `ps | grep <game_name>`.
   - Select the process from the list in IDA Pro to attach to it.

6. **Debugging:**
   - Once attached, you can set breakpoints, inspect memory, step through the code, and more.

### ID Script for Making Debugging Undetectable

The following Python script for IDA Pro helps in making the debugging session undetectable:

```python
import idautils
import idc

# Set the name of the game process here
game_process = "com.pubg.imobile"

# Set the name of the function you want to debug here
function_name = "main"

# Start the remote debugger
idc.StartDebugServer(23946, "localhost")

# Wait for the game process to start
while True:
    processes = idautils.Processes(True)
    if game_process in processes:
        break

# Attach to the game process
idc.AttachProcess(processes.index(game_process))

# Find the address of the function you want to debug
func_addr = idc.GetFunctionAddress(function_name)

# Set a breakpoint at the function
idc.AddBpt(func_addr)

# Wait for the breakpoint to be hit
while True:
    idc.Wait()
    if idc.BptTouched():
        break

# Inspect memory, step through the code, etc.

# Detach from the game process
idc.DetachProcess()

# Stop the remote debugger
idc.StopDebugServer()
```

### Usage

- Save the above script as a `.py` file.
- Run it within IDA Pro to execute the undetectable debugging session.

### Conclusion

By following these steps, you can effectively debug BGMI using IDA Pro 7.7, allowing for detailed inspection and modification of game behavior. The provided script helps in maintaining a low profile during the debugging process.
```

This README provides comprehensive instructions for setting up and running a remote debugger using IDA Pro on an Android emulator game, including a Python script designed to make the debugging process discreet.
