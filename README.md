# A pdf framework that solves the pain points of enterprise development (implemented in java language)

## 1. What is Nanhu-print-java

Nanhu-print-java is a pdf generation framework implemented by java language. 

User can prepare the json format business data and a xml format configure file, then call the nanhu-print-java framework API to complete the generation of a PDF file.

Development video can refer to: `https://youtu.be/vdTSc8rXr9M`

### 1.1. The background of the birth of Nanhu-print-java

The company I work for wants to implement a custom printing function, which needs to meet the following functions:

1. Define some templates, each field of each template, the label can be customized, can be displayed and hidden, the width of each column of the table can be customized, and users can select fields for these templates on the browser side.
2. The header of the template, the header of the table, needs to be displayed on every page, and information such as amount summary and signature need to be fixed at the bottom of the page.

The company prepared two technical solutions at beginning:

1. The front and back ends are implemented separately. <br>The front end uses js to realize the page and controls the page fields, and the back end uses java to call the iText pdf library to generate pdf for printing.
2. The front-end implements almost all the functions. <br>After the front-end js implements the page, the generated html is sent to the back-end, and the back-end uses java to call the html-to-pdf framework to generate pdf for printing.

Considering that, each field on the page has to do complex display and hide operations and the development of the interface display directly through java is very cumbersome, and after the development is completed, changes in requirements, addition of fields and subtraction of fields will all involve program modifications. The code would be bad maintenance, therefore, the first option was rejected.

Therefore, the company chose the second solution, using js to generate html on the front end, and sending it to the back end to convert html to pdf.<br>

During the implementation process, the following problems were encountered that were difficult to solve.

1.	There are obvious differences in fonts, page styles, etc. between the front-end html and the pdf generated by the back-end.<br><br>
For a longer document, with a long table inside the document, it will appear on multiple pages when printed.<br><br>
The front-end js code generates a div according to the defined height, and adds the content of the page one by one. <br><br>When the content exceeds one page, regenerates a div and adds content to it.  <br>The div array take shape to multiple pages.<br><br>
After sending these multiple pages to the backend, using html to pdf framework, and generating pdf, it is found that the frontend displays a good html page, due to the difference of font fendering between the frontend windows and the backend linux environment, and the browser in the front-end windows environment differs from the analysis of the css style by the html-to-pdf framework in the back-end environment. <br><br>The pdf generated by the back-end will have the problem that the page content is too small or too large, lead to the printing effect is not good .<br><br>
For example: an A4 page, an html page at the front end, fills the entire div, and the display is very beautiful, but when the pdf is generated, due to the small font size or the framework conversion, the content of the generated pdf page is very small: there is a lot of white space at the bottom of the entire pdf page.<br><br>
However, due to the opacity of converting html to pdf, this problem is difficult to solve.
2.	There are complicated js controls on the page to display and hide, and the page code is difficult to maintain.<br><br>
For example: when a certain column of the table adjusts the column width, the text in this column may wrap. <br><br>At this time, the part of the content that was originally at the end of the table will be pushed out of the page boundary, and it is necessary to trigger the height control code of the page elements.

After a period of failed development and testing, the author developed the Nanhu-print-java framework, which solved the problems encountered by the company relatively smoothly.<br>

Nanhu-print-java defines a file in xml format, which defines dynamic tags such as if, forEach, set, and static tags such as table, div, and span. <br><br>Constrain users to only use these tags.<br><br>
On the front end, through Nanhu-print-js, parse xml to generate html, which will be displayed in the browser.<br><br>
On the back end, pdf is generated by parsing xml, so that the font and other styles of pdf are not affected by the front-end environment, which better solves the problem of differences between front-end display and back-end printing.<br><br>
The user's development process is mainly to configure files in xml format, and there is basically no complicated code control, which also achieves the effect of greatly improving development efficiency and reducing maintenance costs.

## 2. The basic workflow of Nanhu-print-java

<img width="439" alt="work_flow_chart" src="https://github.com/hongjinqiu/nanhu-print-java/assets/1661806/c97363da-964e-4195-9a1c-6424f3ff54a4">

First of all, Nanhu-print-java is a pdf printing framework, which defines its own XML model format file. <br><br>When users write XML format files, they need to follow the element definition of the XSD file.

As shown in the figure above, when using Nanhu-print-java, the user must prepare the XML model file and the business data to be printed, and then call the framework API of Nanhu-print-java to complete the generation of the PDF file.

The sample code is as follows:

<img width="556" alt="demo_code" src="https://github.com/hongjinqiu/nanhu-print-java/assets/1661806/01eb7fcf-3614-48bd-886f-84090ad3e904">

## 3. Main functions of Nanhu-print-java

### 3.1. Each page has a fixed header, and the last page has a fixed footer.

Bill printing is a common function in enterprise applications. <br><br>It is usually required to display the title and company name at the top of a page, and display the summary of the table amount, date, company signature and other information at the bottom of the page.

If the table information in the document is relatively long and there are multiple pages, it is usually required to have the table head row information at the top of every page.

Using the Nanhu-print-java framework, users can complete such functions conveniently and quickly through configuration.

The simplified content of the configuration is:

