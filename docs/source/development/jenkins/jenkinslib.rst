Jenkins Library
###############

Find below the description of the Jenkins library functions meant for external use.


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
