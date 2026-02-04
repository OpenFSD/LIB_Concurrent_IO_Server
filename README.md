# LIBRARY Template For Avril Open Concurrent IO Server In Full Stack Development Apps.
## Concurrent Server Boxed with Inputs and Outputs, Launchs a do thread for each praise.

### C# .NET Console.

### Using Windows 11 Home.
Edition: Windows 11 Home OEM System Builder

Version: 24H2

### Using Microsoft Visual Studio Professional 2022.

Version 17.13.4.
 
## Dependencies.
### WriteEnableForThreadsAtStacks.
 - https://github.com/AvrilOpenCFSD/LIB_WriteEnableForThreadsAt_STACK
#### Similar Repositiry.
 - https://github.com/cameron314/readerwriterqueue
   
### LaunchEnableForConcurrentThreadsAtEnds
 - https://github.com/OpenFSD/LIB_LaunchEnableForConcurrentThreadsAt_END
#### Similar Repositiry.
 - https://github.com/cameron314/concurrentqueue

## Implementation
<img width="1550" height="829" alt="flow chart" src="https://github.com/user-attachments/assets/c1bcd56a-79b0-402c-9cae-2c6c3caa3421" />

### Server Shell App.

#### Execute Server.
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ServerAssembly/engine/Execute.cs
````
namespace Avril_FSD.ServerAssembly
{
    public class Execute
    {
.
.
.
        public void Initialise_Libraries(Avril_FSD.ServerAssembly.Framework_Server obj)
        {
            obj.Get_server().Get_execute().Set_program_ServerConcurrency(Avril_FSD.Library_For_Server_Concurrency.Initialise_Server_Concurrency());
            System.Console.WriteLine("created server concurrency.");//TESTBENCH
        }

        public void Initialise_NetworkingPipes(Avril_FSD.ServerAssembly.Framework_Server obj)
        {
            obj.Get_server().Get_execute().Set_networking_Server(new Avril_FSD.ServerAssembly.Networking_Server());
            obj.Get_server().Get_execute().Get_networking_Server().Initialise_networking_Server();
        }

        public void Initialise_Threads(Avril_FSD.ServerAssembly.Framework_Server obj)
        {
            byte threadIdCounter = 0;
            obj.Get_server().Get_execute().Set_thread(threadIdCounter, Thread.CurrentThread);
            
            threadIdCounter++;
            obj.Get_server().Get_execute().Set_thread(threadIdCounter, new Thread(() => obj.Get_server().Get_execute().Get_networking_Server().Thread_IO_Server(threadIdCounter)));
            obj.Get_server().Get_execute().Get_thread(threadIdCounter).Start();
            System.Console.WriteLine("starting = > Thread_IO_Server on core " + (threadIdCounter).ToString());//TESTBENCH
        }

        public void Create_And_Run_Graphics(Avril_FSD.ServerAssembly.Framework_Server obj)
        {
            System.Console.WriteLine("starting = > gameInstance");//TESTBENCH
            using (Avril_FSD.ServerAssembly.Game_Instance gameInstance = new Avril_FSD.ServerAssembly.Game_Instance())
            {
                gameInstance.Run(obj.Get_server().Get_data().Get_settings().Get_refreshRate());
            }
        }
.
.
.
	}
}
````

