<div align="center">

## Save and Load Objects from XML Files


</div>

### Description

This example illustrates how to save objects to XML files, and load objects from XML files, via serialization.
 
### More Info
 
Sample output:

Original values in XML file C:\Documents and Settings\Administrator\Local Settings\Temp\tmp45A5.tmp:

name = Cindy, age = 45, happy = True, color = (red=0, green=0, blue=0, alpha=0)

name = Kevin, age = 44, happy = True, color = (red=0, green=0, blue=0, alpha=255)

name = David, age = 18, happy = True, color = (red=255, green=0, blue=0, alpha=128)

Loaded values from XML file C:\Documents and Settings\Administrator\Local Settings\Temp\tmp45A5.tmp:

name = Cindy, age = 45, happy = True, color = (red=0, green=0, blue=0, alpha=0)

name = Kevin, age = 44, happy = True, color = (red=0, green=0, blue=0, alpha=255)

name = David, age = 18, happy = True, color = (red=255, green=0, blue=0, alpha=128)

Contents of XML file:

&lt;?xml version="1.0" encoding="utf-8"?&gt;

&lt;ArrayOfCustomer xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema"&gt;

&lt;Customer&gt;

&lt;name&gt;Cindy&lt;/name&gt;

&lt;age&gt;45&lt;/age&gt;

&lt;happy&gt;true&lt;/happy&gt;

&lt;color&gt;

&lt;red&gt;0&lt;/red&gt;

&lt;green&gt;0&lt;/green&gt;

&lt;blue&gt;0&lt;/blue&gt;

&lt;alpha&gt;0&lt;/alpha&gt;

&lt;/color&gt;

&lt;/Customer&gt;

&lt;Customer&gt;

&lt;name&gt;Kevin&lt;/name&gt;

&lt;age&gt;44&lt;/age&gt;

&lt;happy&gt;true&lt;/happy&gt;

&lt;color&gt;

&lt;red&gt;0&lt;/red&gt;

&lt;green&gt;0&lt;/green&gt;

&lt;blue&gt;0&lt;/blue&gt;

&lt;alpha&gt;255&lt;/alpha&gt;

&lt;/color&gt;

&lt;/Customer&gt;

&lt;Customer&gt;

&lt;name&gt;David&lt;/name&gt;

&lt;age&gt;18&lt;/age&gt;

&lt;happy&gt;true&lt;/happy&gt;

&lt;color&gt;

&lt;red&gt;255&lt;/red&gt;

&lt;green&gt;0&lt;/green&gt;

&lt;blue&gt;0&lt;/blue&gt;

&lt;alpha&gt;128&lt;/alpha&gt;

&lt;/color&gt;

&lt;/Customer&gt;

&lt;/ArrayOfCustomer&gt;


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Kamilche](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/kamilche.md)
**Level**          |Beginner
**User Rating**    |3.7 (11 globes from 3 users)
**Compatibility**  |VB\.NET
**Category**       |[Data Structures](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/data-structures__10-8.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/kamilche-save-and-load-objects-from-xml-files__10-6479/archive/master.zip)





### Source Code

```
Imports System.Xml.Serialization
Imports System.IO
Public Class Form1
  Private Sub Form1_Load(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MyBase.Load
    ' Put a breakpoint on the next line and step through it.
    Test()
  End Sub
  Public Sub Test()
    Dim filename As String = My.Computer.FileSystem.GetTempFileName()
    Dim lst As List(Of Customer)
    Dim lst2 As List(Of Customer)
    ' Create list of objects
    lst = New List(Of Customer)
    lst.Add(New Customer("Cindy", 45, True, New MyColor(0, 0, 0, 0)))
    lst.Add(New Customer("Kevin", 44, True, New MyColor(0, 0, 0, 255)))
    lst.Add(New Customer("David", 18, True, New MyColor(255, 0, 0, 128)))
    ' Print original values
    Debug.Print(vbCrLf & "Original values in XML file " & filename & ":")
    For Each item As Customer In lst
      Debug.Print("  " & item.ToString)
    Next
    ' Save to XML file
    SaveObjectToXML(Of List(Of Customer))(lst, filename)
    ' Load from XML file
    lst2 = LoadObjectFromXML(Of List(Of Customer))(filename)
    ' Print loaded values
    Debug.Print(vbCrLf & "Loaded values from XML file " & filename & ":")
    For Each item As Customer In lst2
      Debug.Print("  " & item.ToString)
    Next
    ' Print XML file
    Debug.Print(vbCrLf & "Contents of XML file:")
    Debug.Print(File.ReadAllText(filename))
  End Sub
End Class
Public Module modMain
  Public Function SaveObjectToXML(Of T)(ByVal obj As Object, ByVal filename As String) As Boolean
    ' Save an object to XML.
    ' Usage: SaveObjectToXML(Of Project)(proj, filename)
    Dim sw As StreamWriter = New StreamWriter(filename)
    Dim xmls As XmlSerializer = Nothing
    Try
      xmls = New XmlSerializer(GetType(T))
      xmls.Serialize(sw, obj)
    Catch ex As Exception
      Debug.Print(ex.ToString)
      MsgBox(ex.ToString)
      Return False
    End Try
    sw.Flush()
    sw.Close()
    Return True
  End Function
  Public Function LoadObjectFromXML(Of T)(ByVal filename As String) As T
    ' Load an object from XML.
    ' Usage: proj = LoadObjectFromXML(Of Project)(filename)
    Dim sr As StreamReader = New StreamReader(filename)
    Dim xmls As XmlSerializer = Nothing
    Dim obj As T
    Try
      xmls = New XmlSerializer(GetType(T))
      obj = CType(xmls.Deserialize(sr), T)
    Catch ex As Exception
      Debug.Print(ex.ToString)
      MsgBox(ex.ToString)
      Return Nothing
    End Try
    sr.Close()
    Return obj
  End Function
End Module
' Classes must be public and reside outside any modules to be serializable
<Serializable()> _
Public Class Customer
  Public name As String
  Public age As Integer
  Public happy As Boolean
  Public color As MyColor
  Public Sub New(ByVal name As String, ByVal age As Integer, ByVal happy As Boolean, ByVal color As MyColor)
    Me.name = name
    Me.age = age
    Me.happy = happy
    Me.color = color
  End Sub
  Public Sub New()
    ' Must have a parameterless constructor to work
  End Sub
  Public Overrides Function ToString() As String
    Return "name = " & name & ", age = " & age & ", happy = " & happy & ", color = " & color.ToString
  End Function
End Class
Public Class MyColor
  Public red As Byte
  Public green As Byte
  Public blue As Byte
  Public alpha As Byte
  Public Sub New(ByVal red As Byte, ByVal green As Byte, ByVal blue As Byte, ByVal alpha As Byte)
    Me.red = red
    Me.green = green
    Me.blue = blue
    Me.alpha = alpha
  End Sub
  Public Sub New()
    'necessary for seralization
  End Sub
  Public Overrides Function ToString() As String
    Return "(red=" & red & ", green=" & green & ", blue=" & blue & ", alpha=" & alpha & ")"
  End Function
End Class
```

