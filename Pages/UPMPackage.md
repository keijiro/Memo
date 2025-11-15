# UPM Package Signature (Unity 6.3)

Starting with Unity 6.3, the Package Manager shows warnings for packages
without signatures.

There are a few options to avoid those warnings:

1. Migrating to [UPM Publishing on Asset Store].
2. Add signatures to packages using the signing tool.
3. Do nothing. Ask users to ignore the warning.

[UPM Publishing on Asset Store]:
  https://assetstore.unity.com/publishing/upm-publishing

Although UPM Publishing might be the most user-friendly and safest approach, it
requires a Unity review. Unfortunately, my packages never meet the quality
requirements for it.

“Do nothing” could be a practical option. Although seeing warnings every time
in the Unity Editor is irritating, advanced users can bear with it.

Fortunately, adding a signature to a package using the signing tool is pretty
easy. So I decided to update my package workflow to support signing.

## Exporting Signed Package: GUI or CLI

There are two methods to sign a package:

- GUI: Export a signed tarball using Package Manager GUI
- CLI: Sign a package using command line options of the Unity Editor executable

The problem with the CLI method is that it needs a password via a command line
option. I use my Google account to sign in with my Unity ID, so setting up a
dedicated password just for this purpose feels inconvenient.

For now, I decided to use the GUI for signing. It’s pretty easy to use—just
press the “Export” button and select an organization.

## Automation using AI coding agents

Usually, I use a shell script to automate this kind of process. This time, I
chose to use AI coding agents like Claude Code or Gemini CLI instead of it.

I wrote an instruction sheet for AI agents. It describes the process and
structure of my packages:

https://github.com/keijiro/UnityCustomPackageTest/blob/master/AGENT.md

Using this sheet, I simply ask them to assemble release notes, update the
package manifest, and push the result to GitHub and npmjs.com.
