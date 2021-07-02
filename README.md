# How-to-make-a-WinPE-Media
# Step 1: Create working files
1.you shoule download the ADK from the MS website.(Download the Windows ADK and Download the WinPE add-on for the Windows ADK)
https://docs.microsoft.com/en-us/windows-hardware/get-started/adk-install

2.Insatll the ADK and the WinPE adk.you can find the files in C:\Program Files (x86)\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment

Start the Deployment and Imaging Tools Environment as an administrator.

Run copype to create a working copy of the Windows PE files. For more information about copype, see Copype command line options.(copype amd64 C:\WinPE_amd64)

# Step 2: Customize WinPE (Usually not needed)
Note, when you add more packages to WinPE, it slows WinPE performance and boot time. Only add additional packages when necessary.
Common customizations

Add an update. If you're going to be capturing an FFU at the end of the lab, apply KB4048955 to your WinPE image. To learn more, see: WinPE: mount and customize.

Add a video or network driver. (WinPE includes generic video and network drivers, but in some cases, additional drivers are needed to show the screen or connect to the network.). To learn more, see WinPE: Add drivers.

Add PowerShell scripting support. To learn more, see WinPE: Adding Windows PowerShell support to Windows PE. PowerShell scripts are not included in this lab.

Set the power scheme to high-performance. Speeds deployment. Note, our sample deployment scripts already set this scheme automatically. See WinPE: Mount and Customize: High Performance.

Optimize WinPE: Recommended for devices with limited RAM and storage (for example, 1GB RAM/16GB storage). After you add drivers or other customizations to Windows PE, see WinPE: Optimize and shrink the image to help reduce the boot time.
