# Sign in or register [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

The dbt VS Code extension comes with a suite of features that are available to all users for 14 days. After the 14-day trial, sign in or register for a dbt platform account to keep using all features, including advanced features such as [live preview for models and CTEs](https://docs.getdbt.com/docs/dbt-extension-features.md#live-preview-for-models-and-ctes), [column-level lineage](https://docs.getdbt.com/docs/dbt-extension-features.md#rich-lineage-in-context), and more.

Refer to [VS Code extension features](https://docs.getdbt.com/docs/fusion/fusion-availability.md?version=1.13#dbt-vs-code-extension-features) for the full list of features and their availability.

Most features remain available without signing in — only advanced features prompt you to sign in after the trial ends.

<!-- -->

Run [`dbt login status`](https://docs.getdbt.com/reference/commands/login.md#dbt-login-status) to view your currently authenticated status.

## Key points[​](#key-points "Direct link to Key points")

* The extension is free for organizations for up to 15 users. Refer to the [acceptable use policy](https://www.getdbt.com/dbt-assets/vscode-plugin-aup).
* Registration links your editor to your registered dbt platform account so you can keep using gated features after the 14-day trial.
* You can authenticate with `dbt login` or register from inside the extension. If you don't have a dbt platform account, you can create a free account during authentication to unlock advanced features.
* If a valid [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) file exists on your machine, the extension can use it automatically.

If you've registered before, you won't need to authenticate again unless your session expired.

## Session expiry and re-authorization[​](#session-expiry-and-re-authorization "Direct link to Session expiry and re-authorization")

Your sign-in session stays active across editor restarts. dbt automatically renews your session while you're using the extension or the CLI. You may need to re-authorize in a few specific cases:

* **Session expired after 7 days of inactivity.** The extension shows the message: "Your dbt extension session expired. Sign in again to continue using the extension." Click the prompt or run **dbt: Register dbt extension** from the command palette to sign in again.
* **A feature needs broader access than your initial sign-in granted.** [`dbt login`](https://docs.getdbt.com/reference/commands/login.md) and the dbt VS Code extension may request different sets of permissions. If you signed in via `dbt login` and later use an extension feature that needs more access (for example, the catalog tab or running a job), the extension prompts you to re-authorize. You'll re-authorize with the same dbt platform account — you don't need a second account.
* **You authenticated with `dbt_cloud.yml` instead of OAuth.** File-based credentials don't expire on the 7-day inactivity rule. You only re-authenticate if you regenerate the file or it's removed.

When re-authorizing, the **Authorize dbt login** consent screen lists the access being requested. Click **Allow access** to continue.

## Choose a sign-in path[​](#choose-a-sign-in-path "Direct link to Choose a sign-in path")

There are a couple of different ways to sign in or register for a dbt platform account. Choose the best path for you:

| If you...                                                             | Use this path                                                                                                                                                       |
| --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Starting authentication from the terminal                             | [`dbt login`](https://docs.getdbt.com/docs/sign-in-dbt-extension.md?registration-path=dbt-login#choose-a-sign-in-path) from the command line.                       |
| Are new to dbt and don't have a dbt platform account                  | [First-time registration](https://docs.getdbt.com/docs/sign-in-dbt-extension.md?registration-path=first-time#choose-a-sign-in-path) from the dbt VS Code extension. |
| Can't sign in or need to reset access                                 | [Recover your login](https://docs.getdbt.com/docs/sign-in-dbt-extension.md?registration-path=recover-login#choose-a-sign-in-path) from the dbt VS Code extension.   |
| Prefer file-based authentication to remove need for re-authenticating | [`dbt_cloud.yml`](https://docs.getdbt.com/docs/sign-in-dbt-extension.md?registration-path=dbt-cloud-yml#choose-a-sign-in-path) from the dbt VS Code extension.      |

* dbt login
* First-time registration
* Existing account sign-in
* Recover your login
* dbt\_cloud.yml

### dbt login

`dbt login` is available in dbt Core v2.0 and later. To install the dbt VS Code extension, [upgrade your project to the dbt Fusion engine](https://docs.getdbt.com/docs/upgrade-to-fusion-extension.md).

Use this path if you prefer the terminal or want a single auth flow that works across the CLI and the dbt VS Code extension. Running `dbt login` opens the same browser flow that the extension uses when you select **dbt: Register dbt extension** from the command palette, so completing it once authenticates both.

1. From your terminal, run:

   ```shell
   dbt login
   ```

2. Complete the sign-in or sign-up flow in your browser.

   * Refer to [First-time registration](https://docs.getdbt.com/docs/sign-in-dbt-extension.md?registration-path=first-time#first-time-registration) if you've never registered
   * Refer to [Existing account sign-in](https://docs.getdbt.com/docs/sign-in-dbt-extension.md?registration-path=existing-account#existing-dbt-account) if you already have a dbt account.
   * Refer to [Recover your login](https://docs.getdbt.com/docs/sign-in-dbt-extension.md?registration-path=recover-login#recover-your-login) if you've forgotten your password or your account is locked.

3. On the **Authorize dbt login** screen, click **Allow access**. dbt saves your credentials to your local dbt configuration directory and uses them for future CLI commands:

   * macOS and Linux: `~/.dbt/`
   * Windows: `C:\Users\[username]\.dbt\`

   [![The Authorize VS Code Extension consent screen.](/img/docs/extension/signin-authorize-extension.png?v=2 "The Authorize VS Code Extension consent screen.")](#)The Authorize VS Code Extension consent screen.

4. Return to your editor and restart the dbt VS Code extension, or open a new editor window.

5. The extension detects the credentials and shows **Registered**.

Authorizing additional access

When you register in the dbt VS Code extension, you might be prompted to authorize more access than you granted when signing in through the CLI (or the other way around). This is expected — some extension features need broader access than your initial sign-in. You can authorize the request with the same dbt platform account you already use, so a single account covers both the CLI and the dbt VS Code extension.

For more details on when re-authorization is triggered, refer to [Session expiry and re-authorization](https://docs.getdbt.com/docs/sign-in-dbt-extension.md#session-expiry-and-re-authorization).

You've now unlocked the full feature set of the dbt VS Code extension. For details on what `dbt login` unlocks across tools, refer to the [`dbt login` reference](https://docs.getdbt.com/reference/commands/login.md).

### First-time registration

Register to unlock the full [feature set](https://docs.getdbt.com/docs/dbt-extension-features.md) of the dbt VS Code extension. Use this path if you've never registered before. The extension and the CLI open the same browser flow, so you can start from either entry point:

* **From the extension:** Click the registration prompt, or open the command palette (Windows/Linux: Ctrl + Shift + P, macOS: Cmd + Shift + P) and run **dbt: Register dbt extension**.

  [![The extension registration prompt in VS Code.](/img/docs/extension/registration-prompt.png?v=2 "The extension registration prompt in VS Code.")](#)The extension registration prompt in VS Code.

* **From the CLI:** Run `dbt login` in your terminal.

After you start the flow, complete the browser steps:

1. On the **Log in to dbt platform** page, enter your email and click **Continue**.

2. On the **Create your dbt platform** page, enter your name, create a password, then click **Continue**. dbt creates a free account for you.

3. On the **Authorize dbt login** screen, review the requested access. Leave **Keep session alive** on if you want dbt to renew your session automatically, then click **Allow access**.

   [![The Authorize dbt login consent screen.](/img/docs/extension/signin-authorize-extension.png?v=2 "The Authorize dbt login consent screen.")](#)The Authorize dbt login consent screen.

4. Return to your editor. The extension shows **Registered**. If you started from the CLI, your credentials are also saved for future CLI commands.

5. Continue with the [Get started](https://docs.getdbt.com/docs/install-dbt-extension.md#getting-started) workflow.

Authorizing additional access

When you register in the dbt VS Code extension, you might be prompted to authorize more access than you granted when signing in through the CLI (or the other way around). This is expected — some extension features need broader access than your initial sign-in. You can authorize the request with the same dbt platform account you already use, so a single account covers both the CLI and the dbt VS Code extension.

For more details on when re-authorization is triggered, refer to [Session expiry and re-authorization](https://docs.getdbt.com/docs/sign-in-dbt-extension.md#session-expiry-and-re-authorization).

### Existing account sign-in

Use this path if you've registered before, including with an older or inactive registration. The extension and the CLI open the same browser flow, so you can start from either entry point:

* **From the extension:** [Update the VS Code extension](https://code.visualstudio.com/docs/setup/setup-overview#_update-cadence) to the latest version, restart your editor, then click the registration prompt or run **dbt: Register dbt extension** from the command palette.
* **From the CLI:** Run `dbt login` in your terminal.

After you start the flow, complete the browser steps:

1. On the **Sign in to dbt** page, enter the email you used to register, agree to the terms of service, complete the "I'm not a robot" check, then click **Continue**.

2. On the **Verify your email** page, enter the verification code dbt sent to your email and click **Continue**.

3. If your email is linked to more than one account, select the account you want to use on the account selection page.

4. If your account uses an authenticator app, choose a method to verify your identity, then complete the prompt.

   [![The verify-your-identity screen for accounts that use an authenticator app.](/img/docs/extension/signin-verify-identity.png?v=2 "The verify-your-identity screen for accounts that use an authenticator app.")](#)The verify-your-identity screen for accounts that use an authenticator app.

5. On the **Authorize dbt login** screen, review the requested access and click **Allow access**.

**When you might still need a [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md):**

* You want a file-based credential for automations or defer.
* You use platform features that require `dbt_cloud.yml`, such as Mesh or auto-deferral.

Authorizing additional access

When you register in the dbt VS Code extension, you might be prompted to authorize more access than you granted when signing in through the CLI (or the other way around). This is expected — some extension features need broader access than your initial sign-in. You can authorize the request with the same dbt platform account you already use, so a single account covers both the CLI and the dbt VS Code extension.

For more details on when re-authorization is triggered, refer to [Session expiry and re-authorization](https://docs.getdbt.com/docs/sign-in-dbt-extension.md#session-expiry-and-re-authorization).

### Recover your login

Use this path if the login flow says you have an account but you don't remember your password.

1. On the **Sign in to dbt** page, click **Reset password**.
2. On the reset-password screen, enter the email you used to register.
3. Check your inbox for the password-reset email and follow the link.
4. Set a new password.
5. Return to your terminal and run `dbt login` again. The password reset opens a separate browser session from the original terminal login session, so you need to restart the login flow.
6. On the **Sign in to dbt** page, sign in with the email and new password you just set, agree to the terms of service, complete the "I'm not a robot" check, then click **Continue**.
7. If your email is linked to more than one account, select the account you want to use.
8. On the **Authorize dbt login** screen, click **Allow access**. When authorization completes, the flow returns to your terminal and the dbt VS Code extension.

If you still can't sign in, your account may be locked. Contact [dbt Support](mailto:support@getdbt.com) to unlock your account, then continue with the **Existing account sign-in** flow.

Authorizing additional access

When you register in the dbt VS Code extension, you might be prompted to authorize more access than you granted when signing in through the CLI (or the other way around). This is expected — some extension features need broader access than your initial sign-in. You can authorize the request with the same dbt platform account you already use, so a single account covers both the CLI and the dbt VS Code extension.

For more details on when re-authorization is triggered, refer to [Session expiry and re-authorization](https://docs.getdbt.com/docs/sign-in-dbt-extension.md#session-expiry-and-re-authorization).

### Register with `dbt_cloud.yml`

Use this path if your workflow requires a credential file or if you want to remove the need to re-authenticate when sessions expire.

`dbt_cloud.yml` is a separate, complementary authentication path. You may still need `dbt_cloud.yml` for dbt platform features such as Mesh and auto-deferral.

1. Log in to dbt platform.

2. Go to **Account settings** → **VS Code extension**.

3. In the **Set up your credentials** section, click **Download credentials** to get your [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) file.

   [![Download the dbt\_cloud.yml file from your dbt platform account.](/img/docs/extension/download-registration-2.png?v=2 "Download the dbt_cloud.yml file from your dbt platform account.")](#)Download the dbt\_cloud.yml file from your dbt platform account.

4. Move the file into your `.dbt` directory:

   * macOS and Linux: `~/.dbt/dbt_cloud.yml`
   * Windows: `C:\Users\[username]\.dbt\dbt_cloud.yml`

5. Return to your editor, open the command palette, and run **dbt: Register dbt extension**.

6. The extension detects the credential file and continues the registration flow.

**Behavior details:**

* If a [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) file exists, the extension uses it automatically.
* If the file is missing, the extension prompts you to sign in or add the file.

If your dbt project uses environment variables, configure them locally so the extension can use the same values as your dbt environment. For more information, refer to [Configure environment variables](https://docs.getdbt.com/docs/configure-dbt-extension.md).

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

If you run into any issues, check out the troubleshooting section below.

 How to create a .dbt directory in root and move dbt\_cloud.yml file

If you've never had a `.dbt` directory, you should perform the following recommended steps to create one. If you already have a `.dbt` directory, move the `dbt_cloud.yml` file into it. Some information about the `.dbt` directory:

* A `.dbt` directory is a hidden folder in the root of your filesystem. It's used to store your dbt configuration files. The `.` prefix is used to create a hidden folder, which means it's not visible in Finder or File Explorer by default.
* To view hidden files and folders, press Command + Shift + G on macOS or Ctrl + Shift + G on Windows. This opens the "Go to Folder" dialog where you can search for the `.dbt` directory.

- Create a .dbt directory
- Move the dbt\_cloud.yml file

1. Clone your dbt project repository locally.
2. Use the `mkdir` command followed by the name of the folder you want to create.

* If using macOS, add the `~` prefix to create a `.dbt` folder in the root of your filesystem:

  <!-- -->

  * macOS: `mkdir ~/.dbt`
  * Windows: `mkdir %USERPROFILE%\.dbt`

You can move the `dbt_cloud.yml` file into the `.dbt` directory using the `mv` command or by dragging and dropping the file into the `.dbt` directory by opening the Downloads folder using the "Go to Folder" dialog and then using drag-and-drop in the UI.

To move the file using the terminal, use the `mv/move` command. This command moves the `dbt_cloud.yml` from the `Downloads` folder to the `.dbt` folder. If your `dbt_cloud.yml` file is located elsewhere, adjust the path accordingly.

#### Mac or Linux[​](#mac-or-linux "Direct link to Mac or Linux")

In your command line, use the `mv` command to move your `dbt_cloud.yml` file into the `.dbt` directory. If you've just downloaded the `dbt_cloud.yml` file and it's in your Downloads folder, the command might look something like this:

```bash
mv ~/Downloads/dbt_cloud.yml ~/.dbt/dbt_cloud.yml
```

#### Windows[​](#windows "Direct link to Windows")

In your command line, use the move command. Assuming your file is in the Downloads folder, the command might look like this:

```bash
move %USERPROFILE%\Downloads\dbt_cloud.yml %USERPROFILE%\.dbt\dbt_cloud.yml
```

 I can't see the lineage tab in Cursor

If you're using the dbt VS Code extension in Cursor, the lineage tab works best in Editor mode and doesn't render in Agent mode. If you're in Agent mode and the lineage tab isn't rendering, just switch to Editor mode to view your project's table and column lineage.

 The extension gets stuck in a loading state

If the extension is attempting to activate during startup and locks into a permanent loading state, check that:

* Your dbt VS Code extension is on the latest version.
* Your IDE is on the latest version.
* You have a valid `dbt_cloud.yml` file configured and in the [correct location](#register-with-dbt_cloudyml).

If you're still experiencing issues, try these steps before contacting dbt Support:

* Delete and download a new copy of your `dbt_cloud.yml` file.
* Delete and reinstall the dbt VS Code extension.

 dbt platform configurations

If you're a cloud-based dbt platform user who has the `dbt-cloud:` config in the `dbt_project.yml` file and are also using dbt Mesh, you must have the project ID configured:

```yaml
dbt-cloud:
project-id: 12345 # Required
```

If you don’t configure this correctly, cross-platform references will not resolve properly, and you will encounter errors executing dbt commands.

 dbt extension not activating

If the dbt extension has activated successfully, you will see the **dbt Extension** label in the status bar at the bottom left of your editor. You can view diagnostic information about the dbt extension by clicking the **dbt Extension** button.

If the **dbt Extension** label is not present, then it is likely that the dbt extension was not installed successfully. If this happens, try uninstalling the extension, restarting your editor, and then reinstalling the extension.

**Note:** It is possible to "hide" status bar items in VS Code. Double-check if the dbt Extension status bar label is hidden by right-clicking on the status bar in your editor. If you see dbt Extension in the right-click menu, then the extension has installed successfully.

 Missing dbt LSP features

If you receive a `no active LSP for this workspace` error message or aren't seeing dbt Language Server (LSP) features in your editor (like autocomplete, go-to-definition, or hover text), start by first following the general troubleshooting steps mentioned earlier.

If you've confirmed the dbt extension is installed correctly but don't see LSP features, try the following:

1. Check extension version — Ensure that you're using the latest available version of the dbt extension by:

   <!-- -->

   * Opening the **Extensions** page in your editor, or
   * Going to the **Output** tab and looking for the version number, or
   * Running `dbtf --version` in the terminal.

2. Reinstall the LSP — If the version is correct, reinstall the LSP:

   <!-- -->

   1. Open the Command Palette: Command + Shift + P (macOS) or Ctrl + Shift + P (Windows/Linux).
   2. Paste `dbt: Reinstall dbt LSP` and enter.

This command downloads the LSP and re-activates the extension to resolve the error.

 Unsupported dbt version

If you see an error message indicating that your version of dbt is unsupported, then there is likely a problem with your environment.

Check the dbt Path setting in your VS Code settings. If this path is set, ensure that it is pointing to a valid dbt Fusion Engine executable. If necessary, you can also install the dbt Fusion engine directly using these instructions: [Install the Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)

 Addressing the 'dbt language server is not running in this workspace' error

To resolve the `dbt language server is not running in this workspace` error, you need to add your dbt project folder to a workspace:

1. In VS Code, click **File** in the toolbar then select **Add Folder to Workspace**.
2. Select the dbt project file you want to add to a workspace.
3. To save your workspace, click **File** then select **Save Workspace As**.
4. Navigate to the location you want to save your workspace.

This should resolve the error and open your dbt project by opening the workspace it belongs to. For more information on workspaces, refer to [What is a VS Code workspace?](https://code.visualstudio.com/docs/editing/workspaces/workspaces).

 Manifest cannot be downloaded from the dbt platform

If the dbt VS Code extension cannot download the manifest from the dbt platform or you get `warning: dbt1200: Failed to download manifest` using Fusion locally, you are probably having DNS-related issues.

To confirm this, do a DNS lookup for the host Fusion is trying to download from (for example, prodeu2.blob.core.windows.net) by using `dig` on Linux/Mac or `nslookup` on Windows.

If this doesn't return an IP address, the likely reason is that your company uses the same cloud provider with private endpoints for cloud resources, and DNS requests for these are forwarded to private DNS zones.

This situation can be remedied by setting up an internet fallback, which will then return a public IP to any cloud storage that does not have a private IP registered with the private DNS zone.

For Azure refer to [Fallback to internet for Azure Private DNS zones](https://learn.microsoft.com/en-us/azure/dns/private-dns-fallback).

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)
