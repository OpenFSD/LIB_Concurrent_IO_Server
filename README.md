# Avril - LIBRARY Template Of Concurrent Input Output Server For Full Stack Development.
## Concurrent Server Boxed with Inputs and Outputs, Launchs a do thread for each praise event id.

### C# .NET Console.

### Using Windows 11 Home.
Edition: Windows 11 Home OEM System Builder

Version: 24H2

### Using Microsoft Visual Studio Professional 2022.

Version 17.13.4.
 

## Dependencies.
### WriteEnableForThreadsAtStacks.
 - https://github.com/OpenFSD/LIB_WriteEnableForThreadsAt_STACK
#### Similar Repositiry.
 - https://github.com/cameron314/readerwriterqueue
   
### LaunchEnableForConcurrentThreadsAtEnds
 - https://github.com/OpenFSD/LIB_LaunchEnableForConcurrentThreadsAt_END
#### Similar Repositiry.
 - https://github.com/cameron314/concurrentqueue

## Using
### C#
#### IMPORT_LIB_Server_IO_Concurrnecy.cs
````
﻿using System;
using System.Runtime.InteropServices;
using System.Security;

namespace Avril_FSD
{
    [SuppressUnmanagedCodeSecurity]
    public static class Library_For_Server_Concurrency
    {
        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Initialise_Server_Concurrency@CLIBServerIOConcurrnecy@Avril_FSD@@SAPAXXZ")]
        public static extern IntPtr Initialise_Server_Concurrency();

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Initalise_Programs@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@@Z")]
        public static extern void Initialise_Programs(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Flip_InBufferToWrite@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@@Z")]
        public static extern void Flip_InBufferToWrite(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Flip_OutBufferToWrite@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@@Z")]
        public static extern void Flip_OutBufferToWrite(IntPtr obj);


        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = " ?Get_flag_isNewInputDataReady@CLIBServerIOConcurrnecy@Avril_FSD@@SA_NPAVFramework_Server@2@@Z")]
        public static extern bool Get_flag_isNewInputDataReady(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_flag_isNewOutputDataReady@CLIBServerIOConcurrnecy@Avril_FSD@@SA_NPAVFramework_Server@2@@Z")]
        public static extern bool Get_flag_isNewOutputDataReady(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_flag_IsStackLoaded_Server_InputAction@CLIBServerIOConcurrnecy@Avril_FSD@@SA_NPAVFramework_Server@2@@Z")]
        public static extern bool Get_flag_IsStackLoaded_Server_InputAction(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_flag_IsStackLoaded_Server_OutputRecieve@CLIBServerIOConcurrnecy@Avril_FSD@@SA_NPAVFramework_Server@2@@Z")]
        public static extern bool Get_flag_IsStackLoaded_Server_OutputRecieve(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_flag_IsInitialised_Avril_FSD_ServerConcurrency@CLIBServerIOConcurrnecy@Avril_FSD@@SA_NPAVFramework_Server@2@@Z")]
        public static extern bool Get_flag_Avril_FSD_ServerConcurrency_Initialised(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_program_WriteEnableStack_ServerInputAction@CLIBServerIOConcurrnecy@Avril_FSD@@SAPAXXZ")]
        public static extern IntPtr Get_program_WriteEnableStack_ServerInputAction();

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_program_WriteEnableStack_ServerOutputRecieve@CLIBServerIOConcurrnecy@Avril_FSD@@SAPAXXZ")]
        public static extern IntPtr Get_program_WriteEnableStack_ServerOutputRecieve();

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Pop_Stack_Output@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@@Z")]
        public static extern void Pop_Stack_Output(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Push_Stack_InputPraises@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@@Z")]
        public static extern void Push_Stack_InputPraises(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Select_Set_Intput_Subset@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@D@Z")]
        public static extern void Select_Set_Intput_Subset(IntPtr obj, byte praiseEventId);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_flag_IsNewInputDataReady@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@_N@Z")]
        public static extern void Set_flag_IsNewInputDataReady(IntPtr obj, bool value);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_flag_IsNewOutputDataReady@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@_N@Z")]
        public static extern void Set_flag_IsNewOutputDataReady(IntPtr obj, bool value);

        // Praise Event Id
        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_PraiseEventId@CLIBServerIOConcurrnecy@Avril_FSD@@SADPAVFramework_Server@2@@Z")]
        public static extern byte Get_PraiseEventId(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_PraiseEventId@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@D@Z")]
        public static extern void Set_PraiseEventId(IntPtr obj, byte value);
    }

// Praise 0 Data
    [SuppressUnmanagedCodeSecurity]
    internal static class Library_For_Praise_0_Events
    {
        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_Praise0_Input_IsPingActive@CLIBServerIOConcurrnecy@Avril_FSD@@SA_NPAVFramework_Server@2@@Z")]
        public static extern bool Get_Praise0_Input_IsPingActive(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_Praise0_Input_IsPingActive@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@_N@Z")]
        public static extern void Set_Praise0_Input_IsPingActive(IntPtr obj, bool value);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_Praise0_Output_IsPingActive@CLIBServerIOConcurrnecy@Avril_FSD@@SA_NPAVFramework_Server@2@@Z")]
        public static extern void Get_Praise0_Output_IsPingActive(IntPtr obj);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_Praise0_Output_IsPingActive@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@_N@Z")]
        public static extern void Set_Praise0_Output_IsPingActive(IntPtr obj, bool value);
    }
// Praise 1 Data
    [SuppressUnmanagedCodeSecurity]
    internal static class Library_For_Praise_1_Events
    {
        // Praise 1 Data
        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_Praise1_Input_mouseDelta_X@CLIBServerIOConcurrnecy@Avril_FSD@@SAMPAVFramework_Server@2@@Z")]
        public static extern float Get_Praise1_Input_mouseDelta_X();
        
        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_Praise1_Input_mouseDelta_Y@CLIBServerIOConcurrnecy@Avril_FSD@@SAMPAVFramework_Server@2@@Z")]
        public static extern float Get_Praise1_Input_mouseDelta_Y();

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_Praise1_Input_mouseDelta_X@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@M@Z")]
        public static extern void Set_Praise1_Input_mouseDelta_X(float value);
        
        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_Praise1_Input_mouseDelta_Y@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@M@Z")]
        public static extern void Set_Praise1_Input_mouseDelta_Y(float value);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_Praise1_Output_Player_Fowards@CLIBServerIOConcurrnecy@Avril_FSD@@SA?AV?$Matrix@N$02$00$0A@$02$00@Eigen@@PAVFramework_Server@2@@Z")]
        public static extern float[] Get_Praise1_Output_Player_Fowards();

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_Praise1_Output_Player_Up@CLIBServerIOConcurrnecy@Avril_FSD@@SA?AV?$Matrix@N$02$00$0A@$02$00@Eigen@@PAVFramework_Server@2@@Z")]
        public static extern float[] Get_Praise1_Output_Player_Up();

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Get_Praise1_Output_Player_Right@CLIBServerIOConcurrnecy@Avril_FSD@@SA?AV?$Matrix@N$02$00$0A@$02$00@Eigen@@PAVFramework_Server@2@@Z")]
        public static extern float[] Get_Praise1_Output_Player_Right();

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_Praise1_Output_Player_Fowards@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@V?$Matrix@N$02$00$0A@$02$00@Eigen@@@Z")]
        public static extern void Set_Praise1_Output_Player_Fowards(float[] value);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_Praise1_Output_Player_Up@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@V?$Matrix@N$02$00$0A@$02$00@Eigen@@@Z")]
        public static extern void Set_Praise1_Output_Player_Up(float[] value);

        [DllImport("LIBServerIOConcurrnecy.dll", EntryPoint = "?Set_Praise1_Output_Player_Right@CLIBServerIOConcurrnecy@Avril_FSD@@SAXPAVFramework_Server@2@V?$Matrix@N$02$00$0A@$02$00@Eigen@@@Z/")]
        public static extern void Set_Praise1_Output_Player_Right(float[] value);

    }
    // Praise 2 Data
    [SuppressUnmanagedCodeSecurity]
    internal static class Library_For_Praise_2_Events
    {

    }

}
````
