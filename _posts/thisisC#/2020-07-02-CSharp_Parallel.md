---
layout: single
title: "[C#] Parallel 클래스를 활용한 병렬 프로그래밍"
categories:
  - C#
toc: true
toc_sticky: true
toc_label: "On this Page"
date: 2020-07-02
---





이번 시간에는 병렬 처리를 위한 Parallel 클래스를 알아보자.

-----------

#### Parallel 클래스

병렬 처리는 하나의 작업을 여러 작업자가 나누어 수행한 뒤 하나의 결과로 만드는 것을 말한다.  

Parallel 클래스는 Parallel.For(), Parallel.Foreach() 등의 메소드를 제공하면서 병렬처리를 쉽게 구현할 수 있도록 해준다.  방대한 양의 데이터를 처리하는 경우 스레드간에 서로의 값에 영향을 받지 않는다면 순차 처리하는 것보다 빠른 시간 내에 처리가 가능하다.  단, 다중스레드로 병렬처리 하는 경우 반복문 내의 작업이 인덱스 순서대로 진행되는것은 아니다.



아래의 예제를 살펴보자.

```c#
class Program
{
    static void Main(string[] args)
    {
        // 1. 순차적 실행
        for (int i = 0; i < 1000; i++){
            Console.WriteLine("{0}: {1}", Thread.CurrentThread.ManagedThreadId, i);
        }

        // 2. Parallel 클래스를 활용한 병렬 처리
        Parallel.For(0, 1000, (i) => {
        	Console.WriteLine("{0}: {1}", Thread.CurrentThread.ManagedThreadId, i);
        });
        Console.Read();
    }
}
```

첫 번째 for 문과 두 번째 Parallel 클래스를 활용한 방법은 모두 동일하게 0부터 999까지의 정수를 출력하기 위한 예제이다.  

1번째 방법으로 실행하는 경우 Thread.CurrentThread.ManagedThreadId 가 동일하게 1로 찍힐 것이며, i의 값은 순차적으로 증가하며 출력된다.  

반면 2번째 방법으로 하면 각기 다른 ThreadId가 찍히며 i의 순서도 랜덤하게 찍힐 것이다.  

Parallel 클래스를 활용하면 쉽게 병렬 처리를 하면서 순서를 보장받지 못하지만, 멀티스레드 방식으로 더 빠른 처리속도를 낼 수 있다.





아래의 예제를 살펴보자.  두 행렬의 곱을 계산하며 두 행렬에 들어간 값은 랜덤으로 배정하였다.  System.Diagnostics.Stopwatch 클래스를 사용하여 순차처리와 병렬처리의 성능을 비교하도록 하였다.

