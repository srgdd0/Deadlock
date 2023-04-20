# Deadlock
> The problem with this code is that it can lead to a deadlock situation. A deadlock occurs when two or more threads are blocked, waiting for each other to release the locks they hold, and as a result, they cannot make any progress.

## fix
```python
import threading
import time

l1 = threading.Lock()
l2 = threading.Lock()

def f1(name):
    print('thread', name, 'about to lock l1')
    with l1:
        print('thread', name, 'has lock l1')
        time.sleep(0.3)
        print('thread', name, 'about to lock l2')
        with l2:
            print('thread', name, 'has lock l2')
            print('thread', name, 'has both locks')

def f2(name):
    print('thread', name, 'about to lock l1')
    with l1:
        print('thread', name, 'has lock l1')
        time.sleep(0.3)
        print('thread', name, 'about to lock l2')
        with l2:
            print('thread', name, 'has lock l2')
            print('thread', name, 'has both locks')

if __name__ == '__main__':
    t1 = threading.Thread(target=f1, args=['t1',])
    t2 = threading.Thread(target=f2, args=['t2',])

    t1.start()
    t2.start()

```
**both threads acquire locks in the same order (l1 before l2), which ensures that they will not end up waiting for each other to release the lock they hold. As a result, the code will not run into a deadlock situation.**

## fix on c#
```csharp
using System;
using System.Threading;

class Program
{
    static object l1 = new object();
    static object l2 = new object();

    static void f1(string name)
    {
        Console.WriteLine("thread " + name + " about to lock l1");
        lock (l1)
        {
            Console.WriteLine("thread " + name + " has lock l1");
            Thread.Sleep(300);
            Console.WriteLine("thread " + name + " about to lock l2");
            lock (l2)
            {
                Console.WriteLine("thread " + name + " has lock l2");
                Console.WriteLine("thread " + name + " has both locks");
            }
        }
    }

    static void f2(string name)
    {
        Console.WriteLine("thread " + name + " about to lock l1");
        lock (l1)
        {
            Console.WriteLine("thread " + name + " has lock l1");
            Thread.Sleep(300);
            Console.WriteLine("thread " + name + " about to lock l2");
            lock (l2)
            {
                Console.WriteLine("thread " + name + " has lock l2");
                Console.WriteLine("thread " + name + " has both locks");
            }
        }
    }

    static void Main()
    {
        Thread t1 = new Thread(() => f1("t1"));
        Thread t2 = new Thread(() => f2("t2"));

        t1.Start();
        t2.Start();
    }
}

```
