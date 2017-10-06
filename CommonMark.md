CommonMark
==========

![Markdown Icon](_files/markdown_icon.png)

CommonMark propose a **standard, unambiguous syntax specification for Markdown**, along with a suite of comprehensive tests to validate Markdown implementations against this specification.

* [Quick reference card](http://commonmark.org/help/)
* [The CommonMark specification](http://spec.commonmark.org/)
* [Reference implementation on GitHub](https://github.com/commonmark/CommonMark)


Short reference
---------------

### **Text** *styles*

    *Italic* or _Italic_
    **Bold** or __Bold__


### Headings (1-6)

    Heading1
    =========
    Heading2
    --------
    
    # Heading1
    ## Heading2
    ### Heading3
    #### Heading4
    ##### Heading5
    ###### Heading6

    ## Heading2 ########################
    #### Heading4 ######################


### Links

Autolinks are absolute URIs and email addresses inside `<` and `>`. They are parsed as links, with the URL or email address as the link label.

    <http://example.org>
    <foo@bar.example.com>

    [link text](/some/uri)
    [link text](http://example.org "optional link title")

    [link text][1]
    [Another link text][2]
    [link text][some link label]
    [link text][foo]
    ...
    [1]: http://example.org/one
    [2]: http://example.org/another "optional link title"
    [some link label]: /some/uri
    [foo]: http://example.org/other

> NOTE: The destination cannot contain spaces or line breaks, even if enclosed in pointy brackets.


### Images

    ![Alt Text](http://example.org/img.jpg)
    ![alt](http://example.org/img.jpg "img title")

    ![Some alt text][1]
    ...
    [1]: (http://example.org/img.jpg)


### Blockquites

> Some blockquotes

    > Some blockquote


### Lists

- List Item
    - Sub Item
- List Item

1) Item One
    1) Sub Item One
    2) Sub Item Two
2) Item Two

```
* Item
* Item
    * Sub Item
    * Sub Item
* Item

- Item
- Item
    - Sub Item
    - Sub Item
- Item

1. Item1
2. Item2

1) Item1
2) Item2
```


### Horizontal Rule

    ---
    ---------------
    ***
    **********


### Code

`Inline code` with backticks

    `Inline code` with backticks

``Inline code contains a ` backtick`` with two backticks

    ``Inline code contains a ` backtick`` with two backticks

Code block

    ``` bash
    echo '3 backticks or'
    echo 'indent 4 spaces'
    ```

        # indented code block
        print '3 backticks or'
        print 'indent 4 spaces'

> NOTE: Backslash escapes do not work in code spans. All backslashes are treated literally.

> NOTE: Code span backticks have higher precedence than any other inline constructs except HTML tags and autolinks.



### Entity and numeric character references

All valid HTML entity references and numeric character references, except those occuring in code blocks and code spans, are recognized as such and treated as equivalent to the corresponding Unicode characters.

&nbsp; &amp; &copy; &AElig; &Dcaron;
&frac34; &HilbertSpace; &DifferentialD;
&ClockwiseContourIntegral; &ngE;

    &nbsp; &amp; &copy; &AElig; &Dcaron;
    &frac34; &HilbertSpace; &DifferentialD;
    &ClockwiseContourIntegral; &ngE;

**Decimal numeric character** consist of `&#` + a string of 1–8 arabic digits + `;`. A numeric character reference is parsed as the corresponding Unicode character. 

&#35; &#1234; &#992; &#98765432; &#0;
    
    &#35; &#1234; &#992; &#98765432; &#0;

**Hexadecimal numeric character** consist of `&#` + either `X` or `x` + a string of 1-8 hexadecimal digits + `;`. They too are parsed as the corresponding Unicode character.

&#X22; &#XD06; &#xcab;
    
    &#X22; &#XD06; &#xcab;

---

> NOTE: Indicators of block structure always take precedence over indicators of inline structure.

> NOTE: One to three spaces indentation are allowed.


