+++
title = "Hijacking GitHub Runners"
date = 2024-06-22
[taxonomies]
tags = ["github", "CI/CD", "secdevops"]
+++

[Hijacking GitHub runners to compromise the organization](https://www.synacktiv.com/publications/hijacking-github-runners-to-compromise-the-organization)

Summary:

> In a recent engagement we managed to compromise a GitHub app allowed to register self-hosted runners at the organization level. Turns out, it is possible to register a GitHub runner with the ubuntu-latest tag, granting access to jobs originally designated for GitHub-provisioned runners. Using this method, an attacker could compromise any workflow of the organization and steal CI/CD secrets or push malicious code on the different repositories.

The fundamental breakdown of what to look out for and think about:

> the runs-on directive indicates that the workflow must run on a runner with the ubuntu-latest tag. This label is one of the tags meaning the workflow should be executed on a runner provided by GitHub. 

> They are used to specify the type of runner and the operating system needed for a workflow. The ubuntu-latest label is quite common.

> this repository is protected with branch protections. Even with write access it would not be possible to deploy a malicious workflow with nord-stream to extract the different secrets, since the required_pull_request_reviews protection is enabled

{{ image(src="github_runner.png", alt="") }}

> This workflow is set to run when a comment is posted on a pull request. Between lines 12 and 22, it retrieves the pull request reference associated with the comment. It then uses the GitHub API through some JavaScript to obtain the branch reference of the targeted pull request. Finally, the code performs a checkout of the code coming from the pull request at line 26.

> Contexts can be accessed using the following expression syntax: `${{ <context> }}`. At runtime, the context will be replaced with the associated value, like a match and replace.

> The vulnerability is present at line 25. The code uses the GitHub context to obtain a reference to the pull request, which corresponds to the branch name of the pull request. This branch name is under the attacker's control, meaning they can create a dummy pull request with the following branch name. Then to trigger the vulnerable code, the attacker just need to create a comment on the associated pull request.

It's like anything else when looking for code injection within a software stack. Researching what Github Runners are about and understand what user controlled variables allowed in a workflow to gain server-side code execution. All this does take time unless you are a specialist in secdevops of which is in their wheelhouse.