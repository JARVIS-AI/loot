Contributing To LOOT
====================

A [general guide](https://loot.github.io/docs/contributing/How-To-Contribute) to contributing to LOOT may be found on LOOT's website. Information more specific to this repository is found here.

## Repository Branching Structure

The repository branching structure is pretty simple:

* The `master` branch holds code that's released or that will be in the next
  release. Code on `master` is generally pretty stable as CI must pass before
  anything can be merged into it.
* Other branches are generally themed on specific features or groups of changes,
  and come and go as they are merged into one of the two above, or discarded.

## Getting Involved

The best way to get started is to comment on something in GitHub's commit log or on the [issue tracker](https://github.com/loot/loot/issues) (new tracker entries are always welcome).

Surprise pull requests aren't recommended because everything you touched may have been rewritten, making your changes a pain to integrate, or obsolete. There are only generally a few contributors at any one time though, so there's not much chance of painful conflicts requiring resolution, provided you're working off the correct branch.

When you do make a pull request, please do so from a branch which doesn't have the same name as they branch you're requesting your changes to be merged into. It's a lot easier to keep track of what pull request branches do when they're named something like `you:specific-cool-feature` rather than `you:master`.

## Translating LOOT

### General Information

LOOT supports translation into other languages, with the following limitations:

* Debug log messages and error messages generated by the libraries LOOT uses cannot be translated.
* Masterlist messages can be translated, but translations must be submitted to the masterlist maintainers for addition. Translating masterlist messages won't be covered here.
* The languages LOOT supports is defined in LOOT's settings, so new translations can be added without updating LOOT. However, LOOT's default settings are hardcoded, so LOOT must be updated to support new translations by default.

This guide assumes you have a basic understanding of how to use a Git client and GitHub to submit your changes. If you don't, don't worry! You can still contribute; simply follow the previously linked [guide](https://loot.github.io/docs/contributing/How-To-Contribute) to learn the basic principles needed. If you're having any trouble, feel free to ask a team member for help, such as in the **#dev-questions** or **#translations** channels in our [Discord](https://loot.github.io/discord/) or at our dedicated [localization issue](https://github.com/loot/loot.github.io/issues/77). If you're still uncomfortable with the process, we can always submit your translations for you, but we encourage you to try submitting them yourself first.

To translate everything but masterlist messages, first fork this repository. All file paths given below are relative to its base folder.

Keep in mind that you don't need to submit a perfect pull request; just follow the instructions below to the best of your ability and we will correct any coding-related mistakes when we review your submission.

### Translating the Installer

First check that an [Inno Setup translation](http://www.jrsoftware.org/files/istrans/) exists for your language. Unofficial translations are acceptable, but require a bit of extra handling. If there isn't an official or unofficial translation for Inno Setup, you're better off making a translation and getting it listed on the linked page before continuing.

1. Open the installer script at `scripts/installer.iss` in a text editor of your choice.
2. If your language only has an unofficial translation, add a `#define <Language>Exists` block for it near the top of the script, like it has been done for Korean and Simplified Chinese.
3. Add your language to the `[Languages]` section. The `Name` should be the [POSIX locale code](https://www.gnu.org/software/gettext/manual/html_node/Locale-Names.html) for your language. The `MessagesFile` filename is the filename of the Inno Setup translation that you checked exists. If your language only has an unofficial translation, wrap its line in `#ifdef` and `#endif` lines, again like it has been done for Korean and Simplified Chinese.
4. Translate the string(s) in the `[CustomMessages]` into your language, following the example of the existing translations. Again, if your language only has an unofficial translation, wrap its line(s) in `#ifdef` and `#endif` lines.
5. Save your changes.

If your language's Inno Setup translation is unofficial, also do the following:

1. Open the script at `scripts/build_installer.ps1` in a text editor of your choice.
2. Add an entry for your language's translation file to the `$unofficialLanguageFiles` array.
3. Save your changes.

### Translating the LOOT application

1. Download and install the latest version of [Poedit](https://poedit.net/).
2. If you are starting a new translation, select `Create new...` on the welcome page or `File -> New from POT/PO file...` and select the template file at `resources/l10n/template.pot`. Select your language from the drop-down list, specifying a region/dialect if desired, and click `OK`.
3. If you are updating a previous translation, in Poedit, open the `loot.po` translation file in the relevant subdirectory of `resources/l10n`, then select `Translation -> Update from POT file...` and select the template file at `resources/l10n/template.pot`. Click `OK` in the `Update summary` dialog if it appears.
4. Edit the translation file to add or update translations of the programs' text. Strings that were added since the last translation will be missing a translation, and strings that have been changed since the last translation will be highlighted in orange.
5. Go to `Translation -> Properties... -> Translation Properties -> Project name and version:` and check that it matches the latest version of LOOT. If it doesn't, please update it. Then go to `File -> Preferences -> General -> Information about the translator` and fill the `Name:` field with your name/alias. If you don't mind, you can also fill out the `Email:` field so that we or future translators can contact you if need be.
6. Save the translation file with the filename `loot.po` in `resources/l10n/<locale>/LC_MESSAGES/`, where `<locale>` is your language's POSIX locale code.

Some helpful Poedit settings include:
* Deselecting `File -> Preferences... -> General -> Editing -> Automatically compile MO file when saving`. This file is built by LOOT at runtime and doesn't need to be included in your PR, but you might accidentally submit it alongside your translation.
* Selecting `File -> Preferences... -> General -> Editing -> Show summary after updating files`. This will show you a brief summary of strings that were changed/obsoleted when you update from the template.

Some languages may use different words or phrases for different contexts where only one word or phrase may be used for all contexts in English. Context for strings can be seen in the bottom-right corner in Poedit (if available) under **Notes for translators**. While contextual information isn't supplied for all strings by default, it can be added on request. To request the addition of contextual information to a text string, create an issue for your request in LOOT's [source code issue tracker](https://github.com/loot/loot/issues), quoting the string for which you are requesting contextual information.

You can also add comments of your own for future translators such as why you translated something a certain way or why you left something untranslated. To do so, right-click a string you want to comment on and select `Edit comment`. They can also be seen in the bottom-right corner in Poedit under **Comment**.

Some strings to be translated may contain special characters. Different types of special character that may be encountered are:

* Backslashes (`\`). These are used to escape backslashes and double-quotation marks (`"`) in the C++ translation strings. Don't add new backslashes into translations, and make sure all backslashes in the original string are retained in the translation.
* Formatting placeholders are used so that LOOT can substitute text or numbers that are generated at runtime into pre-made strings. They appear in a few different formats:
  1. A number surrounded by percentage signs, e.g. `%1%`.
  2. A named placeholder, e.g. `%(icon)s` (the name in parentheses must not be changed).
  3. A simple placeholder `%s`.

  If formatting placeholders are used in the untranslated string, they **must all** be present in the translated string, or LOOT will encounter an error when it tries to display the translated string. Placeholders can be moved around so that the sentence makes grammatical sense in the target language.
* A small number of strings also include HTML `<span>` elements that wrap translatable text. While the text inside the elements should be translated, the element tags and their attributes should not be.
* A small number of strings also include Markdown hyperlinks (e.g. `[example text](http://example.com/)`). Similar to HTML `<span>` elements, the text between the square brackets should be translated, but the text between the parentheses should not be.

### Adding A New Translation

If you're adding a new translation, LOOT's source code must be updated to recognise it. While you're welcome to do this yourself and include the changes in your translation's pull request, you don't have to if you're not comfortable with them; we can add them later. The files and functions which must be updated are given below:

* In [loot_settings.h](src/gui/state/loot_settings.h), add the language's
  ISO code and name to the `languages_` initialiser list.
* In [loot_settings_test.h](src/tests/gui/state/loot_settings_test.h), update
  the `defaultConstructorShouldSetDefaultValues` test so that it includes the
  added language, updating the `actualLanguages.size()` check and adding a new
  `EXPECT_EQ(...)` line for the new language.
* In [installer.iss](scripts/installer.iss):
  - Add a line for your language in the `[Languages]` section. If you have Inno
    Setup installed, the available translation files are at
    `C:\Program Files (x86)\Inno Setup 6\Languages\`.
  - Add an entry for your language's translation file to the `[Files]` section.
  - Add a translation for your language to the `[CustomMessages]` section.

See commit [8c28aed7f54dee3a381425b2d4ecf4341309d139](https://github.com/loot/loot/commit/8c28aed7f54dee3a381425b2d4ecf4341309d139)
for an example of the relevant changes.

## Depending on libloot snapshot builds

It's occasionally useful to build LOOT using a snapshot build of libloot, e.g. when integrating
unreleased libloot changes. To do this, download a snapshot build artifact from GitHub Actions (you
need to be logged into GitHub to do so) and unzip it to get a 7-zip or XZ-compressed tar file, then
pass `-DLIBLOOT_URL=<path to that file>` when running `cmake`.

To download and extract a snapshot build artifact during the GitHub Actions CI workflow, add the
following step before the step that runs `cmake`:

```yaml
- name: Download libloot snapshot build artifact
  run: |
    curl -sSfL \
      -H 'authorization: Bearer ${{ secrets.GITHUB_TOKEN }}' \
      -H 'Accept: application/vnd.github.v3+json' \
      -o 'libloot.zip' \
      https://api.github.com/repos/loot/libloot/actions/artifacts/39467110/zip
    unzip libloot.zip
    echo "LIBLOOT_URL=$PWD/$(ls -1 libloot-*)" >> $GITHUB_ENV
```

Replace `39467110` with the relevant artifact's ID. You can get an artifact's ID from the last
path component in its download URL on the libloot build's GitHub Actions page in your browser: for
example, the page at `https://github.com/loot/libloot/actions/runs/542851787` lists
`libloot-0.16.1-9-ge97208c_linux-github-actions-Linux.tar.xz` as an artifact, and its download URL
is `https://github.com/loot/libloot/suites/1982340583/artifacts/39467110`.

Also append ` -DLIBLOOT_URL="$LIBLOOT_URL"` to the arguments passed to `cmake`.

## Code Style

LOOT's JavaScript code style is codified by the included ESLint configuration, which is based off the Airbnb and Prettier rules.

### C++ Code Style

The [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) is used as the base, with deviations as listed below. Formatting style is codified in the repository's `.clang-format` file, but is not enforced.

#### C++ Features

* Static variables may contain non-POD types.
* Reference arguments don't need to be `const` (i.e. they can be used for output variables).
* Exceptions can be used.
* Unsigned integer types can be used.
* There's no restriction on which Boost libraries can be used.
* Specialising `std::hash` is allowed.

#### Naming

* Constant, enumerator and variable names should use `camelCase` or `underscore_separators`, but they should be consistent within the same scope.
* Function names should use `PascalCase` or `camelCase`, but they should be consistent within the same scope.