```c#
using System;
using System.Diagnostics;
using System.Threading.Tasks;

class MultiplyMatrices
{
    #region Sequential_Loop
    static void MultiplyMatricesSequential(double[,] matA, double[,] matB,
                                            double[,] result)
    {
        int matACols = matA.GetLength(1);
        int matBCols = matB.GetLength(1);
        int matARows = matA.GetLength(0);

        for (int i = 0; i < matARows; i++)
        {
            for (int j = 0; j < matBCols; j++)
            {
                double temp = 0;
                for (int k = 0; k < matACols; k++)
                {
                    temp += matA[i, k] * matB[k, j];
                }
                result[i, j] += temp;
            }
        }
    }
    #endregion

    #region Parallel_Loop
    static void MultiplyMatricesParallel(double[,] matA, double[,] matB, double[,] result)
    {
        int matACols = matA.GetLength(1);
        int matBCols = matB.GetLength(1);
        int matARows = matA.GetLength(0);

        // A basic matrix multiplication.
        // Parallelize the outer loop to partition the source array by rows.
        Parallel.For(0, matARows, i =>
        {
            for (int j = 0; j < matBCols; j++)
            {
                double temp = 0;
                for (int k = 0; k < matACols; k++)
                {
                    temp += matA[i, k] * matB[k, j];
                }
                result[i, j] = temp;
            }
        }); // Parallel.For
    }
    #endregion

    #region Main
    static void Main(string[] args)
    {
        // Set up matrices. Use small values to better view
        // result matrix. Increase the counts to see greater
        // speedup in the parallel loop vs. the sequential loop.
        int colCount = 180;
        int rowCount = 2000;
        int colCount2 = 270;
        double[,] m1 = InitializeMatrix(rowCount, colCount);
        double[,] m2 = InitializeMatrix(colCount, colCount2);
        double[,] result = new double[rowCount, colCount2];

        // First do the sequential version.
        Console.Error.WriteLine("Executing sequential loop...");
        Stopwatch stopwatch = new Stopwatch();
        stopwatch.Start();

        MultiplyMatricesSequential(m1, m2, result);
        stopwatch.Stop();
        Console.Error.WriteLine("Sequential loop time in milliseconds: {0}",
                                stopwatch.ElapsedMilliseconds);

        // For the skeptics.
        OfferToPrint(rowCount, colCount2, result);

        // Reset timer and results matrix.
        stopwatch.Reset();
        result = new double[rowCount, colCount2];

        // Do the parallel loop.
        Console.Error.WriteLine("Executing parallel loop...");
        stopwatch.Start();
        MultiplyMatricesParallel(m1, m2, result);
        stopwatch.Stop();
        Console.Error.WriteLine("Parallel loop time in milliseconds: {0}",
                                stopwatch.ElapsedMilliseconds);
        OfferToPrint(rowCount, colCount2, result);

        // Keep the console window open in debug mode.
        Console.Error.WriteLine("Press any key to exit.");
        Console.ReadKey();
    }
    #endregion

    #region Helper_Methods
    static double[,] InitializeMatrix(int rows, int cols)
    {
        double[,] matrix = new double[rows, cols];

        Random r = new Random();
        for (int i = 0; i < rows; i++)
        {
            for (int j = 0; j < cols; j++)
            {
                matrix[i, j] = r.Next(100);
            }
        }
        return matrix;
    }

    private static void OfferToPrint(int rowCount, int colCount, double[,] matrix)
    {
        Console.Error.Write("Computation complete. Print results (y/n)? ");
        char c = Console.ReadKey(true).KeyChar;
        Console.Error.WriteLine(c);
        if (Char.ToUpperInvariant(c) == 'Y')
        {
            if (!Console.IsOutputRedirected) Console.WindowWidth = 180;
            Console.WriteLine();
            for (int x = 0; x < rowCount; x++)
            {
                Console.WriteLine("ROW {0}: ", x);
                for (int y = 0; y < colCount; y++)
                {
                    Console.Write("{0:#.##} ", matrix[x, y]);
                }
                Console.WriteLine();
            }
        }
    }
    #endregion
}
```

병렬처리하는 경우 성능을 무리해서 사용하게 하지 않고 최대한 많은 프로세스를 사용하는것이 목적일 것이다.  위 예제에서는 내부 루프에서 많은 작업이 수행되지 않으므로 외부 루프만 병렬처리 하였다.  적은 작업량과 원치 않는 캐시로 인해 중첩된 병렬 루프에서는 성능이 저하될 수 있다.  따라서 **외부 루프만 병렬처리 하는 것이 대부분의 시스템에서 동시성의 이점을 극대화하는 가장 좋은 방법이다.**



**- 참고**

성능 테스트를 하는 경우 콘솔 또는 파일 시스템과 같은 공유 리소스에 대한 동기 호출은 병렬 루프의 성능을 상당히 저하시킨다.  따라서 성능 테스트 시에는 루프 내에서 Console.WriteLine 등을 호출하지 않도록 하자.









---------------------------------

[참고] https://docs.microsoft.com/ko-kr/dotnet/standard/parallel-programming/how-to-write-a-simple-parallel-for-loop