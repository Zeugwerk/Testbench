# Testbench 

**Testbench** is a lightweight library designed to provide a unified interface for various PLC unittest framework implementations. It acts as a bridge, allowing developers to write consistent test logic while supporting multiple underlying test frameworks. It reduces the overhead to write tests and therefore lower the hurdle for writing unittests, to a bare minimum!

![image](https://github.com/user-attachments/assets/de76a850-9398-492f-8804-32fb175415ad)


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
     where `<TYPE>` can be any IEC61131-3 compatible type like `DINT`, `INT`, `WORD` or `BOOL`

     ![image](https://github.com/user-attachments/assets/be609a50-3da9-4bbe-9cdc-f76d09038e1d)


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

- Implement tests as described in the section above
- Navigate to `Tools > Zeugwerk Creator > Test Explorer`
- In the new pane, click on `Run All Tests`, this will
  - Compile your PLC and check all objects
  - Install it as a local library
  - In the background create a new PLC containing the code generation for all tests (`tests/Tests.sln`)
  - Compile the Testing PLC
  - Activate it on a target
 
You can check the **Examples PLC** in this repository for a demonstration
 
![image](https://github.com/user-attachments/assets/2887a596-f8b9-4f8c-ab2c-1a7c11f6780f)
![image](https://github.com/user-attachments/assets/76f94b76-ba20-4fdd-bb00-144cf181ee70)


