# Authenticate with Azure DevOps

If you use the Studio IDE or dbt CLI to collaborate on your team's Azure DevOps dbt repo, you need to [link your dbt profile to Azure DevOps](#link-your-dbt-cloud-profile-to-azure-devops), which provides an extra layer of authentication.

## Link your dbt profile to Azure DevOps[​](#link-your-dbt-profile-to-azure-devops "Direct link to Link your dbt profile to Azure DevOps")

Connect your dbt profile to Azure DevOps using OAuth:

1. Click your account name at the bottom of the left-side menu and click **Account settings**

2. Scroll down to **Your profile** and select **Personal profile**.

3. Go to the **Linked accounts** section in the middle of the page.

   [![Azure DevOps Authorization Screen](/img/docs/dbt-cloud/connecting-azure-devops/LinktoAzure.png?v=2 "Azure DevOps Authorization Screen")](#)Azure DevOps Authorization Screen

4. Once you're redirected to Azure DevOps, sign into your account.

5. When you see the permission request screen from Azure DevOps App, click **Accept**.

   [![Azure DevOps Authorization Screen](</img/docs/dbt-cloud/connecting-azure-devops/OAuth Acceptance.png?v=2> "Azure DevOps Authorization Screen")](#)Azure DevOps Authorization Screen

You will be directed back to dbt, and your profile should be linked. You are now ready to develop in dbt!

## FAQs[​](#faqs "Direct link to FAQs")

How can I fix my .gitignore file?

A `.gitignore` file specifies which files git should intentionally ignore or 'untrack'. dbt indicates untracked files in the project file explorer pane by putting the file or folder name in *italics*.

If you encounter issues like problems reverting changes, checking out or creating a new branch, or not being prompted to open a pull request after a commit in the Studio IDE — this usually indicates a problem with the [.gitignore](https://github.com/dbt-labs/dbt-starter-project/blob/main/.gitignore) file. The file may be missing or lacks the required entries for dbt to work correctly.

The following sections describe how to fix the `.gitignore` file in:

 Fix in the Studio IDE

To resolve issues with your `gitignore` file, adding the correct entries won't automatically remove (or 'untrack') files or folders that have already been tracked by git. The updated `gitignore` will only prevent new files or folders from being tracked. So you'll need to first fix the `gitignore` file, then perform some additional git operations to untrack any incorrect files or folders.

1. Launch the Studio IDE into the project that is being fixed, by selecting **Develop** on the menu bar.
2. In your **File Explorer**, check to see if a `.gitignore` file exists at the root of your dbt project folder. If it doesn't exist, create a new file.
3. Open the new or existing `gitignore` file, and add the following:

```bash
# ✅ Correct 
target/
dbt_packages/
logs/
# legacy -- renamed to dbt_packages in dbt v1
dbt_modules/
```

* **Note** — You can place these lines anywhere in the file, as long as they're on separate lines. The lines shown are wildcards that will include all nested files and folders. Avoid adding a trailing `'*'` to the lines, such as `target/*`.

For more info on `gitignore` syntax, refer to the [Git docs](https://git-scm.com/docs/gitignore).

4. Save the changes but *don't commit*.
5. Restart the IDE by clicking on the three dots next to the **IDE Status button** on the lower right corner of the IDE screen and select **Restart IDE**.

[![Restart the IDE by clicking the three dots on the lower right or click on the Status bar](/img/docs/dbt-cloud/cloud-ide/restart-ide.png?v=2 "Restart the IDE by clicking the three dots on the lower right or click on the Status bar")](#)Restart the IDE by clicking the three dots on the lower right or click on the Status bar

6. Once the Studio IDE restarts, go to the **File Catalog** to delete the following files or folders (if they exist). No data will be lost:

   * `target`, `dbt_modules`, `dbt_packages`, `logs`

7. **Save** and then **Commit and sync** the changes.

8. Restart the Studio IDE again using the same procedure as step 5.

9. Once the Studio IDE restarts, use the **Create a pull request** (PR) button under the **Version Control** menu to start the process of integrating the changes.

10. When the git provider's website opens to a page with the new PR, follow the necessary steps to complete and merge the PR into the main branch of that repository.

    * **Note** — The 'main' branch might also be called 'master', 'dev', 'qa', 'prod', or something else depending on the organizational naming conventions. The goal is to merge these changes into the root branch that all other development branches are created from.

11. Return to the Studio IDE and use the **Change Branch** button, to switch to the main branch of the project.

12. Once the branch has changed, click the **Pull from remote** button to pull in all the changes.

13. Verify the changes by making sure the files/folders in the `.gitignore` file are in italics.

[![A dbt project on the main branch that has properly configured gitignore folders (highlighted in italics).](/img/docs/dbt-cloud/cloud-ide/gitignore-italics.png?v=2 "A dbt project on the main branch that has properly configured gitignore folders (highlighted in italics).")](#)A dbt project on the main branch that has properly configured gitignore folders (highlighted in italics).

 Fix in the Git provider

Sometimes it's necessary to use the git providers web interface to fix a broken `.gitignore` file. Although the specific steps may vary across providers, the general process remains the same.

There are two options for this approach: editing the main branch directly if allowed, or creating a pull request to implement the changes if required:

* Edit in main branch
* Unable to edit main branch

When permissions allow it, it's possible to edit the `.gitignore` directly on the main branch of your repo. Here are the following steps:

1. Go to your repository's web interface.
2. Switch to the main branch and the root directory of your dbt project.
3. Find the `.gitignore` file. Create a blank one if it doesn't exist.
4. Edit the file in the web interface, adding the following entries:

```bash
target/
dbt_packages/
logs/
# legacy -- renamed to dbt_packages in dbt v1
dbt_modules/
```

5. Commit (save) the file.
6. Delete the following folders from the dbt project root, if they exist. No data or code will be lost:
   <!-- -->
   * `target`, `dbt_modules`, `dbt_packages`, `logs`
7. Commit (save) the deletions to the main branch.
8. Switch to the Studio IDE , and open the project that you're fixing.
9. [Rollback your repo to remote](https://docs.getdbt.com/docs/cloud/git/version-control-basics.md#the-git-button-in-the-cloud-ide) in the IDE by clicking on the three dots next to the **IDE Status** button on the lower right corner of the IDE screen, then select **Rollback to remote**.
   <!-- -->
   * **Note** — Rollback to remote resets your repo back to an earlier clone from your remote. Any saved but uncommitted changes will be lost, so make sure you copy any modified code that you want to keep in a temporary location outside of dbt.
10. Once you rollback to remote, open the `.gitignore` file in the branch you're working in. If the new changes aren't included, you'll need to merge the latest commits from the main branch into your working branch.
11. Go to the **File Explorer** to verify the `.gitignore` file contains the correct entries and make sure the untracked files/folders in the .gitignore file are in *italics*.
12. Great job 🎉! You've configured the `.gitignore` correctly and can continue with your development!

If you can't edit the `.gitignore` directly on the main branch of your repo, follow these steps:

1. Go to your repository's web interface.
2. Switch to an existing development branch, or create a new branch just for these changes (This is often faster and cleaner).
3. Find the `.gitignore` file. Create a blank one if it doesn't exist.
4. Edit the file in the web interface, adding the following entries:

```bash
target/
dbt_packages/
logs/
# legacy -- renamed to dbt_packages in dbt v1
dbt_modules/
```

5. Commit (save) the file.
6. Delete the following folders from the dbt project root, if they exist. No data or code will be lost:
   <!-- -->
   * `target`, `dbt_modules`, `dbt_packages`, `logs`
7. Commit (save) the deleted folders.
8. Open a merge request using the git provider web interface. The merge request should attempt to merge the changes into the 'main' branch that all development branches are created from.
9. Follow the necessary procedures to get the branch approved and merged into the 'main' branch. You can delete the branch after the merge is complete.
10. Once the merge is complete, go back to the Studio IDE, and open the project that you're fixing.
11. [Rollback your repo to remote](https://docs.getdbt.com/docs/cloud/git/version-control-basics.md#the-git-button-in-the-cloud-ide) in the Studio IDE by clicking on the three dots next to the **Studio IDE Status** button on the lower right corner of the Studio IDE screen, then select **Rollback to remote**.
    <!-- -->
    * **Note** — Rollback to remote resets your repo back to an earlier clone from your remote. Any saved but uncommitted changes will be lost, so make sure you copy any modified code that you want to keep in a temporary location outside of dbt.
12. Once you rollback to remote, open the `.gitignore` file in the branch you're working in. If the new changes aren't included, you'll need to merge the latest commits from the main branch into your working branch.
13. Go to the **File Explorer** to verify the `.gitignore` file contains the correct entries and make sure the untracked files/folders in the .gitignore file are in *italics*.
14. Great job 🎉! You've configured the `.gitignore` correctly and can continue with your development!

For more info, refer to this [detailed video](https://www.loom.com/share/9b3b8e2b617f41a8bad76ec7e42dd014) for additional guidance.

How to migrate git providers

To migrate from one git provider to another, refer to the following steps to avoid minimal disruption:

1. Outside of dbt, you'll need to import your existing repository into your new provider. By default, connecting your repository in one account won't automatically disconnected it from another account.

   As an example, if you're migrating from GitHub to Azure DevOps, you'll need to import your existing repository (GitHub) into your new Git provider (Azure DevOps). For detailed steps on how to do this, refer to your Git provider's documentation (Such as [GitHub](https://docs.github.com/en/migrations/importing-source-code/using-github-importer/importing-a-repository-with-github-importer), [GitLab](https://docs.gitlab.com/ee/user/project/import/repo_by_url.html), [Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/repos/git/import-git-repository?view=azure-devops))

2. Go back to dbt and set up your [integration for the new Git provider](https://docs.getdbt.com/docs/cloud/git/git-configuration-in-dbt-cloud.md), if needed.

3. Disconnect the old repository in dbt by going to **Account Settings** and then **Projects**.

4. Click on the **Repository** link, then click **Edit** and **Disconnect**.

   [![Disconnect and reconnect your Git repository in your dbt Account settings page.](/img/docs/dbt-cloud/disconnect-repo.png?v=2 "Disconnect and reconnect your Git repository in your dbt Account settings page.")](#)Disconnect and reconnect your Git repository in your dbt Account settings page.

5. Click **Confirm Disconnect**.

6. On the same page, connect to the new Git provider repository by clicking **Configure Repository**

   * If you're using the native integration, you may need to OAuth to it.

7. That's it, you should now be connected to the new Git provider! 🎉

Note — As a tip, we recommend you refresh your page and Studio IDE before performing any actions.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
