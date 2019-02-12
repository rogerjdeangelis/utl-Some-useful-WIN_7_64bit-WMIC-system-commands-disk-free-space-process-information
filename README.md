# utl-Some-useful-WIN_7_64bit-WMIC-system-commands-disk-free-space-process-information
Some useful WIN_7_64bit WMIC system commands disk free space process information.

    Some useful WIN_7_64bit WMIC system commands disk free space process information

       Four Examples

           1. Info on all your disks "WMIC LOGICALDISK"
           2. Info on a Process like sas.exe "WMIC PROCESS WHERE Name=sas.exe"
           3. Info on your computer "WMIC COMPUTERSYSTEM LIST full"
           4. Info on your Plug and Play devices "WMIC PATH Win32_PnPdevice"

    github
    https://tinyurl.com/yx97j4xq
    https://github.com/rogerjdeangelis/utl-Some-useful-WIN_7_64bit-WMIC-system-commands-disk-free-space-process-information

    related repo
    https://tinyurl.com/y3na9jr6
    https://github.com/rogerjdeangelis/utl_how_to_check_available_disk_space_windows_unix

    Inspired by StackOverflow
    https://tinyurl.com/y6rdl6uq
    https://stackoverflow.com/questions/54645713/finding-pid-responsible-for-error-a-lock-is-not-available-for

    Useful site
    https://www.xorrior.com/wmic-the-enterprise/

    INPUT
    =====

      WMIC commands (Win 7 64bit)

        WMIC LOGICALDISK /PROCESS /COMPUTERSYSTEM /PATH


    PROCESS and OUTPUT
    ==================

    ---------------------------------------------
    1. Info on all your disks "WMIC LOGICALDISK"
    ---------------------------------------------

    filename diskdata pipe "wmic logicaldisk get name,size,freespace /format:csv";
    data diskdata;
        infile diskdata;
        input;
        if _n_ > 2;
        _infile_ = compress(_infile_, '0d'x);
        drive = scan(_infile_,3,",");
        size  = put(input(scan(_infile_,4,","),14.),comma19.);
        free  = put(input(scan(_infile_,2,","),14.),comma19.);
     run;quit;


    WORK.DISKDATA total obs=3  (disk in CD tray and external ESATA)

     DRIVE         SIZE               FREE

      C:      120,033,042,432     41,784,782,848
      D:      240,054,693,888    197,120,688,128
      E:      240,054,693,888    205,012,396,082


    --------------------------------------------------------------------
    2. Info on a Process like sas.exe "WMIC PROCESS WHERE Name=sas.exe"
    --------------------------------------------------------------------

    filename proces pipe 'WMIC PROCESS WHERE Name="sas.exe" LIST Brief /format:csv';
    data proces;
        infile proces;
        input;
        if _n_ > 2;
        *_infile_ = compress(_infile_, '0d'x);
        Node            = scan(_infile_,1,",");
        HandleCount     = scan(_infile_,2,",");
        Name            = scan(_infile_,3,",");
        Priority        = scan(_infile_,4,",");
        ProcessId       = scan(_infile_,5,",");
        ThreadCount     = scan(_infile_,6,",");
        WorkingSetSize  = scan(_infile_,7,",");
     run;quit;

    WORK.PROCES total obs=1

      NODE     HANDLECOUNT     NAME      PRIORITY    PROCESSID    THREADCOUNT    WORKINGSETSIZE

      E6420        322        sas.exe       8          1064           27           73478144


    ---------------------------------------------------------
    3. Info on your computer "WMIC COMPUTERSYSTEM LIST full"
    ---------------------------------------------------------

    filename putr pipe 'wmic computersystem LIST full';
    data putr;
        length putr $200;
        infile putr;
        input;
        putr = _infile_;
        if _n_ > 2;
     run;quit;


    WORK.PUTR total obs=56

       PUTR (Laptop)
       
      Manufacturer=Dell Inc.
      Model=Latitude E6420
      Name=E6420
      SystemType=x64-based PC
      TotalPhysicalMemory=17055105024
      UserName=E6420\backup
      Workgroup=BEASTS
      Domain=BEASTS
      WakeUpType=6
      BootROMSupported=TRUE
      BootupState=Normal boot
      Caption=E6420
      CurrentTimeZone=-300
      DaylightInEffect=FALSE
      Description=AT/AT COMPATIBLE
      EnableDaylightSavingsTime=TRUE
      NetworkServerModeEnabled=TRUE
      NumberOfProcessors=1
      PowerState=0
      PowerSupplyState=3
      PrimaryOwnerName=backup
      ResetCapability=1
      ResetCount=-1
      ResetLimit=-1
      Roles={"LM_Workstation","LM_Server","Print","NT","Potential_Browser","Master_Browser"}
      Status=OK
      ThermalState=3

    ------------------------------------------------------------------
    4. Info on your Plug and Play devices "WMIC PATH Win32_PnPdevice"
    ------------------------------------------------------------------


    filename proces pipe 'wmic path Win32_PnPdevice';
    data proces;
        length pnp $123;
        infile proces;
        input;
        if _n_ > 1;
        put _infile_;
        pnp=_infile_;
        keep pnp;
     run;quit;


    \\E6420\root\CIMV2:Win32_DesktopMonitor.DeviceID=DesktopMonitor1
    \\E6420\root\CIMV2:Win32_DesktopMonitor.DeviceID=DesktopMonitor2
    \\E6420\root\CIMV2:Win32_DesktopMonitor.DeviceID=DesktopMonitor3
    \\E6420\root\CIMV2:Win32_VideoController.DeviceID=VideoController1
    \\E6420\root\CIMV2:Win32_VideoController.DeviceID=VideoController2
    \\E6420\root\CIMV2:Win32_VideoController.DeviceID=VideoController3
    \\E6420\root\CIMV2:Win32_SoundDevice.DeviceID=HDAUDIO\FUNC_$1&VEN_1$DE&DEV_$$1C&SUBSYS
    \\E6420\root\CIMV2:Win32_SoundDevice.DeviceID=HDAUDIO\FUNC_$1&VEN_111D&DEV_7?E7&SUBSYS
    \\E6420\root\CIMV2:Win32_DiskDrive.DeviceID=\\.\PHYSICALDRIVE$
    \\E6420\root\CIMV2:Win32_DiskDrive.DeviceID=\\.\PHYSICALDRIVE1
    \\E6420\root\CIMV2:Win32_DiskDrive.DeviceID=\\.\PHYSICALDRIVE2
    \\E6420\root\CIMV2:Win32_MotherboardDevice.DeviceID=Motherboard
    \\E6420\root\CIMV2:Win32_ParallelPort.DeviceID=LPT1
    \\E6420\root\CIMV2:Win32_SCSIController.DeviceID=PCI\VEN_*$*?&DEV
    \\E6420\root\CIMV2:Win32_SCSIController.DeviceID=PCI\VEN_1217&DEV
    \\E6420\root\CIMV2:Win32_SCSIController.DeviceID=PCI\VEN_1217&DEV
    \\E6420\root\CIMV2:Win32_Keyboard.DeviceID=USB\VID_413C&PID_21$5\7&33FC?D4C&$&1
    \\E6420\root\CIMV2:Win32_Keyboard.DeviceID=HID\VID_$4?D&PID_C$*4&MI_$1&COL$1\9&9B4?AFB&$&$$$$
    \\E6420\root\CIMV2:Win32_Keyboard.DeviceID=HID\VID_$4?D&PID_C232\2&29C*$1E$&$&$$$$
    \\E6420\root\CIMV2:Win32_Keyboard.DeviceID=ACPI\PNP$3$3\4&1212F$?D&$
    \\E6420\root\CIMV2:Win32_USBController.DeviceID=PCI\VEN_*$*?&DEV_1C2?&SUBSYS
    \\E6420\root\CIMV2:Win32_USBController.DeviceID=PCI\VEN_*$*?&DEV_1C2D&SUBSYS
    \\E6420\root\CIMV2:Win32_PointingDevice.DeviceID=ACPI\DLL$493\4&1212F$?D&$
    \\E6420\root\CIMV2:Win32_PointingDevice.DeviceID=USB\VID_$4?D&PID_C$*4&MI_$$\*&25C*DE4A&$&$$$$
    \\E6420\root\CIMV2:Win32_PointingDevice.DeviceID=HID\VID_$4?D&PID_C231\2&E1$*A7?&$&$$$$
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\VID_$4?D&PID_C$*4\11*23?713933
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\VID_413C&PID_*194\?&?F$C4*D&$&?
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\VID_$A5C&PID_5*$$\$12345?7*9ABCD
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\ROOT_HUB2$\4&13AEED32&$
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\ROOT_HUB2$\4&1*BB?A7*&$
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\VID_*$*7&PID_$$24\5&1*B19DDB&$&1
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\VID_1BCF&PID_2B*3\?&?5?93*B&$&5
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\VID_*$*7&PID_$$24\5&3?A?529?&$&1
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\VID_413C&PID_2513\?&?5?93*B&$&1
    \\E6420\root\CIMV2:Win32_USBHub.DeviceID=USB\VID_413C&PID_2513\?&?5?93*B&$&2
    \\E6420\root\CIMV2:Win32_SerialPort.DeviceID=COM3
    \\E6420\root\CIMV2:Win32_SerialPort.DeviceID=COM1
    \\E6420\root\CIMV2:Win32_NetworkAdapter.DeviceID=$
    \\E6420\root\CIMV2:Win32_NetworkAdapter.DeviceID=1
    \\E6420\root\CIMV2:Win32_NetworkAdapter.DeviceID=2
    ...
    \\E6420\root\CIMV2:Win32_NetworkAdapter.DeviceID=27
    \\E6420\root\CIMV2:Win32_NetworkAdapter.DeviceID=2*
    \\E6420\root\CIMV2:Win32_POTSModem.DeviceID=USB\VID_413C&PID_*194&MI_$2
