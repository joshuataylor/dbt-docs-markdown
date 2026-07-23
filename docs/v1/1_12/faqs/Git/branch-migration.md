# Branches when changing Git providers or repositories

This FAQ explains how Git branch names and commit history carry over when you switch Git providers (for example, GitHub or GitLab), start using a new repository, or reconnect your project to a different remote. It is for dbt Core and dbt platform users: general Git behavior about remotes, branches, and history applies to both; sections on the Studio IDE, managed repositories, and disconnecting a project repository apply only to the dbt platform.

Your branch names and history only appear on a new Git provider if you copied the entire repository with its history. If you only copied files or created a fresh repository, old branches do not show up. If you use the dbt platform and the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md), work you saved there but have not pushed is separate and could be lost.

The following sections cover common scenarios, what to expect in each case, and what action to take to keep your branches and history intact:

 Git provider migration

If you fully migrate the same repository to a new Git provider so all commits and branches are preserved, your branches and history typically carry over.

If you only copy files, create a new repository with a single initial commit, or import only the default branch, other branches will not appear on the new remote until you push or import them.

For steps to disconnect and reconnect your repository in the dbt platform, refer to [How to migrate git providers](https://docs.getdbt.com/faqs/Git/git-migration.md).

 Branches missing after creating a new repository

Branches may not appear after you create a new repository.

When you create a brand-new repository, it starts with only the commits and branches that exist in that new remote. Branches from your previous repository do not appear automatically, even if the files look similar.

If you need earlier branches and commit history, migrate the original repository so the full commit history and branches are retained. If you continue with a newly created repository, push any branches you want to keep from your local clone.

If you use the dbt platform and need to disconnect or reconnect your project repository, follow [How to migrate git providers](https://docs.getdbt.com/faqs/Git/git-migration.md).

 Project repository connection

In the dbt platform, the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md) shows branches from the Git remote connected to your project. It does not maintain a second, independent copy of your full commit history and branches.

Before you disconnect the repository integration in dbt platform or switch to a different remote, save and push work you need to keep. Uncommitted changes are at risk until you **Commit and sync** (push). Until then they will not appear in your Git provider (for example, GitHub).

For disconnect steps, refer to [How to migrate git providers](https://docs.getdbt.com/faqs/Git/git-migration.md).

Unsaved edits in the browser can also be lost. **Commit and sync** what you need to keep, or copy it outside the Studio IDE. For information on how work is stored in the Studio IDE, refer to [Work retention](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md?version=1.12#about-the-start-up-process-and-work-retention).

Pre-cutover verification

To confirm branches and connectivity before switching over, create a separate project in the dbt platform pointed at the new repository, verify the branches you expect, then disconnect the old repository and remove the temporary project when finished.

 From managed to self-hosted

Exporting a project as a ZIP is a snapshot of files only and does not preserve your Git history or branches. To migrate properly and keep your full history, follow [Move from a managed repository to a self-hosted repository](https://docs.getdbt.com/faqs/Git/managed-repo.md).
