#### [MyHome](https://meng-lanhome.github.io/MyCode/)
------------
- Code
```
Private filePaths() As String 
Sub demo()
    Dim pptApp As PowerPoint.Application, pptPres As PowerPoint.Presentation, pptShp As PowerPoint.Shape, sld As PowerPoint.Slide
    Dim WordDoc As Word.Document
    Set WordDoc = Word.ActiveDocument
    ListFilePaths ("C:\Users\menglan\Desktop\tools\office\Demo\PPT")
    Set pptApp = New PowerPoint.Application
    pptApp.Visible = msoTrue
    Set pptPres = pptApp.Presentations.Open(filePaths(0))
    n = 1
    For Each sld In pptPres.Slides
        With sld
            .Select
            For Each pptShp In .Shapes
                If pptShp.Type = msoChart Then
                    pptShp.Copy
                    WordDoc.Activate
                    Selection.PasteSpecial Placement:=0, DataType:=wdPasteMetafilePicture
                    If n = 2 Then
                        n = 1
                        Selection.EndOf Unit:=wdStory
                        Selection.TypeParagraph
                    Else
                        n = n + 1
                    End If
                End If
            Next
        End With
    Next
    With WordDoc
        For i = 1 To .InlineShapes.Count
            With .InlineShapes(i)
                .LockAspectRatio = msoFalse
                .Height = CentimetersToPoints(5)
                .Width = CentimetersToPoints(8)
            End With
        Next
    End With
    pptPres.Close
    pptApp.Quit
    Set pptPres = Nothing
    Set pptApp = Nothing
End Sub
Function ListFilePaths(folderPath)
    Dim fso As Object 
    Dim folder As Object 
    Dim file As Object 
    
    Dim i As Long 
    Set fso = CreateObject("Scripting.FileSystemObject")
    Set folder = fso.GetFolder(folderPath)
    ReDim filePaths(folder.Files.Count - 1)
    i = 0
    For Each file In folder.Files
        filePaths(i) = file.Path 
        i = i + 1
    Next file
End Function
```
