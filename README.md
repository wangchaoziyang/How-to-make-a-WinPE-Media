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

# Step 3: Create bootable media

Now that you now have a set of working files, you can use MakeWinPEMedia to build bootable WinPE media.

# Create a bootable WinPE USB drive

1.Attach a USB drive to your technician PC.

2.Start the Deployment and Imaging Tools Environment as an administrator.

3.Optional You can format your USB key prior to running MakeWinPEMedia. MakeWinPEMedia will format your WinPE drive as FAT32. If you want to be able to store files larger than 4GB on your WinPE USB drive, you can create a multipartition USB drive that has an additional partition formatted as NTFS. See Create a multipartition USB drive for instructions.

4.Use MakeWinPEMedia with the /UFD option to format and install Windows PE to the USB flash drive, specifying the USB key's drive letter:
MakeWinPEMedia /UFD C:\WinPE_amd64 P:

# Create a WinPE ISO, DVD, or CD

1.Use MakeWinPEMedia with the /ISO option to create an ISO file containing the Windows PE files:
MakeWinPEMedia /ISO C:\WinPE_amd64 C:\WinPE_amd64\WinPE_amd64.iso

2.Optional Burn a DVD or CD: In Windows Explorer, right-click the ISO file, and select Burn disc image > Burn, and follow the prompt

# DISM 解压WIM文件

Dism /Mount-Image /ImageFile:"C:\WinPE_amd64\media\sources\boot.wim" /index:1 /MountDir:"C:\WinPE_amd64\mount"

# 新增裝置驅動程式 (.inf 檔案)

Dism /Add-Driver /Image:"C:\WinPE_amd64\mount" /Driver:"C:\SampleDriver\driver.inf"

# 新增套件/語言/選用元件/.cab 檔案

Dism /Add-Package /Image:"C:\WinPE_amd64\mount" /PackagePath:"C:\Program Files\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\WinPE-HTA.cab"  

Dism /Add-Package /Image:"C:\WinPE_amd64\mount" /PackagePath:"C:\Program Files\Windows Kits\10\Assessment and Deployment Kit\Windows Preinstallation Environment\amd64\WinPE_OCs\en-us\WinPE-HTA_en-us.cab"

# 新增檔案和資料夾

將檔案和資料夾複製到 C:\WinPE_amd64\mount 資料夾。 這些檔案會顯示在 WinPE 的 X:\ 資料夾中。（請勿新增太多檔案，因為這些檔案會使 WinPE 速度變慢，而且可能佔滿預設 RAMDisk 環境中的可用記憶體。）

# 新增啟動指令碼

修改 Startnet.cmd 以包含您的自訂命令。 這個檔案位於您掛接的映像 (位於 C:\WinPE_amd64\mount\Windows\System32\Startnet.cmd)。

您也可以從這個檔案呼叫其他批次檔或命令列指令碼。

# 新增應用程式

1.在掛接的 WinPE 映像中建立應用程式目錄。（md "C:\WinPE_amd64\mount\windows\<MyApp>"）

2.將必要的應用程式檔案複製到本機 WinPE 目錄。（Xcopy C:\<MyApp> "C:\WinPE_amd64\mount\windows\<MyApp>"）

3.稍後再從 X：目錄啟動 WinPE 並執行應用程式，以測試應用程式。（X:\Windows\System32> X:\Windows\<MyApp>）

ps:如果您的應用程式需要暫存儲存空間，或當 WinPE 執行應用程式時沒有回應，您可能需要增加配置給 WinPE 的暫存儲存空間 (臨時空間) 數量。

4.若要在 WinPE 啟動時自動啟動執行命令介面或應用程式，請將路徑位置新增至 Winpeshl.ini 檔案。



