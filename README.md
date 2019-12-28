# Web-Wallpaper
This code allows you to set a webbrowser as wallpaper in Windows 10. You will only require to add a webbrowser to your form.
You can use any window Handle as wallpaper.


Declarations
```vb.net
       <DllImport("user32.dll", SetLastError:=True, CharSet:=CharSet.Auto)>
    Public Shared Function SendMessageTimeout(ByVal windowHandle As IntPtr, ByVal Msg As UInteger, ByVal wParam As IntPtr, ByVal lParam As IntPtr, ByVal flags As SendMessageTimeoutFlags, ByVal timeout As UInteger, <System.Runtime.InteropServices.Out()> ByRef result As IntPtr) As IntPtr
    End Function

    <DllImport("user32.dll")>
    Public Shared Function EnumWindows(ByVal lpEnumFunc As EnumWindowsProc, ByVal lParam As IntPtr) As <MarshalAs(UnmanagedType.Bool)> Boolean
    End Function

    Public Delegate Function EnumWindowsProc(ByVal hwnd As IntPtr, ByVal lParam As IntPtr) As Boolean

    <DllImport("user32.dll", SetLastError:=True)>
    Public Shared Function FindWindow(ByVal lpClassName As String, ByVal lpWindowName As String) As IntPtr
    End Function

    <DllImport("user32.dll", SetLastError:=True)>
    Public Shared Function FindWindowEx(ByVal parentHandle As IntPtr, ByVal childAfter As IntPtr, ByVal className As String, ByVal windowTitle As IntPtr) As IntPtr
    End Function

    <DllImport("user32.dll", SetLastError:=True)>
    Public Shared Function SetParent(ByVal hWndChild As IntPtr, ByVal hWndNewParent As IntPtr) As IntPtr
    End Function

    <Flags>
    Public Enum SendMessageTimeoutFlags As UInteger
        SMTO_NORMAL = &H0
        SMTO_BLOCK = &H1
        SMTO_ABORTIFHUNG = &H2
        SMTO_NOTIMEOUTIFNOTHUNG = &H8
        SMTO_ERRORONEXIT = &H20
    End Enum
```

Code
```vb.net
    Dim progman As IntPtr = FindWindow("Progman", Nothing)
        Dim result As IntPtr = IntPtr.Zero

        SendMessageTimeout(progman, &H52C, New IntPtr(0), IntPtr.Zero, SendMessageTimeoutFlags.SMTO_NORMAL, 1000, result)

        Dim workerw As IntPtr = IntPtr.Zero

        EnumWindows(New EnumWindowsProc(Function(tophandle, topparamhandle)
           Dim p As IntPtr = FindWindowEx(tophandle, IntPtr.Zero, "SHELLDLL_DefView", IntPtr.Zero)

           If p <> IntPtr.Zero Then
             workerw = FindWindowEx(IntPtr.Zero, tophandle, "WorkerW", IntPtr.Zero)
           End If

           Return True
        End Function), IntPtr.Zero)


        WebBrowser1.Left = 0
        WebBrowser1.Top = 0

        WebBrowser1.Navigate("https://chrisandriessen.nl/web/bg.html")

        WebBrowser1.Size = New Size(1920, 1080)

        SetParent(WebBrowser1.Handle, workerw)
```
