# APL Ride Client

## What
The APL Ride Client implements the RIDE protocol in APL and makes it easy to launch a separate APL process and control it with simple commands.

## Why

Remote controlling an APL process via RIDE allows for mocking user interaction with the APL interpreter, such as:
* executing expressions in the session
* creating and changing items using the editor
* tracing, skipping lines, resuming execution
* anything else allowed for in the RIDE protocol

This makes it an ideal framework for writing scripted tests to validate the behaviour of the interpreter or development tools.

## How

Execute a simple expression in the session and collect the output:
```apl
      c←⎕NEW Ride.Client
      c.Start
      c.Execute'?3 3⍴100'
      ⎕←c.WaitForSessionOutput 1000
 5  79  8                     
46  72 25                     
21 100 32                     
```

Define a function:
```apl
      c.Edit'foo' ('r←foo' 'r←42')
      c.Execute'foo'
      ⎕←c.WaitForSessionOutput 1000  
42 
```

Define a class:
```apl
      c.Edit'○ MyClass'(':Class MyClass' ':Field Public Shared ID←12' ':EndClass') 
      c.Execute'↑⎕SRC MyClass'
      ⎕←c.WaitForSessionOutput 1000
:Class MyClass                                                                   
:Field Public Shared ID←12                                                       
:EndClass                                                                        
      c.Execute'MyClass.ID' 
      ⎕←c.WaitForSessionOutput 1000
12 
```

Run multiple clients simultaneously:
```apl
      cs←⎕NEW¨10⍴Ride.Client
      cs.Start               
      cs.Execute⊂'?⍳10'       
      r←cs.WaitForSessionOutput 1000
```