---
layout: page
title: "Feature: Border Control for Table Pagination"
permalink: /nanhu_print_java_doc/document/feature_border_control_table_pagination
---

# Feature: Border Control for Table Pagination

## Overview

nanhu-print-java version 1.0.6 introduces a new feature: **Border control when table is paginated**. This feature allows you to add specific border styles to the first/last row of the table tbody, or the first/last row of each page when printing a table across multiple pages.

## Background

When printing a long table that spans multiple pages, it's often necessary to add specific border styles to:
- The first row of the entire table body (tbody)
- The last row of the entire table body (tbody)
- The first row of each page
- The last row of each page

This is particularly useful for:
- Creating visual separation between table sections
- Highlighting important rows
- Maintaining consistent table appearance across pages
- Improving readability of multi-page tables

## Parameters

The framework provides four parameters to implement this function, arranged in order of priority from low to high:

### 1. firstLineOfTbodyCss
- **Description**: Applied to the first row of the table tbody
- **Priority**: Lowest
- **Use Case**: When you want to apply a specific border style to the very first row of the entire table body

### 2. lastLineofTbodyCss
- **Description**: Applied to the last row of the table tbody
- **Priority**: Second lowest
- **Use Case**: When you want to apply a specific border style to the very last row of the entire table body

### 3. firstLineOfPageCss
- **Description**: Applied to the first row of each page
- **Priority**: Second highest (higher than table tbody border)
- **Use Case**: When you want to apply a specific border style to the first row that appears on each page

### 4. lastLineOfPageCss
- **Description**: Applied to the last row of each page
- **Priority**: Highest
- **Use Case**: When you want to apply a specific border style to the last row that appears on each page

## Priority Rules

When multiple parameters are configured, the framework applies them according to the following priority rules:

1. If a row is both the first row of tbody AND the first row of a page, `firstLineOfPageCss` takes precedence
2. If a row is both the last row of tbody AND the last row of a page, `lastLineOfPageCss` takes precedence
3. If a row is the first row of tbody but NOT the first row of a page, `firstLineOfTbodyCss` is applied
4. If a row is the last row of tbody but NOT the last row of a page, `lastLineofTbodyCss` is applied

## Configuration Example

Here's a complete example showing how to use all four parameters:

```xml
<tbody>
    <forEach var="item" itemsJs="order.itemLi" varStatus="index">
        <tr>
            <td cls="borderLeftSolid textAlignCenter paddingAll">
                <params>
                    <param name="firstLineOfTbodyCss" value="borderLeftSolid borderTopSolid textAlignCenter paddingAll"></param>
                    <param name="lastLineofTbodyCss" value="borderLeftSolid borderBottomSolid textAlignCenter paddingAll"></param>
                    <param name="firstLineOfPageCss" value="borderLeftSolid borderTopSolid textAlignCenter paddingAll"></param>
                    <param name="lastLineOfPageCss" value="borderLeftSolid borderBottomSolid textAlignCenter paddingAll"></param>
                </params>
                <span value="js:item.name" />
            </td>
        </tr>
    </forEach>
</tbody>
```

## Result Example

Here's an example of the output when using border control for table pagination:

![Table Pagination Border Control Example](/nanhu_print_java_doc/images/page_pagination_border.png)

This example demonstrates how the border control feature works when a table spans multiple pages. You can see that:
- The first row of each page has a top border
- The last row of each page has a bottom border
- The borders are applied consistently across all pages

## Practical Examples

### Example 1: Add top border to first row of each page

```xml
<tbody>
    <forEach var="item" itemsJs="order.itemLi" varStatus="index">
        <tr>
            <td cls="borderLeftSolid textAlignCenter paddingAll">
                <params>
                    <param name="firstLineOfPageCss" value="borderLeftSolid borderTopSolid textAlignCenter paddingAll"></param>
                </params>
                <span value="js:item.name" />
            </td>
        </tr>
    </forEach>
</tbody>
```

### Example 2: Add bottom border to last row of each page

