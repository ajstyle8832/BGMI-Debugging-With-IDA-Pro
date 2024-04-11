# IDA-Pro-BGMI-Debugging
Connecting IDA Pro 7.7 dbgsrv inside an emulator game like BGMI for debugging
## Here's an example of how to connect IDA Pro 7.7 dbgsrv inside an emulator game like BGMI for debugging and the ID script for making it undetectable:
## Connecting IDA Pro 7.7 dbgsrv inside an emulator game like BGMI for debugging

To connect IDA Pro 7.7 dbgsrv inside an emulator game like BGMI for debugging, follow these steps:

    Start the emulator and launch the BGMI game.
    Open IDA Pro and select "Debugger" from the menu.
    Choose "Remote Linux GDB Server" and enter the IP address of your computer and the port number 23946.
    Click "Start Server" to start the dbgsrv server.
    In the emulator, open a command prompt and type the following command: adb forward tcp:23946 tcp:23946. This will forward the debugging traffic from the emulator to your computer.
    In IDA Pro, open the APK file of the game and select "Debugger" from the menu.
    Choose "Attach to process" and select the process of the game. You can find the process name by running the following command in the emulator: ps | grep <game_name>.
    Once you have attached the debugger to the process, you can set breakpoints, inspect memory, and step through the code.

ID Script for Making it Undetectable

To make the debugging process undetectable, you can use the following ID script:

Python code:

import idautils

import idc


-- Set the name of the game process here

game_process = "com.pubg.imobile"


-- Set the name of the function you want to debug here

function_name = "main"


-- Start the remote debugger

idc.StartDebugServer(23946, "localhost")


-- Wait for the game process to start

while True:

    processes = idautils.Processes(True)

    if game_process in processes:

        break


-- Attach to the game process

idc.AttachProcess(processes.index(game_process))


-- Find the address of the function you want to debug

func_addr = idc.GetFunctionAddress(function_name)


-- Set a breakpoint at the function

idc.AddBpt(func_addr)


-- Wait for the breakpoint to be hit

while True:

    idc.Wait()

    if idc.BptTouched():

        break


-- Inspect memory, step through the code, etc.


-- Detach from the game process

idc.DetachProcess()


-- Stop the remote debugger

idc.StopDebugServer()

You can save this script as a .py file and run it in IDA Pro to make the debugging process undetectable.