```xml
<body>
    <params>
        <param name="extendToFillBody" value="default"></param>
    </params>
    <table>
        <thead showPosition="firstPage">
            BillTitle,,,,,,
        </thead>
        <thead showPosition="everyPage">
            table head content,,,,,,
        </thead>
        <tbody>
            table body content,,,,,,
        </tbody>
        <tloop>
            last page fill content,,,,,,
        </tloop>
        <tbottom>
            last page bottom content,,,,,,
        </tbottom>
    </table>
</body>
```

### 3.2. Display page number at any position on every page

For multi-page documents, it may be necessary to display page numbers in the table header, and may also need to display page numbers in the table footer.

Users can use the following configuration methods to realize page number related information appearing anywhere on the page.

```xml
<div>
    <params>
        <param name="customContent" value="com.hongjinqiu.nanhuprint.eval.custom.CustomPageNumber" />
        <param name="customContentFormat" value="{currentPageNumber} of {totalPageNumber}" />
    </params>
</div>
```

### 3.3. Print with template

For documents such as courier orders, when developing a pdf printing template, a picture needs to be used as the background.

When users use Nanhu-print-java to develop this type of version, they can set the background image, and then adjust the padding value of the text:

```xml
<div backgroundSize="contain" width="100px" height="420px">
    <css>
        <backgroundImage js="url('http://xxxx.png')" />
    </css>
    <div paddingLeft="10px" paddingTop="24px"><span value="InvoiceCode" /></div>
</div>
```

## 3.4. Watermark

The Nanhu-print-java framework supports text watermarking or image watermarking through configuration. <br><br>The configuration is as follows:

Image watermark:

```xml
<div fontWeight="bold" paddingTop="10">
    <params>
        <param name="waterMark" value="default" />
        <param name="waterMarkOpacity" value="0.9" />
        <param name="waterMarkOffsetX" value="-150" />
        <param name="waterMarkOffsetY" value="0" />
        <param name="waterMarkImage" value="http://localhost:8891/images/camel.png" />
        <param name="waterMarkImageWidth" value="200" />
        <param name="waterMarkImageHeight" value="78" />
        <param name="waterMarkRotation" value="45" />
        <param name="waterMarkLayer" value="default" />
    </params>
</div>
```

Text watermark:

```xml
<div>
    <params>
        <param name="waterMark" value="default" />
        <param name="waterMarkText" value="I am waterMarkText" />
        <param name="waterMarkOpacity" value="0.5" />
        <param name="waterMarkTextFontSize" value="24" />
        <param name="waterMarkOffsetX" value="0" />
        <param name="waterMarkOffsetY" value="100" />
        <param name="waterMarkRotation" value="45" />
        <param name="waterMarkLayer" value="under" />
    </params>
</div>
```

### 3.5. Different backgrounds of table rows are printed alternately

If you have a long table and need to display different background colors for each row, you can easily implement it through configuration. <br><br>The configuration example is:

```xml
<forEach var="item" itemsJs="data.contentList" varStatus="index">
    <set valueJs="'white'" var="loopBgColor" />
    <if testJs="index %2 == 0">
        <set valueJs="'orange'" var="loopBgColor" />
    </if>
    <tr fontFamily="abc" backgroundColor="js:loopBgColor">
        <td width="100%">
            <div paddingTop="20">
                <span value="js:item"/>
            </div>
        </td>
    </tr>
</forEach>
```

### 3.6. Use dynamic tags to implement complex display logic

Nanhu-print-java framework supports dynamic tags: if, forEach, macroRef, set, Macro.

An example configuration is:

```xml
<if testJs="index %2 == 0"></if>
<forEach var="item" itemsJs="data.contentList" varStatus="index"></forEach>
```
### 3.7. Macros for repeating block references

In the page, if there are repeated code display blocks, you can put the repeated code display blocks in the macro tag, and then refer to them in other places.

Define macro code block:

```xml
<macro name="addressBillingMacro">
    <div cls="f12">
        <span value="wwww"/>
    </div>
</macro>
```

Refer macro code block:

```xml
<macroRef name="addressBillingMacro"/>
```

### 3.8. Reduce the font size in the cell to show the full content

In the printing of documents, sometimes the cell width is fixed, but the cell content is too long, which can be configured through the `scaleToFitContentByPdf` parameter to easily achieve content scaling.

Configuration example:

```xml
<div width="20px" scaleToFitContentByPdf="true">
    <span value="RMB 999,999,999.99" />
</div>
```

### 3.9. Cell width can be set to change dynamically with cell content

If you want the text not to shrink or wrap, and the cell width to change with the content, you can configure it as follows:

```xml
<td textAlign="left">
    <params>
        <param name="calcWidth" value="com.hongjinqiu.nanhuprint.eval.custom.CalcWidth" />
        <param name="calcWidthTagId" value="leftIssueBy" />
    </params>
    <div id="leftIssueBy" cls="f13 bodyLineHeight" whiteSpace="nowrap" paddingRight="5px" >
        <span value="ISSUED BY:" />
    </div>
</td>
```

### 3.10. Formatting of fields such as quantity, unit price, and amount

Quantity, unit price, amount and other fields usually need to be formatted and displayed. <br><br>Users can pass the value to the framework by formatting in the application.

The formatting of these fields can also be implemented through the configuration provided by the framework:

```xml
<span value="js:item.item_price" format="num"/>
<span value="js:item.item_price" format="unitPrice"/>
<span value="js:item.item_price" format="amt"/>
```
