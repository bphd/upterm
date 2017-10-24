# Building and working on Windows

To build and work with Upterm on Windows requires a small, but significant development environment.

## Setting up the build environment

### Windows

For the purposes of development, I strongly encourage a VM to ensure that your development stays contained and doesn't clutter up your host Windows install (if you have one). Obviously, if you do not run Windows as a host OS, a VM is the only option.

You can use a Windows VM running the latest Windows 10, installed from an ISO creted with the official [Media Creation Tool](https://www.microsoft.com/en-ca/software-download/windows10). VirtualBox (on all supported platforms) and Hyper-V will both work as hypervisors to support the guest development VM if you do not want to install the following software sets into your main OS. If you use a VM, you will require approximately 35-40GB of disk space for everything involved.

Note that you can run this Windows 10 VM indefinitely without activation, and **you do not need to purchase an additional Windows license for this development environment**.

In theory, this could be done inside of a microsoft/nanoserver Docker container, however I have not tried it and testing would require to pack and export the builds every time which makes debugging trickier (but this could be useful for a CI/CD pipeline).

### Visual Studio 2017

- Community edition will suffice. Install the **Desktop development with C++** workload, and I also installed the **Python development**, **Node.js development**, and **Linux development with C++** workloads. The latter three are likely optional, but will ensure that you get some other tooling that may be useful.
- Under _Individual components_, check the **VC++ 2015.3 v140 toolset for desktop (x86,x64)** component, which provides the mechanism for working with the necessary VC++ runtimes for the C/C++ code.
- Additionally, make sure that *Git for Windows* is selected in the Individual components tab, as that will make your life a fair bit easier.

### Python

- While we installed Python with Visual Studio, I recommend installing the offiical latest Windows x64 Python 2.7.x build from [python.org](https://www.python.org/downloads/windows/). I have used Pyhton 2.7.14, but anything newer than 2.7.12 should work.
- While installing, make _sure_ that you choose the option to include the Python target directory in the system PATH. This will ensure that the necessary commands are available from the command windows we'll be using.

### Node.js

- While we installed Node.js with Visual Studio, I recommend installing the official latest Windows x64 8.7.x build from [nodejs.org](https://nodejs.org/en/download/). I have used 8.7.0, and I have no intuition on how picky Upterm will be to this version.
- While installing, make _sure_ that you choose the option to include the Node.js target directory in the system PATH. This will ensure that the necessary commands are available from the command windows we'll be using.
- After the installation has finished, open an elevated Powershell prompt, and run the following commands:
  - `npm config set msvs_version 2015` (To match the additional VC++ toolkit we installed)
  - `npm config set optional false` (To work around a bug in fsevents and npm)

## Building and running

This part is pretty easy.

- Clone the Github repo to anywhere you like using the `git` command that Visual Studio kindly installed into the path for us.
- From the start menu, open the **Developer Command Prompt for VS 2017**, and all following commands will be run from within this terminal window.
  - If you prefer Powershell to cmd (which you probably do), you can type `powershell` in this prompt to launch and drop into a Powershell interpreter which will offer better tab completion (among other things).
- Change into the directory you cloned the repo into.
- `npm install` isn't strictly necessary, but helps you identify any missing install issues.
- `npm start` will build everything, and launch Upterm upon successful build completion.
- `npm run pack` will fetch a few other dependencies and package the build into a standalone redistributable contained in the `dist/` folder in the repo root.
  - Note that you will need to run `npm install 7zip-bin-win` before this command will succeed.
  - You can then copy the setup executable to handle installing (without administrative privileges, nicely), or copy the `win-unpacked` directory, and running directly from there.
  