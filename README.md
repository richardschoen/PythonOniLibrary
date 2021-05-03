# PythonOniLibrary
**NOTE:** PYRUN command functionality has been merged in to the QSHPYRUN command that is part of the QShell on i command set. The PYRUN command here will not be enhanced, however QSHPYRUN will continue to be enhanced any further. You can access the QShell on i project site here:

https://github.com/richardschoen/QShOni

This IBM i library contains useful CL wrapper commands to allow Python to be called and consumed from regular IBM i jobs written in CL, RPG or COBOL. After calls are made to Python scripts, the caller can get return results (STDOUT) from the applications in one of the following formats: IFS file, OUTFILE, job log entries or spool file. 

The main benefit of this wrapper is to be able to integrate Python applications on-the-fly with standard IBM i job streams.

# Installing PYONI library and creating PYRUN command objects

Download the **pyoni.savf** save file from the selected releases page. 

https://github.com/richardschoen/PythonOniLibrary/releases

Upload the **pyoni.savf** to the IFS and place it in **/tmp/pyoni.savf**

Run the following commands to copy the save file from the IFS into a SAVF object

`CRTSAVF FILE(QGPL/PYONI)`
 
`CPYFRMSTMF FROMSTMF('/tmp/pyoni.savf') TOMBR('/qsys.lib/qgpl.lib/pyoni.file') MBROPT(*REPLACE) CVTDTA(*NONE)`

Restore the PYONI library

`RSTLIB SAVLIB(PYONI) DEV(*SAVF) SAVF(QGPL/PYONI)`

Build the PYONI commands

`ADDLIBLE PYONI`

`CRTCLPGM PGM(PYONI/SRCBLDC) SRCFILE(PYONI/SOURCE) SRCMBR(SRCBLDC) REPLACE(*YES)`

`CALL PGM(PYONI/SRCBLDC)`

# Using the PYRUN CL command to call a Python application

The following example calls a python 3 script named: helloworld.py from the /python directory and passes the program up to 40 parameters individually via the PYRUN CL command. 

 ```
      PYRUN SCRIPTDIR('/python')          
      SCRIPTFILE(helloworld.py)            
      PYVERSION(3)
      ARGS(p1val p2val p3val p4val p5val p6val p7val)                          
      PYPATH(*DEFAULT)   
      CCSID(37)          
      DSPSTDOUT(*YES)    
      LOGSTDOUT(*NO)     
      PRTSTDOUT(*NO)     
      DLTSTDOUT(*YES)   
```      

# PYRUN command parms

**SCRIPTDIR** - The IFS directory location for the Python script. **Ex: /python**

**SCRIPTFILE** - The script file name you want to call without the directory path. The PYRUN command puts it all together. **Ex: hello.py**

**PYVERSION** - The Python version you want to use. It should be set to either **2** for Python 2 or **3** for Python 3.

**ARGS** - Command line parameter argument list. Up to 40 - 200 byte argument/parameter values can be passed to a Python script call. Each parm is automatically trimmed. Do NOT put double quotes around the parms or your program call may get errors because your parameters get compromised with extra double quotes. The double quotes are already added automatically inside the CL command processing program. Single quotes are allowed around your parmaeter data though if desired:  Ex: **'My Parm Value 1' 'My Parm Value 2'**

**PYPATH** - The this is the directory path to your Python binaries (python/python3). Hopefully you have already installed the Yum versions so the default path should be good. Leave value set to `*DEFAULT`. **Default= /QOpenSys/pkgs/bin**. The default path lives in the **PYPATH** data area in the **PYONI** library.

**CCSID** - When using the iToolkit component for command access, I originally had some issues with CL commands not working correctly. However I don't currently remember exactly why. This may have been solved, however I recommend still passing a value of 37 unless you are in a non US country. If you set to `*SAME`, the CCSID will stay the same as your current job with no change.

**DSPSTDOUT** - Display the outfile contents. Nice when debugging. 

**LOGSTDOUT** - Place STDOUT log entries into the current jobs job log. Use this if you want the log info in the IBM i joblog.

**PRTSTDOUT** - Print STDOUT to a spool file. Use this if you want a spool file of the log output.

**DLTSTDOUT** - This option insures that the STDOUT IFS temp files get cleaned up after processing. All IFS log files get created in the /tmp/mono directory.

# Using the PYRUN2 CL command to call a Python application passing individual named parms. 

The following example calls a python 3 script named: helloworld.py from the /python directory and passes the program up to 20 parameters individually via the PYRUN CL command. (This was the original PYRUN command left in for educational purposes.) 

 ```
      PYRUN2 SCRIPTDIR('/python')          
      SCRIPTFILE(helloworld.py)            
      PYVERSION(3)                          
      PARM01(p1)                            
      PARM02(p2)                            
      PARM03(p3)                            
      PARM04(p4)                            
      PARM05(p5)                            
      PARM06(p6)                            
      PARM07(p7)                            
      PARM08(p8)                            
      PARM09(p9)                            
      PARM10(p10)                           
      PYPATH(*DEFAULT)   
      CCSID(37)          
      DSPSTDOUT(*YES)    
      LOGSTDOUT(*NO)     
      PRTSTDOUT(*NO)     
      DLTSTDOUT(*YES)   
```      

# PYRUN2 command parms

**SCRIPTDIR** - The IFS directory location for the Python script. **Ex: /python**

**SCRIPTFILE** - The script file name you want to call without the directory path. The PYRUN command puts it all together. **Ex: hello.py**

**PYVERSION** - The Python version you want to use. It should be set to either **2** for Python 2 or **3** for Python 3.

**PARM01 - PARM20** - Command line parameters. Up to 20 args can be passed to a Python script call. Do NOT put double quotes around parms or your program call may get errors because your parameters get compromised with extra double quotes. Normally double quotes are added automatically inside the CL command processing program. Single quotes are allowed around your parmaeter data:  Ex: **'My Parm Value 1' 'My Parm Value 2'**

**PYPATH** - The this is the directory path to your Python binaries (python/python3). Hopefully you have already installed the Yum versions so the default path should be good. Leave value set to `*DEFAULT`. **Default= /QOpenSys/pkgs/bin**. The default path lives in the **PYPATH** data area in the **PYONI** library.

**CCSID** - When using the iToolkit component for command access, I originally had some issues with CL commands not working correctly. However I don't currently remember exactly why. This may have been solved, however I recommend still passing a value of 37 unless you are in a non US country. If you set to `*SAME`, the CCSID will stay the same as your current job with no change.

**DSPSTDOUT** - Display the outfile contents. Nice when debigging. 

**LOGSTDOUT** - Place STDOUT log entries into the current jobs job log. Use this if you want the log info in the IBM i joblog.

**PRTSTDOUT** - Print STDOUT to a spool file. Use this if you want a spool file of the log output.

**DLTSTDOUT** - This option insures that the STDOUT IFS temp files get cleaned up after processing. All IFS log files get created in the /tmp/mono directory.


# helloword.py sample Python script to echo parms and test PYRUN if you need one

Enter the following statements into a Python file in the IFS. Name it **helloworld.py**

```
#Hello World Python script to echo parms
#!/usr/bin/python                                      
                                                       
import sys                                             
                                                       
#Echo back the parameters                              
print (sys.argv[1:]);                                  
```
