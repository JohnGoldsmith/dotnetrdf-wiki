[[Home]] > [[User Guide|UserGuide]] > [[UserGuide/Tools|Tools]] > [[UserGuide/Tools/rdfEditor|rdfEditor]] > [[UserGuide/Tools/rdfEditor/Advanced Settings|Advanced Settings]]

# rdfEditor Advanced Settings 

As seen in the main page for [[UserGuide/Tools/rdfEditor|rdfEditor]] you have significant control over the display of RDF in the editor:

{{http://www.dotnetrdf.org/images/screenshots/editor_appearance.jpg|rdfEditor - Appearance Settings}}

But sometimes this may not be enough since you may want more detailed control over syntax highlighting. To do this follow this guide:

## Step 1 - Edit Configuration File 

Locate the Directory where rdfEditor and the rest of the Tools are installed. You should see a file named ##rdfEditor.exe.config##, open it and look for the following section:

{{{
#!xml
    <applicationSettings>
        <VDS.RDF.Utilities.Editor.Properties.Settings>
            <setting name="UseCustomisedXshdFiles" serializeAs="String">
                <value>False</value>
            </setting>
        </VDS.RDF.Utilities.Editor.Properties.Settings>
    </applicationSettings>
```

Change the value ##False## to be ##True## and save the File

## Step 2 - Edit Highlighting Definitions 

Under the directory where you found the Configuration file should be a subdirectory called ##Syntax/## in which you will find a number of files with a ##.xshd## extension. These are the highlighting definitions used by the AvalonEdit component which powers the editor, to learn how to edit them we recommend looking at this [[http://www.codeproject.com/KB/edit/AvalonEdit.aspx|CodeProject]] article.

**Warning:** If you edit a file such that it is invalid that highlighting will stop working in the editor. You can set the setting back to ##False## to cause the editor to use the in-built default copies of the syntax highlighting definitions.