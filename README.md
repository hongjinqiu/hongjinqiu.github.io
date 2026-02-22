# [A pdf framework that solves the pain points of enterprise development (implemented in java language)](nanhu_print_java_doc)

nanhu-print-java is a pdf generation framework implemented in java language. 

Users can configure a file in xml format and prepare the json data format they want to print.

Then call the nanhu-print-java framework API to complete the generation of a PDF file. 

[Continue reading the full text »](nanhu_print_java_doc)

---

## nanhu-print-java 1.0.6 Release - Border Control for Table Pagination

nanhu-print-java version 1.0.6 introduces a new feature: **Border control when table is paginated**. This feature allows you to add specific border styles to the first/last row of the table tbody, or the first/last row of each page when printing a table across multiple pages.

The framework provides four parameters to implement this function:
- `firstLineOfTbodyCss`: Applied to the first row of the table tbody
- `lastLineofTbodyCss`: Applied to the last row of the table tbody
- `firstLineOfPageCss`: Applied to the first row of each page
- `lastLineOfPageCss`: Applied to the last row of each page

[Continue reading the full text »](nanhu_print_java_doc/document/feature_border_control_table_pagination)

{% include head.html %}
