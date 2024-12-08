# Testbench 

**Testbench** is a lightweight library designed to provide a unified interface for various PLC unittest framework implementations. It acts as a bridge, allowing developers to write consistent test logic while supporting multiple underlying test frameworks. It reduces the overhead to write tests and therefore lower the hurdle for writing unittests, to a bare minimum!

The library can be used together with **Zeugwerk Creator's Test Explorer** for a seamless test integration into TwinCAT'S IDEs (TcXaeShell, Visual Studio). There, [TcUnit](https://tcunit.org/) is used as a unit-test backend.

## Key Features  
- Framework-agnostic interfaces for test implementation.  
- Designed to support extensibility for new test frameworks.  
- Ideal for use with tools like Zeugwerk Test Explorer.
- Bare minimum code overhead when implementing tests.
- Support for code generators for dynamically created unittests.

---

## Write Unittests  

1. Add Testbench to your PLC by including it as a dependency. The simplest way todo this is by using [Twinpack](https://github.com/Zeugwerk/Twinpack) Package Manager, alternatively you can grab the [latest release](https://github.com/Zeugwerk/Testbench/releases/latest) from GitHub and install it manually in the TwinCAT XAE).
1. Testsuites are function blocks implementing [IUnitTest](xref:Testbench.IUnitTest)
     ``` st  
     FUNCTION_BLOCK <POU>Test EXTENDS <POU> IMPLEMENTS Testbench.IUnitTest>
     ```
     
     By extending testsuites from the object that is testes, the testsuite gets access protected methods of the POU.
1. Tests are implemented as method for each testsuite. Methods, which are considered unittests follow the following signature.
     ``` st
     METHOD Test_<NAME OF THE TEST>
     VAR_INPUT
       assertions : Testbench.IAssertions;
     END_VAR
     ```

     where `assertions` gives access to a large variety of checks (`EqualsDint`, `IsTrue`, `EqualsArray2dLreal`, ...).

     Alternatively, for more simple tests with only 1 assertion
     ``` st
     METHOD Test_<NAME OF THE TEST>
     VAR_OUTPUT
       expected : <TYPE>;
       actual : <TYPE>;
       message : STRING(255);
     END_VAR
     ```
     where <TYPE> can be any IEC61131-3 compatible type like `DINT`, `INT`, `WORD` or `BOOL`

1. Test may be parametrized by using the `DataRow` attribute. When generating the unittest, every datarow will be its own test. The signature for using paramtrized tests looks as follows.
     ``` st
     {attribute 'DataRow(<PARAMETER_1 VALUE>, <PARAMETER_2 VALUE>, ..., <PARAMETER_N VALUE>)'}
     {attribute 'DataRow(<PARAMETER_1 VALUE>, <PARAMETER_2 VALUE>, ..., <PARAMETER_N VALUE>)'}
     // ...
     {attribute 'DataRow(<PARAMETER_1 VALUE>, <PARAMETER_2 VALUE>, ..., <PARAMETER_N VALUE>)'}
     METHOD Test_<NAME OF THE TEST>
     VAR_INPUT
       assertions : IAssertions;
       <PARAMETER_1> : <PARAMETER_1 DATATYPE>
       <PARAMETER_2> : <PARAMETER_2 DATATYPE>
       // ...
       <PARAMETER_N> : <PARAMETER_N DATATYPE>
     END_VAR
     ```

## Execute Unittests

Testbench is designed to integrate seamlessly with **Zeugwerk Test Explorer**. By implementing Testbench’s interfaces, you can enable structured test discovery and execution directly in your explorer tool.

(more to come)
