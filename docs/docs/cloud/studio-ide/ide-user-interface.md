# IDE user interface

The [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md) is a tool for developers to effortlessly build, test, run, and version-control their dbt projects, and enhance data governance — all from the convenience of your browser. Use the Studio IDE to compile dbt code into SQL and run it against your database directly — no command line required!

This page offers comprehensive definitions and terminology of user interface elements, allowing you to navigate the Studio IDE landscape with ease.

[![The Studio IDE layout includes version control on the upper left, files/folders and search on the left, editor on the right, command palette at the top, and command/console at the bottom](/img/docs/dbt-cloud/cloud-ide/ide-basic-layout.png?v=2 "The Studio IDE layout includes version control on the upper left, files/folders and search on the left, editor on the right, command palette at the top, and command/console at the bottom")](#)The Studio IDE layout includes version control on the upper left, files/folders and search on the left, editor on the right, command palette at the top, and command/console at the bottom

## Basic layout[​](#basic-layout "Direct link to Basic layout")

The Studio IDE streamlines your workflow, and features a popular user interface layout with files and folders on the left, editor on the right, and command and console information at the bottom.

[![The Git repo link, documentation site button, Version Control menu, and File Explorer](/img/docs/dbt-cloud/cloud-ide/ide-side-menu.png?v=2 "The Git repo link, documentation site button, Version Control menu, and File Explorer")](#)The Git repo link, documentation site button, Version Control menu, and File Explorer

1. **Git repository link —** The Git repository link, located on the upper left of the Studio IDE, takes you to your repository on the same active branch. It also displays the repository name and the active branch name.

   * **Note:** This linking feature is only available for GitHub or GitLab repositories on multi-tenant dbt accounts.

2. **Documentation site button —** Clicking the Documentation site book icon, located next to the Git repository link, leads to the dbt Documentation site. The site is powered by the latest dbt artifacts generated in the IDE using the `dbt docs generate` command from the Command bar.

3. [**Version Control**](#editing-features) — The Studio IDE's powerful Version Control section contains all git-related elements, including the Git actions button and the **Changes** section.

4. **File explorer —** The File explorer shows the filetree of your repository. You can:

   * Click on any file in the filetree to open the file in the file editor.

   * Click and drag files between directories to move files.

   * Right-click a file to access the sub-menu options like duplicate file, copy file name, copy as `ref`, rename, delete.

   * Use file indicators, located to the right of your files or folder name, to see when changes or actions were made:

     <!-- -->

     * Unsaved (•) — The Studio IDE detects unsaved changes to your file/folder
     * Modification (M) — The Studio IDE detects a modification of existing files/folders
     * Added (A) — The Studio IDE detects added files
     * Deleted (D) — The Studio IDE detects deleted files.

[![Use the Command bar to write dbt commands, toggle 'Defer', and view the current IDE status](/img/docs/dbt-cloud/cloud-ide/ide-command-bar.png?v=2 "Use the Command bar to write dbt commands, toggle 'Defer', and view the current IDE status")](#)Use the Command bar to write dbt commands, toggle 'Defer', and view the current IDE status

5. **Command bar —** The Command bar, located in the lower left of the Studio IDE, is used to invoke [dbt commands](https://docs.getdbt.com/reference/dbt-commands.md). When a command is invoked, the associated logs are shown in the Invocation History Drawer.

6. **Defer to production —** The **Defer to production** toggle allows developers to only build and run and test models they've edited without having to first run and build all the models that come before them (upstream parents). Refer to [Using defer in dbt](https://docs.getdbt.com/docs/cloud/about-cloud-develop-defer.md#defer-in-the-dbt-cloud-ide) for more info.

7. **Status button —** The Studio IDE Status button, located on the lower right of the Studio IDE, displays the current Studio IDE status. If there is an error in the status or in the dbt code that stops the project from parsing, the button will turn red and display "Error". If there aren't any errors, the button will display a green "Ready" status. To access the [Studio IDE Status modal](#modals-and-menus), simply click on this button.

## Search bar and command palette[​](#search-bar-and-command-palette "Direct link to Search bar and command palette")

The Studio IDE provides tools to help you quickly navigate your project's files, find information, run commands, and replace syntax with just a few clicks in a layout that's familiar to users of popular IDEs.

[![Use the search bar and command palette to quickly navigate your file tree and open tabs.](/img/docs/dbt-cloud/cloud-ide/search-and-command.png?v=2 "Use the search bar and command palette to quickly navigate your file tree and open tabs.")](#)Use the search bar and command palette to quickly navigate your file tree and open tabs.

1. [Search and replace](#search-and-replace)
2. [Command palette](#command-palette)

### Search and replace[​](#search-and-replace "Direct link to Search and replace")

The search feature enables you to quickly find specific terms or phrases and replace them with the click of a button.

[![Search files for specific terms and quickly replace them.](/img/docs/dbt-cloud/cloud-ide/search-and-replace.png?v=2 "Search files for specific terms and quickly replace them.")](#)Search files for specific terms and quickly replace them.

1. Toggle between **file tree** and **search** navigation.
2. Search for words or phrases. Enhance the search to match case and/or whole words. You can also input replacement words or phrases. Click the icon next to the **Replace** field to replace all entries.
3. Navigate the search results. Click an entry to open the related file and highlight it on the screen. If you've entered replacement text, you'll see a preview of the new syntax. Click the symbol next to an entry to substitute the text with whatever is in the **Replace** field.

### Command palette[​](#command-palette "Direct link to Command palette")

The command palette enhances navigation of your dbt project, enabling you to search files, content, and symbols, show and run IDE commands, view recent files, and more. Click the command palette to view the available options. Actions supporting keyboard shortcuts display to the right of the text.

[![The command palette enables you to quickly navigate your project and run commands.](/img/docs/dbt-cloud/cloud-ide/command-palette.png?v=2 "The command palette enables you to quickly navigate your project and run commands.")](#)The command palette enables you to quickly navigate your project and run commands.

* **Go to File:** Search for files in your current project and open them in a new tab.
* **Show and Run Commands:** View and run commands related to IDE navigation and settings. Note: dbt commands (such as `run` and `build`) are available only in the [Command bar](#console-section) menu in the console; the command palette doesn't currently support them.
* **Search for Text:** Search for text across your project and either open files from the results or send results to the [search and replace](#search-and-replace) section for bulk changes.
* **Go to Symbol in Editor:** Quickly jump to symbols in the current file.
* **More:** Display advanced features such as **Go to Line/Column**, **Go to Symbol in Workspace**, and search within currently open files only.

[![Go to File.](/img/docs/dbt-cloud/cloud-ide/go-to-file.png?v=2 "Go to File.")](#)Go to File.

[![Show and Run Commands.](/img/docs/dbt-cloud/cloud-ide/show-and-run-commands.png?v=2 "Show and Run Commands.")](#)Show and Run Commands.

[![Search for text.](/img/docs/dbt-cloud/cloud-ide/search-for-text.png?v=2 "Search for text.")](#)Search for text.

[![Go to Symbol in Editor.](/img/docs/dbt-cloud/cloud-ide/go-to-symbol.png?v=2 "Go to Symbol in Editor.")](#)Go to Symbol in Editor.

[![More.](/img/docs/dbt-cloud/cloud-ide/more.png?v=2 "More.")](#)More.

## Editing features[​](#editing-features "Direct link to Editing features")

The Studio IDE features some delightful tools and layouts to make it easier for you to write dbt code and collaborate with teammates.

[![Use the file editor, version control section, and save button during your development workflow](/img/docs/dbt-cloud/cloud-ide/ide-editing.png?v=2 "Use the file editor, version control section, and save button during your development workflow")](#)Use the file editor, version control section, and save button during your development workflow

1. **File editor —** The file editor is where you edit code. Tabs break out the region for each opened file, and unsaved files are marked with a blue dot icon in the tab view. You can edit, format, or lint files and execute dbt commands in your protected primary git branch. Since the Studio IDE prevents commits to the protected branch, it prompts you to commit those changes to a new branch.

   * Use intuitive [keyboard shortcuts](https://docs.getdbt.com/docs/cloud/studio-ide/keyboard-shortcuts.md) to make development easier for you and your team.

2. **Save button —** The editor has a **Save** button that saves editable files. Pressing the button or using the Command-S or Control-S shortcut saves the file contents. You don't need to save to preview code results in the Console section, but it's necessary before changes appear in a dbt invocation. The file editor tab shows a blue icon for unsaved changes.

3. **Version Control —** This menu contains all git-related elements, including the Git actions button. The button updates relevant actions based on your editor's state, such as prompting to pull remote changes, commit and sync when reverted commit changes are present, creating a merge/pull request when appropriate, or pruning branches deleted from the remote repository.

   * The dropdown menu on the Git actions button allows users to revert changes, refresh Git state, create merge/pull requests, prune branches, and change branches.
   * You can also [resolve merge conflicts](https://docs.getdbt.com/docs/cloud/git/merge-conflicts.md) and for more info on git, refer to [Version control basics](https://docs.getdbt.com/docs/cloud/git/version-control-basics.md#the-git-button-in-the-cloud-ide).
   * **Version Control Options menu —** The **Changes** section, under the Git actions button, lists all file changes since the last commit. You can click on a change to open the Git Diff View to see the inline changes. You can also right-click any file and use the file-specific options in the Version Control Options menu.

[![Right-click edited files to access Version Control Options menu](/img/docs/dbt-cloud/cloud-ide/version-control-options-menu.png?v=2 "Right-click edited files to access Version Control Options menu")](#)Right-click edited files to access Version Control Options menu

* Use the **Prune branches** option to remove local branches that have already been deleted from the remote repository. Selecting this triggers a [pop-up modal](#prune-branches-modal), where you can confirm the deletion of the specific local branches, keeping your branch management tidy. Note that this won't delete the branch you're currently on. Pruning branches isn't available for [managed repositories](https://docs.getdbt.com/docs/cloud/git/managed-repository.md) because they don't have a typical remote setup, which prevents remote branch deletion.

## Additional editing features[​](#additional-editing-features "Direct link to Additional editing features")

* **Minimap —** A Minimap (code outline) gives you a high-level overview of your source code, which is useful for quick navigation and code understanding. A file's minimap is displayed on the upper-right side of the editor. To quickly jump to different sections of your file, click the shaded area.

[![Use the Minimap for quick navigation and code understanding](/img/docs/dbt-cloud/cloud-ide/ide-minimap.png?v=2 "Use the Minimap for quick navigation and code understanding")](#)Use the Minimap for quick navigation and code understanding

* **Git Diff View —** Clicking on a file in the **Changes** section of the **Version Control Menu** will open the changed file with Git Diff view. The editor will show the previous version on the left and the in-line changes made on the right.

[![The Git Diff View displays the previous version on the left and the changes made on the right of the Editor](/img/docs/dbt-cloud/cloud-ide/ide-git-diff-view-with-save.png?v=2 "The Git Diff View displays the previous version on the left and the changes made on the right of the Editor")](#)The Git Diff View displays the previous version on the left and the changes made on the right of the Editor

* **Markdown Preview console tab —** The Markdown Preview console tab shows a preview of your .md file's markdown code in your repository and updates it automatically as you edit your code.

[![The Markdown Preview console tab renders markdown code below the Editor tab.](/img/docs/dbt-cloud/cloud-ide/ide-markdown-with-save.png?v=2 "The Markdown Preview console tab renders markdown code below the Editor tab.")](#)The Markdown Preview console tab renders markdown code below the Editor tab.

* **CSV Preview console tab —** The CSV Preview console tab displays the data from your CSV file in a table, which updates automatically as you edit the file in your seed directory.

[![View CSV code in the CSV Preview console tab below the Editor tab.](/img/docs/dbt-cloud/cloud-ide/ide-csv.png?v=2 "View CSV code in the CSV Preview console tab below the Editor tab.")](#)View CSV code in the CSV Preview console tab below the Editor tab.

## Console section[​](#console-section "Direct link to Console section")

The console section, located below the file editor, includes various console tabs and buttons to help you with tasks such as previewing, compiling, building, and viewing the DAG. Refer to the following sub-bullets for more details on the console tabs and buttons.

[![The Console section is located below the file editor and has various tabs and buttons to help execute tasks](/img/docs/dbt-cloud/cloud-ide/ide-console-overview.png?v=2 "The Console section is located below the file editor and has various tabs and buttons to help execute tasks")](#)The Console section is located below the file editor and has various tabs and buttons to help execute tasks

1. **Preview button —** When you click on the **Preview** button, it runs the SQL in the active file editor regardless of whether you have saved it or not and sends the results to the **Results** console tab. You can preview a selected portion of saved or unsaved code by highlighting it and then clicking the **Preview** button.

Details

Row limits in IDE

The Studio IDE returns default row limits, however, you can also specify the number of records returned. Refer to the following sub-bullets for more info:

<br />

<br />

* **500-row limit:** To prevent the IDE from returning too much data and causing browser problems, dbt automatically sets a 500-row limit when using the **Preview Button**. You can modify this by adding `limit your_number` at the end of your SQL statement. For example, `SELECT * FROM` table `limit 100` will return up to 100 rows. Remember that you must write the `limit your_number` explicitly and cannot derive it from a macro.
* **Change row limit default:** In dbt version 1.6 or higher, you can change the default limit of 500 rows shown in the **Results** tab when you run a query. To adjust the setting you can click on **Change row display** next to the displayed rows. Keep in mind that you can't set it higher than 10,000 rows. If you refresh the page or close your development session, the default limit will go back to 500 rows.
* **Specify records returned:** The IDE also supports `SELECT TOP #`, which specifies the number of records to return.

2. **Compile button —** The **Compile** button compiles the saved or unsaved SQL code and displays it in the **Compiled code** tab.

Starting from dbt v1.6 or higher, when you save changes to a model, you can compile its code with the model's specific context. This context is similar to what you'd have when building the model and involves useful context variables like `{{ this }} `or `{{ is_incremental() }}`.

3. **Build button —** The build button allows users to quickly access dbt commands related to the active model in the file editor. The available commands include dbt build, dbt test, and dbt run, with options to include only the current resource, the resource and its upstream dependencies, the resource, and its downstream dependencies, or the resource with all dependencies. This menu is available for all executable nodes.

4. **Lint button** — The **Lint** button runs the [linter](https://docs.getdbt.com/docs/cloud/studio-ide/lint-format.md) on the active file in the file editor. The linter checks for syntax errors and style issues in your code and displays the results in the **Code quality** tab.

5. **dbt Copilot** — [dbt Copilot](https://docs.getdbt.com/docs/cloud/dbt-copilot.md) is a powerful artificial intelligence engine that generates documentation, data-tests, metrics, and semantic models for you. [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

6. **Commands tab** — View the most recently run [dbt commands](https://docs.getdbt.com/reference/dbt-commands.md) from your current IDE session, their results, and relevant system logs.

7. **Problems tab** — You must be running the dbt Fusion engine to utilize the problems tab. Gain insights into problems with your dbt project that may prevent it from running properly in Fusion as you edit and before you execute runs.

[![Preview results show up in the Results console tab](/img/docs/dbt-cloud/cloud-ide/ide-problems-tab.png?v=2 "Preview results show up in the Results console tab")](#)Preview results show up in the Results console tab

8. **Results tab** — The Results console tab displays the most recent Preview results in tabular format.

[![Preview results show up in the Results console tab](/img/docs/dbt-cloud/cloud-ide/results-console-tab.png?v=2 "Preview results show up in the Results console tab")](#)Preview results show up in the Results console tab

9. **Code quality tab** — The Code quality tab displays the results of the linter on the active file in the File editor. It allows you to view code errors, provides code quality visibility and management, and displays the SQLFluff version used.

10. **Compiled code tab —** The Compile generates the compiled code when the Compile button is executed. The Compiled code tab displays the compiled SQL code for the active file in the file editor.

[![Compile results show up in the Compiled Code tab](/img/docs/dbt-cloud/cloud-ide/compiled-code-console-tab.png?v=2 "Compile results show up in the Compiled Code tab")](#)Compile results show up in the Compiled Code tab

11. **Lineage tab —** The Lineage tab in the file editor displays the active model's lineage or DAG. By default, it shows two degrees of lineage in both directions (`2+model_name+2`), however, you can change it to +model+ (full DAG). To use the lineage:

    <!-- -->

    * Double-click a node in the DAG to open that file in a new tab
    * Expand or shrink the DAG using node selection syntax.
    * Note, the `--exclude` flag isn't supported.

[![View resource lineage in the Lineage tab](/img/docs/dbt-cloud/cloud-ide/lineage-console-tab.png?v=2 "View resource lineage in the Lineage tab")](#)View resource lineage in the Lineage tab

## Invocation history[​](#invocation-history "Direct link to Invocation history")

The Invocation History Drawer stores information on dbt invocations in the IDE. When you invoke a command, like executing a dbt command such as `dbt run`, the associated logs are displayed in the Invocation History Drawer.

You can open the drawer in multiple ways:

* Clicking the `^` icon next to the Command bar on the lower left of the page
* Typing a dbt command and pressing enter
* Or pressing Control-backtick (or Ctrl + \`)

[![The Invocation History Drawer returns a log and detail of all your dbt invocations.](/img/docs/dbt-cloud/cloud-ide/ide-inv-history-drawer.png?v=2 "The Invocation History Drawer returns a log and detail of all your dbt invocations.")](#)The Invocation History Drawer returns a log and detail of all your dbt invocations.

1. **Invocation History list —** The left-hand panel of the Invocation History Drawer displays a list of previous invocations in the Studio IDE, including the command, branch name, command status, and elapsed time.

2. **Invocation Summary —** The Invocation Summary, located above **System Logs**, displays information about a selected command from the Invocation History list, such as the command, its status (`Running` if it's still running), the git branch that was active during the command, and the time the command was invoked.

3. **System Logs toggle —** The System Logs toggle, located under the Invocation Summary, allows the user to see the full stdout and debug logs for the entirety of the invoked command.

4. **Command Control button —** Use the Command Control button, located on the right side, to control your invocation and cancel or rerun a selected run.

[![The Invocation History list displays a list of previous invocations in the IDE](/img/docs/dbt-cloud/cloud-ide/ide-results.png?v=2 "The Invocation History list displays a list of previous invocations in the IDE")](#)The Invocation History list displays a list of previous invocations in the IDE

5. **Node Summary tab —** Clicking on the Results Status Tabs will filter the Node Status List based on their corresponding status. The available statuses are Pass (successful invocation of a node), Warn (test executed with a warning), Error (database error or test failure), Skip (nodes not run due to upstream error), and Queued (nodes that have not executed yet).

6. **Node result toggle —** After running a dbt command, information about each executed node can be found in a Node Result toggle, which includes a summary and debug logs. The Node Results List lists every node that was invoked during the command.

7. **Node result list —** The Node result list shows all the Node Results used in the dbt run, and you can filter it by clicking on a Result Status tab.

## Modals and Menus[​](#modals-and-menus "Direct link to Modals and Menus")

Use menus and modals to interact with Studio IDE and access useful options to help your development workflow.

#### Editor tab menu[​](#editor-tab-menu "Direct link to Editor tab menu")

To interact with open editor tabs, right-click any tab to access the helpful options in the file tab menu.

[![ Right-click a tab to view the Editor tab menu options](/img/docs/dbt-cloud/cloud-ide/editor-tab-menu-with-save.png?v=2 " Right-click a tab to view the Editor tab menu options")](#) Right-click a tab to view the Editor tab menu options

#### Global command shortcut[​](#global-command-shortcut "Direct link to Global command shortcut")

The global command shortcut provides helpful shortcuts to interact with the Studio IDE, such as git actions, specialized dbt commands, and compile, and preview actions, among others. To open the menu, use Command-P or Control-P.

[![The Command History returns a log and detail of all your dbt invocations.](/img/docs/dbt-cloud/cloud-ide/ide-global-command-palette-with-save.png?v=2 "The Command History returns a log and detail of all your dbt invocations.")](#)The Command History returns a log and detail of all your dbt invocations.

#### Studio IDE Status modal[​](#-status-modal "Direct link to -status-modal")

The Studio IDE Status modal shows the current error message and debug logs for the server. This also contains an option to restart the Studio IDE. Open this by clicking on the Studio IDE Status button.

[![The Command History returns a log and detail of all your dbt invocations.](/img/docs/dbt-cloud/cloud-ide/ide-status-modal-with-save.png?v=2 "The Command History returns a log and detail of all your dbt invocations.")](#)The Command History returns a log and detail of all your dbt invocations.

#### Commit to a new branch[​](#commit-to-a-new-branch "Direct link to Commit to a new branch")

Edit directly on your protected primary git branch and commit those changes to a new branch when ready.

[![Commit changes to a new branch](/img/docs/dbt-cloud/using-dbt-cloud/create-new-branch.png?v=2 "Commit changes to a new branch")](#)Commit changes to a new branch

#### Commit Changes modal[​](#commit-changes-modal "Direct link to Commit Changes modal")

The Commit Changes modal is accessible via the Git Actions button to commit all changes or via the Version Control Options menu to commit individual changes. Once you enter a commit message, you can use the modal to commit and sync the selected changes.

[![The Commit Changes modal is how users commit changes to their branch.](/img/docs/dbt-cloud/cloud-ide/commit-changes-modal.png?v=2 "The Commit Changes modal is how users commit changes to their branch.")](#)The Commit Changes modal is how users commit changes to their branch.

#### Change Branch modal[​](#change-branch-modal "Direct link to Change Branch modal")

The Change Branch modal allows users to switch git branches in the Studio IDE. It can be accessed through the **Change Branch** link or the **Git actions** button under the **Version control** menu.

[![The Commit Changes modal is how users change their branch.](/img/docs/dbt-cloud/cloud-ide/change-branch-modal.png?v=2 "The Commit Changes modal is how users change their branch.")](#)The Commit Changes modal is how users change their branch.

#### Prune branches modal[​](#prune-branches-modal "Direct link to Prune branches modal")

The Prune branches modal allows users to delete local branches that have been deleted from the remote repository, keeping your branch management tidy. This is accessible through the **Git actions** button under the [**Version control** menu](#editing-features). Note that this won't delete the branch you're currently on. Pruning branches isn't available for managed repositories because they don't have a typical remote setup, which prevents remote branch deletion.

[![The Prune branches modal allows users to delete local branches that have already been deleted from the remote repository.](/img/docs/dbt-cloud/cloud-ide/prune-branch-modal.png?v=2 "The Prune branches modal allows users to delete local branches that have already been deleted from the remote repository.")](#)The Prune branches modal allows users to delete local branches that have already been deleted from the remote repository.

#### Revert Uncommitted Changes modal[​](#revert-uncommitted-changes-modal "Direct link to Revert Uncommitted Changes modal")

The Revert Uncommitted Changes modal is how users revert changes in the IDE. This is accessible via the `Revert File` option above the Version Control Options menu, or via the Git Actions button when there are saved, uncommitted changes in the IDE.

[![The Commit Changes modal is how users change their branch.](/img/docs/dbt-cloud/cloud-ide/revert-uncommitted-changes-with-save.png?v=2 "The Commit Changes modal is how users change their branch.")](#)The Commit Changes modal is how users change their branch.

#### Studio IDE Options menu[​](#-options-menu "Direct link to -options-menu")

Access the Studio IDE Options menu by clicking the three-dot menu located at the bottom right corner of the Studio IDE. This menu contains global options:

* View status details, including the Studio IDE Status modal
* Restart the Studio IDE
* Reinstall dependencies
* Clean dbt project
* [Check & fix deprecations](https://docs.getdbt.com/docs/cloud/studio-ide/autofix-deprecations.md)
* Rollback your repo to remote to refresh your git state and view status details

[![Access the IDE options menu to switch to dark or light mode, restart the IDE, rollback to remote, or view the IDE status](/img/docs/dbt-cloud/cloud-ide/ide-options-menu-with-save.png?v=2 "Access the IDE options menu to switch to dark or light mode, restart the IDE, rollback to remote, or view the IDE status")](#)Access the IDE options menu to switch to dark or light mode, restart the IDE, rollback to remote, or view the IDE status

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
