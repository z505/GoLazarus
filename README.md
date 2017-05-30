# GoLazarus
Plugin for lazarus to use GoLang in Lazarus apps, via simple IPC lib. Create visual apps using lazarus, but then run Go code from the events fired by the widgets. Example: link up a button event to call go code instead of fpc object pascal code. User will not even need to realize it is IPC in order to code golang algorithms in Lazarus. A button will be linked up to a go function instead of an fpc function; the go code will be located in a go package file.

Project status: just started, and will just be a prototype to prove the concept will work, then more work could follow later to make it more than just a prototype.

## For now, some notes

How to compile a go exe? Idea: compile go code first, then allow lazarus to build the project after go code is compiled. When user hits F9 or compile button, AddHandlerOnLazarusBuilding should be the way to make an event that is fired to first compile go code. Then add go compiler messages to the status window using IDEMsgIntf unit.  Figure out how to make the IDE open a go file if there is a go compilation error in the status window.. go compiler message format is slightly different for line number location of the error. FPC is i.e (1,10)

Extend the Lazarus IDE with Codetools, IDEMsgIntf, and Lazideintf units

In lazideintf is the ability to detect compilation of fpc code (before, after):
* after compile: LazarusIDE.AddHandlerOnProjectBuildingFinished(@MyEvent);
* ide after build: LazarusIDE.AddHandlerOnLazarusBuildingFinished(@MyEvent);
* before compile: AddHandlerOnLazarusBuilding
* lihtProjectBuilding, lihtProjectBuildingFinished is a TLazarusIDEHandlerType

Add golang compilation messages using:
```
unit IDEMsgIntf;
...
var Dir: String;
begin
  Dir:=GetCurrentDir;
  IDEMessagesWindow.BeginBlock;
  IDEMessagesWindow.AddMsg('gocode.go(30,4) Error: some go compile error here',Dir,0);
  IDEMessagesWindow.EndBlock;
end;
```
Add source code to a fpc source file, to call a procedure that then sends a message to the go executable:
```
CodeToolBoss.InsertStatements();
```
There may be another way without using CodeToolBoss, in intf related units. Such as:
* Building lazarus with -dEnableCodeCompleteTemplates
* then inserting code that way between begin/end

## Hooking the Events for widgets in the IDE
Need a way to hook into the IDE which can insert code every time an event is added through double clicking the widget or double clicking the event in the object inspector. Related code in Lazarus source code when this happens:
* ValueEditDblClick, ToggleRow, DoCallEdit, in objectinspector.pp
* .RefreshValueEdit (but not called when widget double clicked)
* form widget double clicked: ???
