# cz-git-universal-jira-conventional

**cz-git-universal-jira-conventional** is a plugin for the [**commitizen tools**](https://github.com/commitizen-tools/commitizen), a toolset that helps you to create [**conventional commit messages**](https://www.conventionalcommits.org/en/v1.0.0/). Since the structure of conventional commits messages is standardized they are machine readable and allow commitizen to automaticially calculate and tag [**semantic version numbers**](https://semver.org/) as well as create **CHANGELOG.md** files for your releases.

This plugin extends the commitizen tools by:
- **adding a Jira issue id** in the commit message (can be optional, see config below)
- **create links to Git-Repository** commits in the CHANGELOG.md
- **create links to Jira** issues in the CHANGELOG.md

This plugin is a fork of the original [cz-github-jira-conventional plugin](https://github.com/apheris/cz-github-jira-conventional).
Without it, this plugin would not be possible. It extends it by adding generic git commit path support for different git platforms and the option for leaving the jira issue id empty.

When you call commitizen `commit` you will be prompted to enter the scope of your commit as a Jira issue id (or multiple issue ids, prefixed or without prefix, see config below).
```
> cz commit
? Select the type of change you are committing fix: A bug fix. Correlates with PATCH in SemVer
? JIRA issue number (multiple "42, 123"). XX-
...
```

The changelog created by cz (`cz bump --changelog`)will contain links to the commits in Github and the Jira issues.
```markdown
## v1.0.0 (2021-08-06)

### Features

- **[XX-123](https://myproject.atlassian.net/browse/XX-123)**: create changelogs with links to issues and commits [a374b](https://github.com/apheris/cz-github-jira-conventional/commit/a374b93f39327964f5ab5290252b795647906008)
- **[XX-42](https://myproject.atlassian.net/browse/XX-42),[XX-13](https://myproject.atlassian.net/browse/XX-13)**: allow multiple issue to be referenced in the commit [07ab0](https://github.com/apheris/cz-github-jira-conventional/commit/07ab0e09de36712ab1db93fff0c821ecd80b5849)
``` 


## Installation

Install with pip
`python -m pip install cz-git-universal-jira-conventional` 

You need to use a cz config file that has the **required** additional values `jira_base_url` and `git_commit_base_url` and may contain the **optional** value `jira_prefix`.

Example `.cz.yaml` config for this repository
```yaml
commitizen:
  name: cz_git_universal_jira_conventional
  tag_format: v$version
  version: 1.0.0
  jira_prefix: XX-
  jira_base_url: https://myproject.atlassian.net
  git_commit_base_url: https://github.com/ngoldack/cz-git-universal-jira-conventional/commit/
```

The `jira_prefix` can be either 
- empty (the user must write the prefix for each issue)
- a string (the prefix will be added automatically)
- a list (for multiple projects, the user will be asked to choose a prefix)

```yaml
  jira_prefix: 
    - XX-
    - XY-
    - YY-
```

The `jira_allow_empty` can be either
- `false` (default, a jira issue id is mandatory)
- `true` (jira issue id is optional and can be omitted)

The `git_commit_base_url` depends on your git platform vendor.
You want to add an url which can be appended with to commit-sha for reference. Examples:
- Github: `https://github.com/ngoldack/cz-git-universal-jira-conventional/commit/`
- Bitbucket (Self-Hosted): `https://bitbucket.example.com/projects/PROJECT-NAME/repos/REPO-NAME/commits/`

### pre-commit
Add this plugin to the dependencies of your commit message linting with `pre-commit`. 

Example `.pre-commit-config.yaml` file.
```yaml
repos:
  - repo: https://github.com/commitizen-tools/commitizen
    rev: v2.17.13
    hooks:
      - id: commitizen
        stages: [commit-msg]
        additional_dependencies: [cz-git-universal-jira-conventional]
```
Install the hook with 
```bash
pre-commit install --hook-type commit-msg
```

<!-- LICENSE -->
## License

Distributed under the MIT License. See [LICENSE](LICENSE) for more information.

<!-- ACKNOWLEDGEMENTS -->
## Acknowledgements
This plugin is a fork of the original [cz-github-jira-conventional plugin](https://github.com/apheris/cz-github-jira-conventional).
Without it, this plugin would not be possible.

This plugin would not have been possible without the fantastic work from:
* [commitizen tools](https://github.com/commitizen-tools/commitizen)
* [conventional_JIRA](https://github.com/Crystalix007/conventional_jira)
* [cz-github-jira-conventional plugin](https://github.com/apheris/cz-github-jira-conventional)