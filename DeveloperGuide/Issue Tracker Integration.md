[[Home]] > [[Developer Guide]] > [[DeveloperGuide/Issue Tracker Integration|Issue Tracker Integration]]

= Issue Tracker Integration =

Since our recent upgrade of our [[http://www.dotnetrdf.org/tracker/|Issue Tracker]] we are now able to configure Mercurial hooks to have commits against specific issues logged on the Issue Tracker.

== TortoiseHg Integration ==

You can get the TortoiseHg UIs to recognize issue links by adding the following to your ##.hg\hgrc## file, if you already have a ##[tortoisehg]## section you should omit the ##[tortoisehg]## line and append the lines to the existing section.

{{{
[tortoisehg]
issue.regex = \[?([A-Za-z]{1,50}-(\d+))\]?
issue.link = http://www.dotnetrdf.org/tracker/Issues/IssueDetail.aspx?id={2}
}}}

== Hook Setup ==

To configure this you will need to configure the hook as follows in your ##.hg\hgrc## file of your clone of the dotnetrdf repository.  If you already have a ##[hooks]## section you should omit the ##[hooks]## line and append to your existing ##[hooks]## section instead.

{{{
[hooks]
outgoing.BugNet = "PATH_TO_CLONE\Build\BugNetHooks\BugNET.MercurialChangeGroupHook.exe"
}}}

The dotNetRDF repository already contains the hook code to be run, you simply need to point to your local clone by replacing ##PATH_TO_CLONE## with an absolute path to your clone.  Once this is set in your ##.hg\hgrc## file the hook will be run whenever you do a ##hg push##

=== Integration for hg commit ===

If you prefer to have the hook run on every ##hg commit## you can do so:

{{{
commit.BugNet = "PATH_TO_CLONE\Build\BugNetHooks\BugNET.MercurialChangeGroupHook.exe"
}}}

=== Integration for hg pull ===

If you are frequently pulling in changes from other peoples repositories or those merged via the BitBucket UI using ##hg pull## you may wish to also add the following to your hooks section:

{{{
changegroup.BugNet = "PATH_TO_CLONE\Build\BugNetHooks\BugNET.MercurialChangeGroupHook.exe"
}}}