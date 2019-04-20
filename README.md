# PythonOniLibrary
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

The following example calls a python 3 script named: helloworld.py from the /python directory and passes the program up to 20 parameters individually via the PYRUN CL command. 

 PYRUN SCRIPTDIR('/python')          
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
      PARM11(p11)                           
      PARM12(p12)                           
      PARM13(p13)                           
      PARM14(p14)        
      PARM15(p15)        
      PARM16(p16)        
      PARM17(p17)        
      PARM18(p18)        
      PARM19(p19)        
      PARM20(p20)        
      PYPATH(*DEFAULT)   
      CCSID(37)          
      DSPSTDOUT(*YES)    
      LOGSTDOUT(*NO)     
      PRTSTDOUT(*NO)     
      DLTSTDOUT(*YES)    

# PYRUN command parms

**SCRIPTDIR** - The IFS directory location for the Python script.

**SCRIPTFILE** - The script file you want to call without the directory path. **Ex: hello.py**

**PYVERSION** - The Python version shoud be set to either **2** for Python 2 or **3** for Python 3.

**PARM01 - PARM20** - Command line parameters. Up to 20 args can be passed to a Pythone script call. Do NOT put double quotes around parms or your program call may get errors because your parameters get compromised with extra double quotes. Double quotes are added automatically inside the CL command processing program. Single quotes are allowed around your parmaeter data:  Ex: **'My Parm Value 1' 'My Parm Value 2'**

**PYPATH** - The this is the directory path to your Python binaries (python/python3). Hopefully you have already installed the Yum versions so the default path should be good. Leave value set to `*DEFAULT`. **Default= /QOpenSys/pkgs/bin**

**CCSID** - When using the iToolkit I originally had some issues with CL commands not working correctly. Don't remember exactly why. This may have been solved, however I recommend still passing 37 unless you are in a non US country. If you set to `*SAME`, the CCSID will be the same as your current job with no change.

**DSPSTDOUT** - Display the outfile contents. Nice when debigging. 

**LOGSTDOUT** - Place STDOUT log entries into the current jobs job log. Use this if you want the log info in the IBM i joblog.

**PRTSTDOUT** - Print STDOUT to a spool file. Use this if you want a spool file of the log output.

**DLTSTDOUT** - This option insures that the STDOUT IFS temp files get cleaned up after processing. All IFS log files get created in the /tmp/mono directory.

