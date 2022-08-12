# gh-repo-visibility

This script is used by the `gh` command-line to help sync or update the **visibility** of repositories. If no input file is provided, it will sync the **visibility** of all repositories found in the destination *Organization* from the *Source* Organization.

If an input file is provided, it will update the **visibility** of all repositories in the input file.

**Note:** The tool will also sync the `isTemplate` property of repositories. as well.

## PREREQS

You need to have the following to run this script successfully:

- **GitHub Personal Access Token** with a scope of "repos" and access to the organization(s) that will be updated.
- Either the name of the organizations to be synced, or a file containing the names of the repositories to be updated
- **jq** installed on the machine running the query

## Input file

The `input file` should be in the format:

```csv
Repo_Name1
Repo_Name2
Repo_Name3
```

The script will read the input file list of repositories and replicate the visibility, template status, and archive status of each repository.

## Usage

`gh repo-visibility --source-org OriginalOrg --destination-org MyNewOrg --source-token ABCDEFG1234567 --destination-token 1234567ABCDEFG`

### Output

```bash
gh repo-visibility -do lukastestorg2 -dt <REDACTED> -so lukastestorg1 -st <REDACTED>

######################################################
######################################################
########## GitHub repo visibilty updater #############
######################################################
######################################################

----------------------------------------------------
Validating user input...
Getting all source repos for:[lukastestorg1]
Gathered all data from source GitHub Organization
----------------------------------------------------
Getting all destination repos for:[lukastestorg2]
Gathered all data from destination GitHub Organization
----------------------------------------------------
----------------------------------------------------
Parsing all data and generating update list...
Adding:[lukastestorg2/repo1|internal|false] to update array
Adding:[lukastestorg2/repo2|public|false] to update array
Adding:[lukastestorg2/repo3|private|false] to update array
----------------------------------------------------
Updating visibility of:[lukastestorg2/repo1] to visibility:[internal] and template:[false]
Data was not updated, it was already up to date
----------------------------------------------------
----------------------------------------------------
Updating visibility of:[lukastestorg2/repo2] to visibility:[public] and template:[false]
Successfully updated data on GitHub!
----------------------------------------------------
----------------------------------------------------
Updating visibility of:[lukastestorg2/repo3] to visibility:[private] and template:[false]
Data was not updated, it was already up to date
----------------------------------------------------

######################################################
The script has completed
######################################################
```

### Limitations

- The tool currently is only setup to work on `https://github.com`
- The tool currently scans all repositories in the source organization and destination organization using GraphQL. This could prove to use many of your API rate limit on *much larger* Organizations
