Jenkins Library
###############

Find below the description of the Jenkins library functions meant for external use.


makebuild
*********

Make a new subfolder 'build' and copy a subset of the contents of current folder
into it. The intention is to prepare the build folder as a clean deployment fileset
without development only components.
The default copies all files except:
  - .git
  - .gitignore
  - Jenkinsfile
  - jenkinslib.sh
  - jenkinsPkgDataFile.txt
  - build
  - buildinclude
  - buildexclude

buildinclude and buildexclude are optional files whose content is interpreted as
a list of files that shall either be included with or excluded from the copy into
the build folder.

If there is a buildinclude, but no buildexclude file, all files are considered
excluded by default and only the items listed in buildinclude are copied into the
build folder.

If there is a buildexclude, all contents of the current folder are copied by default,
excluding those listed in buildexclude and excluding those excluded by default.
   
If there is both a buildinclude and a buildexclude file, all files of the current
folder are copied by default, excluding those listed in buildexclude and excluding
those excluded by default. The content of buildinclude is considered a list of
additional (outside of current folder) items to be added to the copy.


PkgPack
*******

Recursively compresses contents of the current folder into the package destination
folder "<dstroot>/<dev|prod>/repo". Two output files of type zip and tbz are
produced respectively.


PkgXar
******

recursively compresses contents of the current folder into the package destination
folder "<dstroot>/<dev|prod>/repo". One output file of type xar is produced.


PkgLinkLatest
*************

Package files of the current build at "<dstroot>/<dev|prod>/repo" are linked as:

 - "<PkgName>-latest" at "<dstroot>/<dev|prod>".
 - "<PkgName>-<PkgVersion>" at "<dstroot>/<dev|prod>/archive"


PkgLinkBundle
*************

Package files of the current build at "<dstroot>/<dev|prod>/repo" are linked as:

 - "<PkgName>-<PkgVersion>" at "<dstroot>/<dev|prod>/<PkgBundleVersion>"


TreeListComponents
******************

Parameter: VersionTreeRootFolder

List all components appearing in a version tree in alphabetical order.


TreeMakeComponentsTable
***********************

Parameter: VersionTreeRootFolder

Produce a csv file containing version tree data formatted into a single table.


TreeForAllInVersions
********************

Parameter: VersionTreeRootFolder
Parameter: ShellFunction

For each leaf in the version tree source package data and call the shell function.
