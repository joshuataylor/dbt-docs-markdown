# Merge conflicts

[Merge conflicts](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/about-merge-conflicts) in the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md) often occur when multiple users are simultaneously making edits to the same section in the same file. This makes it difficult for Git to decide what changes to incorporate in the final merge.

The merge conflict process provides users the ability to choose which lines of code they'd like to preserve and commit. This document will show you how to resolve merge conflicts in the Studio IDE.

## Identify merge conflicts[​](#identify-merge-conflicts "Direct link to Identify merge conflicts")

You can experience a merge conflict in two possible ways:

* Pulling changes from your main branch when someone else has merged a conflicting change.
* Committing your changes to the same branch when someone else has already committed their change first.

The way to [resolve](#resolve-merge-conflicts) either scenario will be exactly the same.

For example, if you and a teammate make changes to the same file and commit, you will encounter a merge conflict as soon as you **Commit and sync**.

The Studio IDE will display:

* **Commit and resolve** git action bar under **Version Control** instead of **Commit** — This indicates that the Cloud Studio IDE has detected some conflicts that you need to address.
* A 2-split editor view — The left view includes your code changes and is read-only. The right view includes the additional changes, allows you to edit and marks the conflict with some flags:

```text
<<<<<< HEAD
    your current code
======
    conflicting code
>>>>>> (some branch identifier)
```

* The file and path colored in red in the **File Catalog**, with a warning icon to highlight files that you need to resolve.
* The file name colored in red in the **Changes** section, with a warning icon.
* If you press commit without resolving the conflict, the Studio IDE will prompt a pop up box with a list which files need to be resolved.

[![Conflicting section that needs resolution will be highlighted](/img/docs/dbt-cloud/cloud-ide/merge-conflict.png?v=2 "Conflicting section that needs resolution will be highlighted")](#)Conflicting section that needs resolution will be highlighted

[![Pop up box when you commit without resolving the conflict](/img/docs/dbt-cloud/cloud-ide/commit-without-resolve.png?v=2 "Pop up box when you commit without resolving the conflict")](#)Pop up box when you commit without resolving the conflict

## Resolve merge conflicts[​](#resolve-merge-conflicts "Direct link to Resolve merge conflicts")

You can seamlessly resolve merge conflicts that involve competing line changes in the Cloud Studio IDE.

1. In the Studio IDE, you can edit the right-side of the conflict file, choose which lines of code you'd like to preserve, and delete the rest.
   <!-- -->
   * Note: The left view editor is read-only and you cannot make changes.
2. Delete the special flags or conflict markers `<<<<<<<`, `=======`, `>>>>>>>` that highlight the merge conflict and also choose which lines of code to preserve.
3. If you have more than one merge conflict in your file, scroll down to the next set of conflict markers and repeat steps one and two to resolve your merge conflict.
4. Press **Save**. You will notice the line highlights disappear and return to a plain background. This means that you've resolved the conflict successfully.
5. Repeat this process for every file that has a merge conflict.

[![Choosing lines of code to preserve](/img/docs/dbt-cloud/cloud-ide/resolve-conflict.png?v=2 "Choosing lines of code to preserve")](#)Choosing lines of code to preserve

Edit conflict files

* If you open the conflict file under **Changes**, the file name will display something like `model.sql (last commit)` and is fully read-only and cannot be edited.
  <br />
* If you open the conflict file under **File Catalog**, you can edit the file in the right view.

## Commit changes[​](#commit-changes "Direct link to Commit changes")

When you've resolved all the merge conflicts, the last step would be to commit the changes you've made.

1. Click the git action bar **Commit and resolve**.
2. The **Commit Changes** pop up box will confirm that all conflicts have been resolved. Write your commit message and click **Commit Changes**.
3. The Studio IDE will return to its normal state and you can continue developing!

[![Conflict has been resolved](/img/docs/dbt-cloud/cloud-ide/commit-resolve.png?v=2 "Conflict has been resolved")](#)Conflict has been resolved

[![Commit Changes pop up box to commit your changes](/img/docs/dbt-cloud/cloud-ide/commit-changes.png?v=2 "Commit Changes pop up box to commit your changes")](#)Commit Changes pop up box to commit your changes

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
