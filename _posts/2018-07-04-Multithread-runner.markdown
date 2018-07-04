---
layout: post
title:  "Multithread runner for load testing"
date:   2018-07-04 13:59:00 -0600
categories: C#
---

Here is a way to run test in a thread pool.

First we created a state class
```C#
    using System;

    namespace MultiThreadHelper {
        public class State<T> {
            public T Result;
            public Exception Exception;
            public bool IsComplete;
        }
    }
```

Then have the major runner

```C#
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading;

    namespace MultiThreadHelper{
        public static class MultiThread{
            public static IList<State<T>> Run<T>(Func<T> func, int threadCount){
                var states = new List<State<T>>();
                for (var i = 0; i < threadCount; i++){
                    states.Insert(i, new State<T>());
                    ThreadPool.QueueUserWorkItem(s =>{
                        var state = (State<T>) s;
                        try{
                            state.Result = func();
                        }
                        catch (Exception e){
                            state.Exception = e;
                        }
                        state.IsComplete = true;
                    },
                                                states[i]);
                }
                while (states.Any(s => !s.IsComplete)) Thread.Sleep(100);
                return states;
            }
        }
    }
```

The following code is a test against a website
```C#
   [Test]
    public void HttpClientTest()
    {
        // Arrange
        var client = new HttpClient();
        var count = 50;
        var url = "http://www.sample.com/";

        // Act
        var results = MultiThread.Run(() => client.GetAsync(url).Result.StatusCode, count);

        // Assert
        Assert.That(results.Count(), Is.EqualTo(count));
        Assert.IsFalse(results.Any(r => r.Exception != null));
        foreach (var result in results) Console.WriteLine(result.Result);
    }
```

If you use WebRequest, you have to set 
```C#
    ServicePointManager.DefaultConnectionLimit = 50;
```
in order to run your tests simutanously.

I used this code to find out a few multithreading bugs in web applications.