#### Loading Server Shell Framework.
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ServerAssembly/gameInstance/Game_Instance.cs
````
namespace Avril_FSD.ServerAssembly
{
    public sealed class Game_Instance : GameWindow
    {
.
.
.
        protected override void OnLoad(EventArgs e)
        {
.
.
.
            Avril_FSD.ServerAssembly.Framework_Server obj = Avril_FSD.ServerAssembly.Program.Get_framework_Server();
            obj.Get_server().Get_execute().Initialise_Threads(obj);

            System.Console.WriteLine("OnLoad .. done");
        }

.
.
.
        protected override void OnUpdateFrame(FrameEventArgs e)
        {
            _time += e.Time;
            foreach (var item in _gameObjects)
            {
                item.Update(_time, e.Time);
            }
            if(Avril_FSD.ServerAssembly.Program.Get_framework_Server().Get_server().Get_execute().Get_execute_Control().Get_exitApplication() == false)
            {
                HandleKeyboard(e.Time);
                HandleMouse();
                switch (cameraSelector)
                {
                    case false:
                        Get_gameObjectFactory().Get_player().Get_CameraFP().Update(_time, e.Time);
                        break;

                    case true:
                        Get_gameObjectFactory().Get_player().Get_CameraTP().Update(_time, e.Time);
                        break;
                }
            }
        }
        private void HandleMouse()
        {
            //Console.WriteLine("TESTBENCH => HandleMouse");
            MouseState mouseState = Mouse.GetCursorState();
        }
        private void HandleKeyboard(double dt)
        {
            var keyState = Keyboard.GetState();

            if (keyState.IsKeyDown(Key.Escape))
            {
                Exit();
            }
            if (keyState.IsKeyDown(Key.W))
            {
                
            }
            if (keyState.IsKeyDown(Key.S))
            {
                
            }

            if (keyState.IsKeyDown(Key.A))
            {
                
            }
            if (keyState.IsKeyDown(Key.D))
            {
                
            }
            _lastKeyboardState = keyState;
        }
.
.
.
    }
}
````

### Client Shell App.

### Execute Client.
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ClientAssembly/engine/Execute.cs
````
namespace Avril_FSD.ClientAssembly
{
    public class Execute
    {
.
.
.
        public void Initialise_NetworkingPipes(Avril_FSD.ClientAssembly.Framework_Client obj)
        {
            Set_networking_Client(new Avril_FSD.ClientAssembly.Networking_Client());
            Get_networking_Client().Initialise_networking_Client();
        }

        public void Initialise_Libraries()
        {
            programId_ConcurrentQue_C = Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Initialise_LaunchEnableForConcurrentThreadsAt();
            System.Console.WriteLine("created Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT");

            programId_WriteQue_C_IA = Avril_FSD.Library_For_WriteEnableForThreadsAt_CLIENTINPUTACTION.Initialise_WriteEnable();
            System.Console.WriteLine("created Library_For_WriteEnableForThreadsAt_CLIENTINPUTACTION");

            programId_WriteQue_C_OR = Avril_FSD.Library_For_WriteEnableForThreadsAt_CLIENTOUTPUTRECIEVE.Initialise_WriteEnable();
            System.Console.WriteLine("created Library_For_WriteEnableForThreadsAt_CLIENTOUTPUTRECIEVE");

        }
        public void Initialise_Threads(Avril_FSD.ClientAssembly.Framework_Client obj)
        {
            byte threadIdCounter = 0;
            obj.Get_client().Get_execute().Set_thread(threadIdCounter, Thread.CurrentThread);
            obj.Get_client().Get_execute().Get_execute_Control().Set_flag_ThreadInitialised(obj, threadIdCounter, false);
            System.Console.WriteLine("Thread Initalised => CurrentThread()" + (threadIdCounter).ToString());//TESTBENCH

            threadIdCounter++;
            obj.Get_client().Get_execute().Set_thread(threadIdCounter, new Thread(() => _networking_Client.Thread_IO_Client(threadIdCounter)));
            obj.Get_client().Get_execute().Get_thread(threadIdCounter).Start();
            System.Console.WriteLine("Thread Initalised => Thread_IO_Client on core " + (threadIdCounter).ToString());//TESTBENCH

            threadIdCounter++;
            while (threadIdCounter < obj.Get_client().Get_global().Get_numberOfCores())
            {
                obj.Get_client().Get_execute().Set_thread(threadIdCounter, new Thread(() => obj.Get_client().Get_algorithms().Get_concurrent((byte)(threadIdCounter - (byte)2)).Thread_Concurrent(threadIdCounter)));
                obj.Get_client().Get_execute().Get_thread(threadIdCounter).Start();
                System.Console.WriteLine("Thread Initalised => Thread_Concurrent on core " + (threadIdCounter).ToString());//TESTBENCH
                threadIdCounter++;
            }
        }

