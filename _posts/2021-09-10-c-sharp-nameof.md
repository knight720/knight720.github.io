---
layout: post
title:  "nameof"
date:   2021-09-10
tags: [C#]
categories: C#
---

最近看到一篇文章內一句話，引發我的好奇心所以來驗證一下跟我的想像的是不是一致。

**"nameof運算式(執行期間不會有任何作用)"**

> [Be Careful About Enum Performance Trap](https://medium.com/ricos-note/be-careful-about-enum-performance-trap-fc8bf1beaa4b)


- C# 程式碼
```c#
using System;

namespace ConsoleApp1
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            Console.WriteLine(Season.Summer.ToString());
            Console.WriteLine(nameof(Season.Summer));
        }
    }

    internal enum Season
    {
        Spring,
        Summer,
        Fall,
        Winter
    }
}
```
> [Source Code](https://github.com/knight720/TestDotNetCore/tree/master/Console/018_nameof/ConsoleApp1)

- 執行結果
```Console
Summer
Summer
```

- 反組譯 DLL
```c#
// ConsoleApp1.Program
using System;

private static void Main(string[] args)
{
	Console.WriteLine(Season.Summer.ToString());
	Console.WriteLine("Summer");
}
```

- 結論
nameof 在編譯後會轉換成字串，因此執行時期不在需要額外的運算資源，因此有較佳的效能。類似的作法還有在 const 也有出現。

- 同場加映

- C# 程式碼
```c#
using System;

namespace ConsoleApp2_const
{
    internal class Program
    {
        private int var_normal = 1;
        private readonly int var_readonly = 2;
        private const int var_const = 3;

        private static void Main(string[] args)
        {
            new Program().Print();
        }

        private void Print()
        {
            Console.WriteLine(var_normal);
            Console.WriteLine(var_readonly);
            Console.WriteLine(var_const);
        }
    }
}
```
> [Source Code](https://github.com/knight720/TestDotNetCore/tree/master/Console/018_nameof/ConsoleApp2_const)

- 執行結果
```Console
1
2
3
```

- 反組譯 DLL
```c#
// ConsoleApp2_const.Program
using System;

private void Print()
{
	Console.WriteLine(var_normal);
	Console.WriteLine(var_readonly);
	Console.WriteLine(3);
}
```