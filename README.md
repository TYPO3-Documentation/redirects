Note: This is a WIP draft for handling redirects. 

# Hints for using these files:

If you need to redirect one URL to another, use [redirects-fix.csv](https://github.com/TYPO3-Documentation/redirects/edit/master/redirects-fix.csv). 

If there is a bunch of URLs where the redirect can be expressed as sourcepath/* >> targetpath/*, use [redirects-var.csv](https://github.com/TYPO3-Documentation/redirects/edit/master/redirects-var.csv).

If you just have the broken link and no target, you can add to [broken-links.txt](https://github.com/TYPO3-Documentation/redirects/edit/master/broken-links.txt). Please only use this as fallback, if no other choice is possible!

General rules: Use a comma (,) between source and target. Start path with /. Don't add domain. Only add URLs on docs.typo3.org. 

# redirects
List of required redirects to be used for creating redirects on docs.typo3.org. This files have a very simple format (seperated with space !) and can be used to **generate** redirects for any webserver or for example to be used as source file for RewriteMap (Apache). The advantage is: these lists are not dependant on a Webserver (Apache, Nginx) and are not dependant on how the redirects are implemented (e.g. RewriteMap, RewriteRule, redirect, etc.). 

Files:

* [redirects-fix.csv](https://raw.githubusercontent.com/TYPO3-Documentation/redirects/master/redirects-fix.csv)
* [redirects-var.csv](https://github.com/TYPO3-Documentation/redirects/blob/master/redirects-var.csv)
* [incoming-broken-links.txt](https://github.com/TYPO3-Documentation/redirects/blob/master/broken-links.txt)

## redirects-fix.csv

Syntax:

```path,targetpath```

Use this as source for URLs which redirect from one full path to another path.

Add full path, starting with slash and without domain.

Example:

```/typo3cms/ContributionWorkflowGuide/Forge/ReportingABug.html,/typo3cms/ContributionWorkflowGuide/ReportingAnIssue/Index.html```

This is how a RedirectMap could look like:

```
RewriteEngine On
RewriteMap rewritemap "txt:/etc/apache2/rewrite-fix.csv"

RewriteCond ${rewritemap:$1} !=""
RewriteRule ^(.*)$  ${rewritemap:$1} [R=permanent,L,NC]
```


## redirects-var.csv

Use this to redirect URLs where wildcards can be used.  

Syntax:

```startofpath,startoftargetpath```

Example:

```/typo3cms/ContributionWorkflowGuide/Forge/,/typo3cms/ContributionWorkflowGuide/ReportingAnIssue/```

This will redirect **all** URls starting with startpath to its matching counterpart. Everything after startpath is reused in the target URL.

This is how the Apache RewriteRule could look like:

```RewriteRule ^(/?)typo3cms/ContributionWorkflowGuide/Forge/(.*)$ $1typo3cms/ContributionWorkflowGuide/ReportingAnIssue/$2 [R=301,L,NC]```


# Outline for using these files with RewriteMap

Clone git repo somewhere on t3docs.typo3.org server

manual / automatic update of redirects

1. git pull changes form this repo to local repo
2. Do sanity checks on .csv files (script): Does file conform to general regex rule: ```^/typo3cms/.*\.html /typo3cms.*\.html$```
3. If ok, cp scripts to final destination on server

RewriteMap does not need to be changed, it uses updated lists of redirects. Webserver reload is required.