        public void Create_And_Run_Graphics(Avril_FSD.ClientAssembly.Framework_Client obj)
        {
            System.Console.WriteLine("starting = > gameInstance");//TESTBENCH
            using (Avril_FSD.ClientAssembly.Game_Instance gameInstance = new Avril_FSD.ClientAssembly.Game_Instance())
            {
                gameInstance.Run(obj.Get_client().Get_data().Get_settings().Get_refreshRate());
            }
        }
.
.
.
    }   
}
````

#### Loading Client Shell Framwork - Scanner For Mouse and Keyboard.
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ClientAssembly/gameInstance/Game_Instance.cs
````
namespace Avril_FSD.ClientAssembly
{
    public sealed class Game_Instance : GameWindow
    {
.
.
.      
        protected override void OnLoad(EventArgs e)
        {
.
.
.
            Avril_FSD.ClientAssembly.Framework_Client obj = Avril_FSD.ClientAssembly.Program.Get_framework_Client();
            obj.Get_client().Get_execute().Initialise_Threads(obj);

            System.Console.WriteLine("OnLoad .. done");
        }

.
.
.
        private void HandleMouse(Avril_FSD.ClientAssembly.Framework_Client obj)
        {
            //Console.WriteLine("TESTBENCH => HandleMouse");
            MouseState mouseState = Mouse.GetCursorState();
            var buffer = obj.Get_client().Get_data().Get_input_Instnace().Get_FRONT_inputDoubleBuffer(obj);
            //Console.WriteLine("Get_isPraiseActive() = " + obj.Get_client().Get_data().Get_data_Control().Get_isPraiseActive(1));
            if (obj.Get_client().Get_data().Get_data_Control().Get_isPraiseActive(1) == false)
            {
                Console.WriteLine("TESTBENCH => Do PRAISE 1 start");
                obj.Get_client().Get_data().Get_data_Control().Set_isPraiseActive(1, true);
                buffer.Set_playerId(0);
                buffer.Set_praiseEventId(1);
                buffer.Get_input_Control().SelectSetIntputSubset(obj, buffer.Get_praiseEventId());
                var subset = (Avril_FSD.ClientAssembly.Praise_Files.Praise1_Input)buffer.Get_praiseInputBuffer_Subset();
                subset.Set_Mouse_X(mouseState.X);
                subset.Set_Mouse_Y(mouseState.Y);
                obj.Get_client().Get_data().Flip_InBufferToWrite();
                obj.Get_client().Get_data().Get_data_Control().Push_Stack_Client_InputAction(obj, obj.Get_client().Get_data().Get_input_Instnace().Get_stack_Client_InputSend(), obj.Get_client().Get_data().Get_input_Instnace().Get_BACK_inputDoubleBuffer(obj));
                Console.WriteLine("TESTBENCH => Do PRAISE 1 end");
            }
        }
        private void HandleKeyboard(Avril_FSD.ClientAssembly.Framework_Client obj, double dt)
        {
            var keyState = Keyboard.GetState();

            if (keyState.IsKeyDown(Key.Escape))
            {
                Exit();
            }
            if (keyState.IsKeyDown(Key.W))
            {
                
            }
            if (keyState.IsKeyDown(Key.S))
            {
                
            }

            if (keyState.IsKeyDown(Key.A))
            {
                
            }
            if (keyState.IsKeyDown(Key.D))
            {
                
            }
            _lastKeyboardState = keyState;
        }
.
.
.
    }
}
````

#### STACK: Client Input Action.
line 9:	private List<Avril_FSD.ClientAssembly.Inputs.Input> _stack_Client_InputSend;
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ClientAssembly/engine/Input_Instance.cs

### Server Shell App.

