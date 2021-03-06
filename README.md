PCom Package

A set of routines that allows specifying areas to print and defines the following entities: 

• Defaults for the printing

• Page Headers and Page Footers, which are printed on every page

• Tables, which have headers and one or more columns, Each column consists of objects
   Table attributes are inherited by Columns and Objects
   The headers are printed before any table objects and are printed on every page where table objects are printed
   
• Columns, which can have headers, and attributes which are be inherited by objects in the columns
  Column attributes are inherited by Objects in the column, so for example if a column has "Border:
  
• Rows in the Tables, one row for each column
 
• Objects, which are printed in columns in tables. 

• Lines. Each object is split into lines to be printed. Whether an object can be split 
   onto different pages is controlled with an attribute. If the object is too large to fit on an page
   it is automatically split onto separate pages



Typical use would be to call

  PCom_InitClear                 to initialize
  
  PCom_PageHeader_DefineText     to define page headers

  PCom_PageFooter_DefineText     to define page footers
  
 then define tables, columns, and objects
 
 then call PCom_Flush to flush the buffered objects
 
 

================================================================================

Like 4D, all keywords are case-insensitive

================================================================================


Objects can contain 4D HTML tags which are expanded at the time of printing and can
  reference the following variables:
    l_PCom_PageNum
    t_PCom_Continued which will contain either "" or " (continued)" if the printing of the object continues from another page
    
  Example: PCom_PageFooter_DefineText ("Page <!--#4DTEXT l_PCom_PageNum -->";"Position:Line 1";"Align:Right";"Style:Bold";"Font-size:10")

================================================================================
  
Attibrutes that can be used in defaults, page headers, page footers, tables, table headers, columns,
  column headers, and objects. The value of an attribute is delimited by a colon, such as "Multi-Style:yes"
  
  Supported attributes are: 
  
  "Multi-Style" can have values of "yes" or "no" 
  This attributes specifies whether to interpret text as having multi-style tags. If used, tags in the text can
  override styles, fonts, sizes specified in the attributes
  
  "Align", Can have values of "Left", "Right" or "Center"

  "Multiline" which means that the line will be wrapped
  
  "Single Line" which means that only a single line will be printed, even if the object
     needs multiple lines to fully print
     
  "Non-truncated" which means that the full width of the page, from the left of the column position
    to the right side of the page, is used to print the object
    
  "Font-Size" which specifies font size and is in the range of (1-128)
  
  "Font" which specifies font 
    If the font is not present in system, defaults to Times
    
  "Style" which can be "Bold", "Plain", "Underline", "Italic"

  "Position" which can be "next" or "Line x", such as "line 2"
    This is meaningful only in the context of a Page Header or a Page Footer
    For page footers, line 1 is the bottom line of the page, line 2 is the next line up, etc.
  
  "Spacing" which can be "single", "double", or a decimal such as "1.2"
  
  "Margin-top" or "Margin-bottom" or "Margin-left" or "Margin-right" or "Margin"
    Margin is the space in pixels outside the border
    Values supported are 0, 1, 2, 3, 4
  
  "Margin-bottom"
  
  "Margin-left"
   
  "Margin-right"
  
  "Margin"
     Allows specifying all 4 margins with one statement
  
  "Border-top" 
    Border is a line around an object, column or table
    Values supported are  0, 1, 2, 3, 4
  
  "Border-left" 
  
  "Border-right" 
  
  "Border-bottom" 
  
  "Border" 
    Allows specifying all 4 borders with one statement
    

  "Padding-top"
    Padding is the space in pixels between the border and the object text/picture
    Values supported are 0, 1, 2, 3, 4
  
  "Padding-bottom"
  
  "Padding-left"
  
  "Padding-right"
  
  "Padding" 
    Allows specifying all 4 Paddings with one statement
  

  "Width" which can be "adaptive" or a percentage or number of pixels, 
     If percentage, is a percentage of the enclosing structure. For tables, the enclosing structure is the page. For
     columns, the enclosing structure is the table. 
     Examples: "30 %" or "80 px" or "adaptive"
     This attribute is meaningful only for columns and tables
     Adaptive is only supportive for columns. The widest object in the column will determine the width of the column

     
  "Left" specifies the left edge of the table, can be px (for pixels), or a percentage of page width
    Note: not used for columns, only widths are used for columns
    
  
  "Right" specifies the right edge of the table, can be px (for pixels), or a percentage of page width
      Note: not used for columns, only widths are used for columns
  	
  "Print-After-Table"
     specifies that the table being defined will be printed after another table is completely printed
     This attribute is only meaningful for Tables

  "Don't-split" specifies that if object won't be split between different pages 
     Values are "yes" and "no"
     (of course, if it too tall to fit on a page, even if there is nothing else on the page, it is split)
  
    
  "Starting-Page-Num" specifies the starting page count of the first page to be printed
  
    
