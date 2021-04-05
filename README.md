This action is to be used with a workflow run for a pull request event that is open.
There are numerous inputs.
The default behaviour is to fail when there are no artifacts - control this with errorNoArtifacts.
Comments can be created in the pull request and / or associated issues - control with addTo.
The comment that is generated can have a prefix and a suffix.
It is possible to filter out artifacts from comments by name.
This is a accomplished with either includes or includesFormatted.  The latter being used if the formatting of the 
artifacts is not consistent.
There are 3 formats
The default, url, will just have the url of the artifact in the comment. 
Name will result in `[name](url)`
The final format is custom and is a format string e.g `Whatever ({name})[{url}] etc`
If includes is not provided then all artifacts are included.  If you provide then e.g - name1, name2
If using includesFormatted then - array of objects {name:string,format?:string}

The remaining inputs control the discovery of the pull request associated issues.
There are 4 ways that associated issues can be specified.
In the pull request itself - control with usePullTitle and usePullBody
In the pull request commits - control with useCommitMessages
Finally there is the branch name - control with useBranch, branchIssueWords and branchDelimiters.

All 4 use the inputs closeWords and caseSensitive as part of their matching.  Close words defaults to the close words used by github to automatically close an issue from a pull request.
The pull request title, pull request body and commit messages will match against closeWord #123
The branch name will match by default closeWord-issue/s-123

The action can also be used with a repository dispatch of a pull request workflow run [see](https://github.com/tonyhallett/workflow-run-conclusion-dispatch-action).  In this case you need to set the input workflowPayload.
```
name: add artifact links to pull request and related issues
on:
  workflow_run:
    workflows: [Pull request run that uploads artifacts]
    types: [completed]

jobs:
  artifacts-url-comments:
    name: add artifact links to pull request and related issues job
    runs-on: windows-2019
    steps:
      - name: add artifact links to pull request and related issues step
        uses: tonyhallett/artifacts-url-comments@v1.0.0
        env:
            GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
            prefix: Here are the artifacts 
            suffix: Have a nice day.
            format: name
            addTo: pullandissues
```
      