#### THREAD: NETWORKING Client Input Action Send.
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ClientAssembly/Networking_Client.cs
````
	if (obj.Get_client().Get_data().Get_data_Control().Get_flag_IsLoaded_Stack_InputAction() == true)
	{
		Avril_FSD.Library_For_WriteEnableForThreadsAt_CLIENTINPUTACTION.Write_Start(obj.Get_client().Get_execute().Get_program_WriteQue_C_IA(), 1);
		byte[] data = new byte[64];
		obj.Get_client().Get_data().Get_data_Control().Pop_Stack_InputAction(obj, obj.Get_client().Get_data().Get_input_Instnace().Get_FRONT_inputDoubleBuffer(obj), obj.Get_client().Get_data().Get_input_Instnace().Get_stack_Client_InputSend());
		obj.Get_client().Get_data().Flip_InBufferToWrite();
		obj.Get_client().Get_algorithms().Get_io_ListenRespond().Encode_NetworkingSteam_At_Client_Input(obj, obj.Get_client().Get_data().Get_input_Instnace().Get_BACK_inputDoubleBuffer(obj), data);
		_client_SOCKET.SendMessageToConnection(_connection, data);
		Avril_FSD.Library_For_WriteEnableForThreadsAt_CLIENTINPUTACTION.Write_End(obj.Get_client().Get_execute().Get_program_WriteQue_C_IA(), 1);
	}
````

### Server Shell App.

#### THREAD: NETWORKING Server Input Action Recieve.
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ServerAssembly/Networking_Server.cs
````
	Avril_FSD.Library_For_WriteEnableForThreadsAt_SERVERINPUTACTION.Write_Start(Avril_FSD.Library_For_Server_Concurrency.Get_program_WriteEnableStack_ServerInputAction(), 0);
	byte[] buffer = new byte[1024];
	netMessage.CopyTo(buffer);
	Avril_FSD.Library_For_Server_Concurrency.Select_Set_Intput_Subset(obj.Get_server().Get_execute().Get_program_ServerConcurrency(), buffer[0]);
	obj.Get_server().Get_algorithms().Get_io_ListenRespond().Decode_NetworkingSteam_At_Server_Input(obj, obj.Get_server().Get_data().Get_input_Instnace().Get_FRONT_inputDoubleBuffer(obj), buffer);
	Avril_FSD.Library_For_Server_Concurrency.Flip_InBufferToWrite(obj.Get_server().Get_execute().Get_program_ServerConcurrency());
	Avril_FSD.Library_For_Server_Concurrency.Push_Stack_InputPraises(obj.Get_server().Get_execute().Get_program_ServerConcurrency());
	if (Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Get_Flag_ConcurrentCoreState(obj.Get_server().Get_execute().Get_program_ServerConcurrency(), Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Get_coreId_To_Launch(obj.Get_server().Get_execute().Get_program_ServerConcurrency())) == Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Get_Flag_Idle(obj.Get_server().Get_execute().Get_program_ServerConcurrency()))
	{
		Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Request_Wait_Launch(obj.Get_server().Get_execute().Get_program_ServerConcurrency(), Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Get_coreId_To_Launch(obj.Get_server().Get_execute().Get_program_ServerConcurrency()));
	}
	Avril_FSD.Library_For_WriteEnableForThreadsAt_SERVERINPUTACTION.Write_End(Avril_FSD.Library_For_Server_Concurrency.Get_program_WriteEnableStack_ServerInputAction(), 0);
````

### LIB_Concurrent_IO_Server.