================================================================================

Routines

  PCom_InitClear { ( force-clear-flag { ; attribute list ) }
    Can be called to clear variables and set defaults for all subsequent tables, columns and objects
    Can be called again to clear tables and columns and reset preferences and page count
    
    
  PCom_PageHeader_DefineText ( text {; attribute list } )
    Defines objects that will be printed as a page header on every page printed
    
    
  PCom_PageFooter_DefineText ( text {; attribute list } )
    Defines objects that will be printed as a page footer on every page printed


  PCom_ObjectPrintText_Immediate( text to print { ; attribute list } )
    Print the text immediately without buffering
    
    
  PCom_LinePrint_Immediate ( line thickness  )
    Prints immediately (without buffering) a horizontal line across the page, 
    line thickness can be "hairline", "1", "2", "3", "4"
    
  
  PCom_Table_Define ( table name { ; table attribute list } )
    Defines a table that will be printed on the page
    
     
  PCom_Table_HeaderDefine ( table name ; table-header text { ; attributes list } )
    This defines a header that will be printed before the table objects are printed
    and will be printed again if the table continues to another page
    
    
  PCom_Column_Define ( table name ; column name { ; attributes list } )
    Define a column in a table to use to format/name a column in rows of data to print
    Column name must be unique for the table. Table must be already defined.
  
  PCom_Column_HeaderDefine ( table name ; column name ; header text { ; attributes list } )
      Define a column header to be printed on the top of the column on every page where any 
      part of the column is printed
      Table and column must already be defined. Defining a Header more than once will over-write prior definitions
      
      
  PCom_Object_TextDefine ( table name ; Column name ; text to print {; attributes list})
    Defines a text object to print in the specified column, table, modified by the attributes list
    If Row number=-1 then places in next row
    
    
  PCom_Flush 
    Print all the stored objects in all the tables and clears the table and column definitions
    
    

Aspirational routines (will be implemented in future)

  PCom_Object_PrintRow ( table name ; First column text { ; next column ... } )
  
  PCom_Object_PrintPicImmediate ( picture { ; attribute list } )
  
  PCom_Object_PicDefine( picture ; table name ; column name ; row { ; attribute list } )

Aspirational attributes  (will be implemented in future)
  
  "vertical-align": "top", "center", "bottom"
    
  "border" value of "hairline"
  
  
 Known limits:
    Can't specify 2 or more text styles of underline, bold, italic
    


Updated 9 Jan 2014

Copyright (C) 2014 Leonard Soloniuk, MD


Permission is hereby granted, free of charge, to any person obtaining a copy of this 
software and associated documentation files (the "Software"), to deal in the Software 
without restriction, including without limitation the rights to use, copy, modify, merge, 
publish, distribute, sublicense, and/or sell copies of the Software, and to permit 
persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in 
all copies or substantial portions of the Software.


THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, 
INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A 
PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT 
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF 
CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE 
OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

