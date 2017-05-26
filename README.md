# GoLazarus
Plugin for lazarus to use GoLang in apps, via simple IPC lib. Example: link up a button event to call go code instead of fpc object pascal code. User will not even need to realize it is IPC in order to code golang algorithms in Lazarus. A button will be linked up to a go function instead of an fpc function; the go code will be located in a go package file.

For now, some notes:

Extend the Lazarus IDE with code tools and lazideintf
Add golang compilation messages using:

'''unit IDEMsgIntf;
...
var Dir: String;
begin
  Dir:=GetCurrentDir;
  IDEMessagesWindow.BeginBlock;
  IDEMessagesWindow.AddMsg('gocode.go(30,4) Error: some go compile error here',Dir,0);
  IDEMessagesWindow.EndBlock;
end;'''
