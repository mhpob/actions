# setup-vdat-action

This action installs the VDAT executable that drives [Innovasea's Fathom
software](https://www.innovasea.com/fish-tracking/products/fathom-software/).
Windows, Linux, and macOS operating systems are supported. It does this by 
downloading the current version of VDAT housed in the Ocean Tracking Network
Data Centre's (OTNDC) GitLab instance utilizing a GitLab personal access
token (PAT) and the GitLab API.

**It is important that you do not directly supply your Personal Access Token!**

**Only do so using a GitHub secret!**

To gain a personal access token and supply it as a GitHub Secret:

1. Contact the OTNDC to open a GitLab account with access to the VDat 
Working Group repository.
2. After logging in, click your profile picture.
3. Click "Preferences", then, under "User Settings", click "Access Tokens".
4. Click the "Add new token" button.
5. Give the token a name and expiration date; this is a tradeoff between 
saftey (short life) and ease (long life).
6. Under "Select scopes" select "read repository".
7. Copy the token. Note that you will not be able to see the PAT
again once you leave this page. Luckily it's easy to make another.
8. In the GitHub repository that will use the action, select
"Settings" > "Secrets and variables" > "Actions"
9. Under "Repository secrets" click "New repository secret"
10. Paste in your copied PAT, give the secret a name, and click "Add secret".
Note that you will not be able to see this value again, only delete
or replace it.
11. In your repository's workflow file, supply the token to the 
`gitlab-auth` argument as `${{ secrets.YOUR_TOKEN_NAME }}`. In
the example below, the PAT is stored in a secret called "GITLAB_TOKEN"
and is supplied using `gitlab-auth: ${{ secrets.GITLAB_TOKEN }}`


## Inputs

- `gitlab-auth`: A GitHub Secret that holds a personal access token
  to the OTNDC's VDat Working Group GitLab. A GitLab account with
  access to the VDat Working Group repository is currently necessary 
  to utilize this action.
- `vdat-repo`: Not currently implemented; TBD
- `vdat-version`: Not currently implemented; TBD

## Outputs

- Currently no output, though the result of `vdat --help` is printed
  to confirm installation. Future version will output the installed
  VDAT version as an environmental variable.

## Usage
You can see this action _in action_ in the testing workflow of the [`rvdat` 
R package](https://github.com/mhpob/rvdat/blob/main/.github/workflows/R-CMD-check.yaml#L35).
The action can be used as below in a more-general manner:

``` yaml
jobs:
  set-up-vdat:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
    
    runs-on: ${{ matrix.os }}
    name: VDAT for ${{ matrix.os }}
    
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4
      
      - name: Set up VDAT
        uses: mhpob/setup-vdat@v1
        with:
          gitlab-auth: ${{ secrets.GITLAB_TOKEN }}
```

## License