#### THREAD: Server Concurreny Loop.
````
void Avril_FSD::Concurrent::Thread_Concurrency(Avril_FSD::Framework_Server* obj, __int8 concurrent_coreId)
{
    bool doneOnce = true;
    while (obj->Get_Server_Assembly()->Get_Execute()->Get_Control_Of_Execute()->GetFlag_ThreadInitialised(concurrent_coreId) == true)
    {
        if (doneOnce == true)
        {
            obj->Get_Server_Assembly()->Get_Execute()->Get_Control_Of_Execute()->SetConditionCodeOfThisThreadedCore(concurrent_coreId);
            doneOnce = false;
        }

    }
    std::cout << "Thread Initialised: ID=" << (concurrent_coreId) << " => Thread_Concurrency()" << std::endl;//TestBench
    while (obj->Get_Server_Assembly()->Get_Execute()->Get_Control_Of_Execute()->GetFlag_SystemInitialised(obj) == true)
    {

    }
    std::cout << "Thread Starting " << (concurrent_coreId) << " => Thread_Concurrency()" << std::endl;//TestBench
    while (obj->Get_Server_Assembly()->Get_Execute()->Get_Control_Of_Execute()->GetFlag_SystemInitialised(obj) == false)
    {
        switch (Avril_FSD::CLIBLaunchEnableForConcurrentThreadsAtSERVER::Get_Flag_ConcurrentCoreState(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_ConcurrentQue_Server(), concurrent_coreId))
        {
        case false:

            break;

        case true:
            if(obj->Get_Server_Assembly()->Get_Data()->Get_Data_Control()->Get_flag_IsStackLoaded_Server_InputAction() == true)
            {
                    Avril_FSD::CLIBWriteEnableForThreadsAtSERVERINPUTACTION::Write_Start(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_WriteEnable_ServerInputAction(), (__int8)(concurrent_coreId + (__int8)1));
                    obj->Get_Server_Assembly()->Get_Data()->Get_OutputRefferenceOfCore(concurrent_coreId)->Get_control_Of_Output()->SelectSet_Output_Subset(obj, obj->Get_Server_Assembly()->Get_Data()->Get_OutputRefferenceOfCore(concurrent_coreId)->Get_out_praiseEventId(), concurrent_coreId);
                    obj->Get_Server_Assembly()->Get_Algorithms()->Get_Concurrent(concurrent_coreId)->Get_Concurrent_Control()->SelectSet_Algorithm_Subset(obj, obj->Get_Server_Assembly()->Get_Data()->Get_InputRefferenceOfCore(concurrent_coreId)->GetPraiseEventId(), concurrent_coreId);
                    obj->Get_Server_Assembly()->Get_Data()->Get_Data_Control()->Store_Praise_In_Data_To_GameInstance_Data(obj, obj->Get_Server_Assembly()->Get_Data()->Get_Stack_InputPraise());
                    obj->Get_Server_Assembly()->Get_Data()->Get_Data_Control()->Pop_Stack_InputPraises(obj, concurrent_coreId);
                    Avril_FSD::CLIBWriteEnableForThreadsAtSERVERINPUTACTION::Write_End(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_WriteEnable_ServerInputAction(), (__int8)(concurrent_coreId + (__int8)1));
                    obj->Get_Server_Assembly()->Get_Algorithms()->Get_Concurrent(concurrent_coreId)->Do_Concurrent_Algorithm_For_PraiseEventId(
                        obj,
                        obj->Get_Server_Assembly()->Get_Data()->Get_InputRefferenceOfCore(concurrent_coreId)->Get_playerId(),
                        obj->Get_Server_Assembly()->Get_Data()->Get_InputRefferenceOfCore(concurrent_coreId)->GetPraiseEventId(),
                        obj->Get_Server_Assembly()->Get_Algorithms()->Get_Concurrent(concurrent_coreId)->Get_Algorithm_Subset(),
                        obj->Get_Server_Assembly()->Get_Data()->Get_InputRefferenceOfCore(concurrent_coreId)->Get_InputBuffer_Subset(),
                        obj->Get_Server_Assembly()->Get_Data()->Get_OutputRefferenceOfCore(concurrent_coreId)->Get_praiseOutputBuffer_Subset()
                    );
                    Avril_FSD::Library_WriteEnableForThreadsAt_SERVEROUTPUTRECIEVE::Write_Start(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_WriteEnable_ServerOutputRecieve(), (__int8)(concurrent_coreId + (__int8)1));
                    obj->Get_Server_Assembly()->Get_Data()->Get_Data_Control()->Push_Stack_Output(obj, concurrent_coreId);
                    obj->Get_Server_Assembly()->Get_Data()->Get_Data_Control()->Store_Praise_Out_Data_To_GameInstance_Data(obj, obj->Get_Server_Assembly()->Get_Data()->Get_Stack_OutputPraise());
                    Avril_FSD::CLIBLaunchEnableForConcurrentThreadsAtSERVER::Thread_End(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_ConcurrentQue_Server(), concurrent_coreId);
                    if (obj->Get_Server_Assembly()->Get_Data()->Get_Data_Control()->Get_flag_IsStackLoaded_Server_OutputRecieve() == true)
                    {
                        if (Avril_FSD::CLIBLaunchEnableForConcurrentThreadsAtSERVER::Get_Flag_ConcurrentCoreState(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_ConcurrentQue_Server(), Avril_FSD::CLIBLaunchEnableForConcurrentThreadsAtSERVER::Get_coreId_To_Launch(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_ConcurrentQue_Server())) == Avril_FSD::CLIBLaunchEnableForConcurrentThreadsAtSERVER::Get_Flag_Idle(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_ConcurrentQue_Server()))
                        {
                            Avril_FSD::CLIBLaunchEnableForConcurrentThreadsAtSERVER::Request_Wait_Launch(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_ConcurrentQue_Server(), Avril_FSD::CLIBLaunchEnableForConcurrentThreadsAtSERVER::Get_coreId_To_Launch(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_ConcurrentQue_Server()));
                        }
                    }
                   Avril_FSD::Library_WriteEnableForThreadsAt_SERVEROUTPUTRECIEVE::Write_End(obj->Get_Server_Assembly()->Get_Execute()->Get_Program_WriteEnable_ServerOutputRecieve(), (__int8)(concurrent_coreId + (__int8)1));
                //}
            }
            break;
        }
    }
}
````

