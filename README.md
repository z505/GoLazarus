# GoLazarus
Plugin for lazarus to use GoLang in Lazarus apps, via simple IPC lib. Create visual apps using lazarus, but then run Go code from the events fired by the widgets. Example: link up a button event to call go code instead of fpc object pascal code. User will not even need to realize it is IPC in order to code golang algorithms in Lazarus. A button will be linked up to a go function instead of an fpc function; the go code will be located in a go package file.

Project status: just started, and will just be a prototype to prove the concept will work, then more work could follow later to make it more than just a prototype.

For now, some notes:

Extend the Lazarus IDE with code tools and lazideintf

In lazideintf is the ability to detect compilation:
* LazarusIDE.AddHandlerOnProjectBuildingFinished(@MyEvent);
* LazarusIDE.AddHandlerOnLazarusBuildingFinished(@MyEvent);

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