```xml
<tbody>
    <forEach var="item" itemsJs="order.itemLi" varStatus="index">
        <tr>
            <td cls="borderLeftSolid textAlignCenter paddingAll">
                <params>
                    <param name="lastLineOfPageCss" value="borderLeftSolid borderBottomSolid textAlignCenter paddingAll"></param>
                </params>
                <span value="js:item.name" />
            </td>
        </tr>
    </forEach>
</tbody>
```

### Example 3: Add borders to both first and last rows of tbody

```xml
<tbody>
    <forEach var="item" itemsJs="order.itemLi" varStatus="index">
        <tr>
            <td cls="borderLeftSolid textAlignCenter paddingAll">
                <params>
                    <param name="firstLineOfTbodyCss" value="borderLeftSolid borderTopSolid textAlignCenter paddingAll"></param>
                    <param name="lastLineofTbodyCss" value="borderLeftSolid borderBottomSolid textAlignCenter paddingAll"></param>
                </params>
                <span value="js:item.name" />
            </td>
        </tr>
    </forEach>
</tbody>
```

### Example 4: Different border styles for different scenarios

```xml
<tbody>
    <forEach var="item" itemsJs="order.itemLi" varStatus="index">
        <tr>
            <td cls="borderLeftSolid textAlignCenter paddingAll">
                <params>
                    <param name="firstLineOfTbodyCss" value="borderLeftSolid borderTopDouble textAlignCenter paddingAll"></param>
                    <param name="lastLineofTbodyCss" value="borderLeftSolid borderBottomDouble textAlignCenter paddingAll"></param>
                    <param name="firstLineOfPageCss" value="borderLeftSolid borderTopSolid textAlignCenter paddingAll"></param>
                    <param name="lastLineOfPageCss" value="borderLeftSolid borderBottomSolid textAlignCenter paddingAll"></param>
                </params>
                <span value="js:item.name" />
            </td>
        </tr>
    </forEach>
</tbody>
```

## Common CSS Classes for Borders

You can use any CSS class name in the parameter values. Here are some commonly used border classes:

- `borderTopSolid`: Solid top border
- `borderBottomSolid`: Solid bottom border
- `borderLeftSolid`: Solid left border
- `borderRightSolid`: Solid right border
- `borderTopDouble`: Double top border
- `borderBottomDouble`: Double bottom border
- `borderTopDashed`: Dashed top border
- `borderBottomDashed`: Dashed bottom border
- `borderTopDotted`: Dotted top border
- `borderBottomDotted`: Dotted bottom border

## Tips and Best Practices

1. **Define CSS classes first**: Make sure the CSS classes you reference in the parameters are defined in your CSS configuration
2. **Test with different page sizes**: Test your border configurations with different page sizes to ensure they work correctly
3. **Consider readability**: Use border styles that enhance readability without being distracting
4. **Combine with other features**: This feature works well with other nanhu-print-java features like fixed headers and footers
5. **Priority awareness**: Be aware of the priority rules when using multiple parameters together

## Migration from Previous Versions

If you're upgrading from a previous version of nanhu-print-java, you don't need to change any existing configurations. The border control feature is optional and only activates when you configure the relevant parameters.

## Version Information

- **Introduced in**: nanhu-print-java 1.0.6
- **Status**: Stable
- **Backward Compatibility**: Fully compatible with previous versions

## Related Documentation

- [Introduction](/nanhu_print_java_doc/document/introduction) - Section 3.11 covers this feature
- [Quick Start](/nanhu_print_java_doc/document/quick_start) - Get started with nanhu-print-java
- [Architecture](/nanhu_print_java_doc/document/architecture) - Understand the framework architecture
- [FAQ](/nanhu_print_java_doc/document/faq) - Frequently asked questions

## Support

For more information or support, please visit:
- GitHub: https://github.com/hongjinqiu/nanhu-print-java
- Demo Project: https://github.com/hongjinqiu/nanhu-print-java-demo

{% include head.html %}