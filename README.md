<p align="center">
    <img width="256" height="256" src="https://user-images.githubusercontent.com/40871836/43460394-2116cd86-9496-11e8-8c8a-df6522c3037c.png">
</p>

# ShellGen
<p align="left">
    <!-- Version -->
    <img src="https://img.shields.io/badge/release-1.0.0-brightgreen.svg">
    <!-- <img src="https://img.shields.io/appveyor/ci/gruntjs/grunt.svg"> -->
    <!-- Docs -->
    <img src="https://img.shields.io/badge/docs-not%20found-lightgrey.svg">
    <!-- License -->
    <img src="https://img.shields.io/badge/license-MIT-blue.svg">
</p>

ShellGen is a dynamic shell code generator with multiple output types which can be formatted in binary, hexadecimal, and the typical shellcode output standard. Outputs are also able to be encrypted and shellcode can be generated by either reading all of the bytes of a particular file or a byte array, whether that's embedded, or again, read in from a filestream. The generated output is structured in what is called a `Shell` and contains a few properties:

- **ID** *// The alphanumeric value assigned to the generated shell for easy maintainability.*
- **Code** *// The actual plainbyte/encrypted byte code of a specified file or raw bytes.*
- **Error** *// Any caught exception if anything goes wrong during generation.*

A   `Shell` also has a serializable property allowing the ability to serialize the entire structure to a file or byte array instead of copying individual components of the generated shell object.

### Requirements
- .NET Framwork 4.6.1

# Features
- Generate shellcode from a file or byte array
- Multiple output types for generated shellcode
    - Plainbyte
    - Hexadecimal
    - Shellcode (standard)
- Generate unique IDs for all generated shells
- Encode bytes using Base64
- Serialize generated shells to a file or byte array

# Examples

### Generating Shells
This example shows how to generate a shellcode `Shell` object both with and without encryption and how to serialize the object into a byte array for later usage or for saving to a file — which we will do here — for later retrieval.

```c#
using System;
using System.IO;
using System.Text;
using System.Runtime.Serialization.Formatters.Binary;
using ShellGen;

namespace Example
{
    class Program
    {
        public static void Main(string[] args)
        {
            // Get the bytes of a message.
            var message = "Hello, World! This is an example message!";
            var messageBytes = Encoding.UTF8.GetBytes(message);

            // Generate a standard shell without encryption.
            Shell standardShell = ShellGenerator.GenerateShell(messageBytes, FormatType.Shellcode);
            Console.WriteLine("A shell has been generated!");

            // Generate a plainbyte shell without encryption.
            Shell plainShell = ShellGenerator.GenerateShell(messageBytes, FormatType.Plain);
            Console.WriteLine("A plainbyte shell has been generated!");

            // Generate a hexadecimal shell without encryption.
            Shell hexShell = ShellGenerator.GenerateShell(messageBytes, FormatType.Hex);
            Console.WriteLine("A hexadecimal shell has been generated!");

            // Generate a shell with encryption.
            Shell encryptedShell = ShellGenerator.GenerateShell(messageBytes, FormatType.Shellcode, "SomeSecurePassword");
            Console.WriteLine("An encrypted shell has been generated!");

            // Serialize one or more shell to a file.
            var serializedBytes = Serialize(encryptedShell);
            if (serializedBytes != null)
            {
                File.WriteAllBytes("PathForDestinationFile", serializedBytes);
                Console.WriteLine("Shell has been saved!");
            }

            // Wait for user interaction.
            Console.Read();
        }

        /// <summary>
        /// Serializes an object that has the <see cref="SerializableAttribute"/> attached.
        /// </summary>
        /// <param name="obj">The object to be serialized to bytes.</param>
        public static byte[] Serialize(object obj)
        {
            try
            {
                var bf = new BinaryFormatter();
                var ms = new MemoryStream();
                bf.Serialize(ms, obj);
                return ms.ToArray();
            }
            catch { return null; }
        }
    }
}
```

### Output
The plaintext output of the standard generated shell given in the example should be expected to look like the following: <br>
```c#
unsigned char shellcode[] = 
{
    0x53,0x47,0x56,0x73,0x62,0x47,0x38,0x73,0x49,0x46,0x64,0x76,
    0x63,0x6D,0x78,0x6B,0x49,0x53,0x42,0x55,0x61,0x47,0x6C,0x7A,
    0x49,0x47,0x6C,0x7A,0x49,0x47,0x46,0x75,0x49,0x47,0x56,0x34,
    0x59,0x57,0x31,0x77,0x62,0x47,0x55,0x67,0x62,0x57,0x56,0x7A,
    0x63,0x32,0x46,0x6E,0x5A,0x53,0x45,0x3D
};
unsigned int size = 56
```

# Credits
**Icon:** `Freepik`
###### https://www.flaticon.com/authors/freepik
<br>

**Encryption:** `sdrapkin`
###### https://github.com/sdrapkin/SecurityDriven.Inferno

# License

Copyright © ∞ Jason Tanner (Antebyte)

All rights reserved.

The MIT License (MIT)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

Except as contained in this notice, the name of the above copyright holder
shall not be used in advertising or otherwise to promote the sale, use or
other dealings in this Software without prior written authorization.
