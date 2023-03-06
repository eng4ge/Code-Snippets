```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Helper
{
    class Program
    {
        static void Main(string[] args)
        {
			byte[] buf = new byte[749] {0xfc,... };
			byte[] encoded = new byte[buf.Length];

			for (int i = 0; i < buf.Length; i++)
			{
				encoded[i] = (byte)(((uint)buf[i] + 2) & 0xFF);
			}

			StringBuilder hex = new StringBuilder(encoded.Length * 2);
			uint counter = 0;
			foreach (byte b in encoded)
			{
				//hex.AppendFormat("0x{0:x2}, ", b);
				//VBA FORMAT
				hex.AppendFormat("{0:D}, ", b);
				counter++;
				if (counter % 50 == 0) 
				{
					hex.AppendFormat("_{0}", Environment.NewLine);
				}
			}

			Console.WriteLine("The payload is: " + hex.ToString());
			Console.ReadLine();
		}
    }
}


```

More Info:
![[Evasion Techniques and Breaching Defenses#6 5 2 Encrypting the C Shellcode Runner]]