#### STACK Server Shell Output Action.
line 11: 	private List<Avril_FSD.ServerAssembly.Outputs.Output> _stack_Client_OutputRecieves;
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ServerAssembly/engine/Data.cs

### Server Shell App.

#### THREAD: NETWORKING Server Output Action Send.
````
	if (obj.Get_server().Get_data().Get_data_Control().Get_flag_IsLoaded_Stack_OutputAction())
	{
		Avril_FSD.Library_For_WriteEnableForThreadsAt_SERVEROUTPUTRECIEVE.Write_Start(Avril_FSD.Library_For_Server_Concurrency.Get_program_WriteEnableStack_ServerOutputRecieve(), 0);
		byte[] data = new byte[64];
		var output = obj.Get_server().Get_data().Get_output_Instnace().Get_FRONT_outputDoubleBuffer(obj);
		output.Get_output_Control().SelectSetOutputSubset(obj, output.Get_praiseEventId());
		obj.Get_server().Get_algorithms().Get_io_ListenRespond().Encode_NetworkingSteam_At_Server_Output(obj, output, data);
		address_CLIENT.SetAddress(info.connectionInfo.address.GetIP(), 27001);
		uint connection = _server_SOCKET.Connect(ref address_CLIENT);
		_server_SOCKET.SendMessageToConnection(connection, data);
		_server_SOCKET.CloseConnection(info.connection);
		Avril_FSD.Library_For_WriteEnableForThreadsAt_SERVEROUTPUTRECIEVE.Write_End(Avril_FSD.Library_For_Server_Concurrency.Get_program_WriteEnableStack_Serv
````

### Client Shell App.

#### THREAD: NETWORKING Client Output Action Recieve.
````
		Avril_FSD.Library_For_WriteEnableForThreadsAt_SERVERINPUTACTION.Write_Start(Avril_FSD.Library_For_Server_Concurrency.Get_program_WriteEnableStack_ServerInputAction(), 0);
		byte[] buffer = new byte[1024];
		netMessage.CopyTo(buffer);
		Avril_FSD.Library_For_Server_Concurrency.Select_Set_Intput_Subset(obj.Get_server().Get_execute().Get_program_ServerConcurrency(), buffer[0]);
		obj.Get_server().Get_algorithms().Get_io_ListenRespond().Decode_NetworkingSteam_At_Server_Input(obj, obj.Get_server().Get_data().Get_input_Instnace().Get_FRONT_inputDoubleBuffer(obj), buffer);
		Avril_FSD.Library_For_Server_Concurrency.Flip_InBufferToWrite(obj.Get_server().Get_execute().Get_program_ServerConcurrency());
		Avril_FSD.Library_For_Server_Concurrency.Push_Stack_InputPraises(obj.Get_server().Get_execute().Get_program_ServerConcurrency());
		if (Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Get_Flag_ConcurrentCoreState(obj.Get_server().Get_execute().Get_program_ServerConcurrency(), Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Get_coreId_To_Launch(obj.Get_server().Get_execute().Get_program_ServerConcurrency())) == Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Get_Flag_Idle(obj.Get_server().Get_execute().Get_program_ServerConcurrency()))
		{
			Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Request_Wait_Launch(obj.Get_server().Get_execute().Get_program_ServerConcurrency(), Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_SERVER.Get_coreId_To_Launch(obj.Get_server().Get_execute().Get_program_ServerConcurrency()));
		}
		Avril_FSD.Library_For_WriteEnableForThreadsAt_SERVERINPUTACTION.Write_End(Avril_FSD.Library_For_Server_Concurrency.Get_program_WriteEnableStack_ServerInputAction(), 0);
````

## THREAD: Client Shell Output Recieve.
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ClientAssembly/Networking_Client.cs
````
 Avril_FSD.Library_For_WriteEnableForThreadsAt_CLIENTOUTPUTRECIEVE.Write_Start(obj.Get_client().Get_execute().Get_program_WriteQue_C_OR(), 1);
                            byte[] buffer = new byte[1024];
                            netMessage.CopyTo(buffer);
                            obj.Get_client().Get_data().Get_output_Instnace().Get_BACK_outputDoubleBuffer(obj).Set_praiseOutputBuffer_Subset(buffer[0]);
                            obj.Get_client().Get_algorithms().Get_io_ListenRespond().Decode_NetworkingSteam_At_Client_Recieve(obj, obj.Get_client().Get_data().Get_output_Instnace().Get_BACK_outputDoubleBuffer(obj), buffer);
                            obj.Get_client().Get_data().Flip_OutBufferToWrite();
                            obj.Get_client().Get_data().Get_data_Control().Push_Stack_Client_OutputRecieve(obj, obj.Get_client().Get_data().Get_output_Instnace().Get_stack_Client_OutputRecieves(), obj.Get_client().Get_data().Get_output_Instnace().Get_FRONT_outputDoubleBuffer(obj));
                            if (obj.Get_client().Get_data().Get_data_Control().Get_flag_IsLoaded_Stack_OutputRecieve())
                            {
                                if (Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Get_State_LaunchBit(obj.Get_client().Get_execute().Get_program_ConcurrentQue_C()))
                                {
                                    Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Request_Wait_Launch(obj.Get_client().Get_execute().Get_program_ConcurrentQue_C(), Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Get_coreId_To_Launch(obj.Get_client().Get_execute().Get_program_ConcurrentQue_C()));
                                }
                            }
                            Avril_FSD.Library_For_WriteEnableForThreadsAt_CLIENTOUTPUTRECIEVE.Write_End(obj.Get_client().Get_execute().Get_program_WriteQue_C_OR(), 1);
````

#### STACK: Client Shell Output Recieve.
line 8:	private Avril_FSD.ClientAssembly.Outputs.Output[] _doublebuffer_Client_Output_Recieve;
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ClientAssembly/engine/Output_Instance.cs

#### THREAD: Client Concurrency.
https://github.com/OpenFSD/Avril_Full_Stack_Development_Template/blob/master/APP_ClientAssembly/engine/Concurrent.cs
````
public void Thread_Concurrent(byte threadId)
        {
            byte _concurrentThreadId = (byte)(threadId - (byte)2);
            Avril_FSD.ClientAssembly.Framework_Client obj = Avril_FSD.ClientAssembly.Program.Get_framework_Client();
            obj.Get_client().Get_execute().Get_execute_Control().Set_flag_ThreadInitialised(obj, threadId, false);
            bool doneOnce_A = false;
            while (obj.Get_client().Get_execute().Get_execute_Control().Get_flag_SystemInitialised() == true)
            {
                if (doneOnce_A == false)
                {
                    doneOnce_A = true;
                    obj.Get_client().Get_execute().Get_execute_Control().Set_flag_ThreadInitialised(obj, threadId, false);
                }
            }
            while (obj.Get_client().Get_execute().Get_execute_Control().Get_exitApplication() == false)
            {
                if (obj.Get_client().Get_data().Get_data_Control().Get_flag_IsLoaded_Stack_OutputRecieve())
                {
                    System.Console.WriteLine("Thread[" + (threadId).ToString() + "] => Get_flag_IsLoaded_Stack_OutputRecieve = " + obj.Get_client().Get_data().Get_data_Control().Get_flag_IsLoaded_Stack_OutputRecieve());//TestBench
                    Avril_FSD.Library_For_WriteEnableForThreadsAt_CLIENTOUTPUTRECIEVE.Write_Start(obj.Get_client().Get_execute().Get_program_WriteQue_C_OR(), (byte)(_concurrentThreadId + (byte)1));
                    obj.Get_client().Get_algorithms().Get_concurrent(_concurrentThreadId).Get_concurrent_Control().SelectSet_Algorithm_Subset(obj, obj.Get_client().Get_data().Get_output_Instnace().Get_BACK_outputDoubleBuffer(obj).Get_praiseEventId(), _concurrentThreadId);
                    obj.Get_client().Get_data().Get_data_Control().Pop_Stack_OutputRecieve(obj, obj.Get_client().Get_data().Get_output_Instnace().Get_BACK_outputDoubleBuffer(obj), obj.Get_client().Get_data().Get_output_Instnace().Get_stack_Client_OutputRecieves());
                    obj.Get_client().Get_data().Flip_OutBufferToWrite();
                    obj.Get_client().Get_data().Get_data_Control().Do_Store_PraiseOutputRecieve_To_GameInstanceData(obj, obj.Get_client().Get_data().Get_output_Instnace().Get_stack_Client_OutputRecieves().ElementAt(1));
                    obj.Get_client().Get_data().Get_data_Control().Set_isPraiseActive(obj.Get_client().Get_data().Get_output_Instnace().Get_FRONT_outputDoubleBuffer(obj).Get_praiseEventId(), false);
                    Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Thread_End(obj.Get_client().Get_execute().Get_program_ConcurrentQue_C(), _concurrentThreadId);
                    if (obj.Get_client().Get_data().Get_data_Control().Get_flag_IsLoaded_Stack_OutputRecieve())
                    {
                        if(Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Get_State_LaunchBit(obj.Get_client().Get_execute().Get_program_ConcurrentQue_C()) == Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Get_Flag_Idle(obj.Get_client().Get_execute().Get_program_ConcurrentQue_C()))
                        {
                            Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Request_Wait_Launch(obj.Get_client().Get_execute().Get_program_ConcurrentQue_C(), Avril_FSD.Library_For_LaunchEnableForConcurrentThreadsAt_CLIENT.Get_coreId_To_Launch(obj.Get_client().Get_execute().Get_program_ConcurrentQue_C()));
                        }
                    }
                    Avril_FSD.Library_For_WriteEnableForThreadsAt_CLIENTOUTPUTRECIEVE.Write_End(obj.Get_client().Get_execute().Get_program_WriteQue_C_OR(), (byte)(_concurrentThreadId + (byte)1));
                }
            }
````


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

## TestBench(s) in C#.
 - https://github.com/AvrilOpenCFSD/Avril_Full_Stack_Development_Template_Libraries/blob/master/TESTBENCH_Libraries_Csharp/Program.cs


## Prospecting In A Neural Network AI IO Device.
 - https://github.com/AvrilOpenCFSD/Neural_Network_AI_IO_Device
