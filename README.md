# zad 1

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp21
{
    internal class Program
    {
        static void Main(string[] args)
        {
            string text = Console.ReadLine();
            Stack<char> stack = new Stack<char>();
            foreach (char a in text)
            {
                stack.Push(a);
            }
            Console.WriteLine(string.Join("", stack));
        }
    }
}

# zad 2

using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp112
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var input = Console.ReadLine();
            var values = input.Split(' ');
            var stack = new Stack<string>(values.Reverse());
            while (stack.Count > 1)
            {
                int first = int.Parse(stack.Pop());
                string op = stack.Pop();
                int second = int.Parse(stack.Pop());
                switch(op)
                {
                    case "+":
                        int sum = first + second;
                        stack.Push(sum.ToString());
                        break;
                    case "-":
                        int diff = first - second;
                        stack.Push(diff.ToString());
                        break;
                }
            }
            Console.WriteLine(stack.Pop());



        }


    }
}

# zad 3

using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp112
{
    internal class Program
    {
        static void Main(string[] args)
        {
            var input = Console.ReadLine();
            var values = input.Split(' ');
            var stack = new Stack<string>(values.Reverse());
            while (stack.Count > 1)
            {
                int first = int.Parse(stack.Pop());
                string op = stack.Pop();
                int second = int.Parse(stack.Pop());
                switch(op)
                {
                    case "+":
                        int sum = first + second;
                        stack.Push(sum.ToString());
                        break;
                    case "-":
                        int diff = first - second;
                        stack.Push(diff.ToString());
                        break;
                }
            }
            Console.WriteLine(stack.Pop());



        }


    }
}

# zad 4

using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp112
{
    internal class Program
    {
        static void Main(string[] args)
        {
            string input = Console.ReadLine();
            Stack<int> indexes = new Stack<int>();
            for (int i = 0; i < input.Length; i++)
            {
                if (input[i] == '(')
                {
                    indexes.Push(i);
                }
                else
                {
                    if (input[i] == ')')
                    {
                        int startIndex = indexes.Pop();
                        string s = input.Substring(startIndex, i - startIndex + 1);
                        Console.WriteLine(s);
                    }
                }
            }
        }


    }
}

# zad 5

using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp112
{
    internal class Program
    {
        static void Main(string[] args)
        {
            string[] players = Console.ReadLine().Split().ToArray();
            int n = int.Parse(Console.ReadLine());
            Queue<string> queue = new Queue<string>(players);
            while(queue.Count != 1)
            {
                for (int i = 1; i < n; i++)
                {
                    queue.Enqueue(queue.Dequeue());
                }
                Console.WriteLine($"Removed {queue.Dequeue()}");
            }
            Console.WriteLine($"Last is {queue.Dequeue()}");
        }


    }
}

# zad 6

using System;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApp112
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int n = int.Parse(Console.ReadLine());
            Queue<string> queue = new Queue<string>();
            int count = 0;
            string input = string.Empty;
            while(input.ToLower() != "end")
            {
                input = Console.ReadLine();
                if(input.ToLower() ==  "end")
                {
                    break;
                }
                if(input == "green")
                {
                    int carsToPass = Math.Min(n, queue.Count);
                    for (int i = 0; i < carsToPass; i++)
                    {
                        Console.WriteLine($"{queue.Dequeue()} passed!");
                        count++;
                    }
                }
                else
                {
                    queue.Enqueue(input);
                }
            }
            Console.WriteLine($"{count} cars passed the crossroads.");
        }


    }
}
