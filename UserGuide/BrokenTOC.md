= Broken TOC =

This is a page to demonstrate that the bug with the TOC macro is not fully resolved. ([[https://bitbucket.org/site/master/issue/2224/toc-macro-does-not-work-in-nested|Issue 2224]])

I am using the macro like so:

{{{
<<toc UserGuide/Storage/>>
}}}

Which should link to all pages within that directory, however the links get extra segments added because they are calculated from the current directory not the wiki root hence they have an extra ##UserGuide/## in them.  Thus if you click on a link below you get a Page doesn't exist message

<<toc UserGuide/Storage/>>