﻿# AutoExceptionHandler

[![NuGet Version](https://img.shields.io/nuget/v/AutoExceptionHandler?logo=nuget)](https://www.nuget.org/packages/AutoExceptionHandler/)[![GitHub Release](https://img.shields.io/github/v/release/retroandchill/AutoExceptionHandler?logo=github)](https://github.com/retroandchill/AutoExceptionHandler/releases)[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=retroandchill_ExceptionHandlerGenerator&metric=alert_status)](https://sonarcloud.io/summary/new_code?id=retroandchill_ExceptionHandlerGenerator) [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=retroandchill_ExceptionHandlerGenerator&metric=coverage)](https://sonarcloud.io/summary/new_code?id=retroandchill_ExceptionHandlerGenerator)

Simple code generator for creating exception handler classes for .NET projects.

## Installation
Install using Nuget: https://www.nuget.org/packages/AutoExceptionHandler/

```powershell
dotnet add package AutoExceptionHandler
```
## Usage
Using this library should be fairly straightforward. Simply create a class that uses
the provided annotations.
```csharp
using System;
using AutoExceptionHandler.Annotations;

namespace AutoExceptionHandler.Sample;

/// <summary>
/// This class used the ExceptionHandler attribute to mark it for generation.
/// </summary>
[ExceptionHandler]
public partial class ExampleHandler {
  /// <summary>
  /// General exception handler method. Generated by this library.
  /// </summary>
  /// <param name="ex">The passed exception. Must be the first argument</param>
  /// <param name="message">Additional parameters can be specified and will be passed to methods that accept it as an argument.</param>
  /// <returns>If it returns non-null it will only take methods that return</returns>
  [GeneralExceptionHandler]
  public partial int HandleException(Exception ex, string message);
                 
  /// <summary>
  /// Handles a single exception type matching the argument.
  /// </summary>
  /// <param name="ex">The exception type. Used in the type check.</param>
  /// <param name="message">Passed along by the generator</param>
  [HandlesException]
  private static int HandleSingle(ArgumentNullException ex, string message) {
    return 4;
  }
           
  /// <summary>
  /// Handles multiple exception types as specified in the header.
  /// </summary>
  /// <param name="ex">The exception type. Must be common between the types. Will be cast if necessary.</param>
  [HandlesException(typeof(NullReferenceException), typeof(ArithmeticException))]
  private static int HandleMultiple(Exception ex) {
    return 5;
  }
  
  /// <summary>
  /// Fallback handler if none of the other methods work.
  /// </summary>
  /// <param name="ex">The exception type. Should match the general handler.</param>
  [FallbackExceptionHandler]
  private static int HandleFallback(Exception ex) {
    return 6;
  }
}
```
