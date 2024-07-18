# IDE Plugin Ideas

I've been collecting various ideas for IDE plugins that I haven't been able to work on, and probably won't be able to work on for a long time.

In order to ensure that those ideas don't go to waste or get lost, I'll collect them in this repository and make them available for anyone who wants to find ideas what new IDE plugins to work on. Some may have already been implemented in some form that I don't know of.

Some of these ideas are simply for fun and I thought that they would be cool to implement or cool to have.

If you are looking for a wider range of ideas, I recommend JetBrains' [Plugin Ideas](https://plugins.jetbrains.com/plugin-ideas) collection, they have a wide variety of plugin requests from community members.

I also marked the ideas, with a badge, that I would currently be comfortable working on under a freelance contract, if someone sees potential in those ideas.

## Acronyms & Glossary

![Open to work on](https://img.shields.io/badge/Open%20to%20work%20on--blue)

On multiple of my previous projects we had dedicated Acronyms & Glossary wiki pages, so if someone was not familiar with a term, they could look it up easily, or extend those shared wikis if they didn't find what they were looking for.

To get this information closer to engineers who do coding, so that they wouldn't have to look up a wiki page, instead they could find out the meaning of an abbreviation or a domain specific term, the lookup could happen right inside the IDE.

Let's call the source files for these information *dictionaries* with the terms as keys (could be case-insensitive), and their descriptions as values.
Such dictionaries could be defined in *properties*, *json*, *yaml*, etc. files, depending on what kind of data you want to store and how you want to separate the descriptions. For example:
- not just a simple description, but e.g. description, application version, related terms, etc.
- example code snippets, or embedded HTML or Markdown formatting for better presentation of term descriptions.

Users could define:
- One or more dictionaries, so that different topics and domain areas could be stored in a nicely separated way.
- Application and project specific dictionaries.
    - Application-level ones may be stored in the plugin itself, or in a remote, shared location that the plugin could access and download dictionaries from. If such remote files are downloaded, the plugin could make periodic checks to see if an updated version of the file(s) is available, and download it automatically or on-demand.
    - Project-level ones may be stored in the corresponding projects under a pre-defined location, or in files with predefined names.

### How to access the dictionary?

I currently see two ways of displaying dictionaries, or descriptions from dictionaries:
- a tool window, or an editor tab component,
- a documentation popup similar to IntelliJ Quick Documentation popups.

#### Documentation popup

This could work as follows:
- user selects some text in an editor, preferably a single term (multiple term selection, e.g. in camelCase text could be handled too),
- user invokes a shortcut key, or a custom context menu action,
- the plugin looks up the term in the available application- and project-level dictionaries.
- plugin displays the description and any additional, relevant information in a custom popup (but not a dialog window).
    - the information in the popup would be limited to a certain number of results
    - option(s) could be added to the popup to display more results in the corresponding tool window/editor tab component

#### Tool window / Editor tab component

The tool window or a standalone editor tab component would display detailed results, and would feature detailed search capabilities for terms.

#### Lookup logic

The following fallback logic could be the basis for looking up terms.
- case-sensitive exact match on the term/key
- case-insensitive exact match on the term/key
- key contains term as case-sensitive
- key contains term as case-insensitive

The actual lookup and fallback logic would depend on the types of files or storage the dictionaries would be stored in.

----

## Cucumber Step Definition Window

I worked a lot with Cucumber and Gherkin features, and it is always a challenge to manage at least three layers of test code: Gherkin feature files, a thin layer of glue code, and the actual test logic the glue code delegates to.

Regardless of how thin the glue code layer is, what if one could see/observe the Gherkin features and the steps' corresponding glue code/step definition implementations, from the same editor a Gherkin feature file is opened in, so that you could edit the Gherkin steps and their implementations in the same editor, in one place?

Although the usefulness of this feature is questionable, it is still fun to brainstorm how this could be implemented, and how this could be made useful at least to some extent.

### Behaviour

It would work as follows:
- Under each step in a Gherkin file, one or more expandable/collapsible links/texts would appear referencing the corresponding step definition method (or lambda expression) implementations, depending on the number of implementations the step has.
- These links would look similar or the same as Inlay Hints / Code Vision above code elements, but these would be display **under** each step.
- Upon clicking on such a link, it would expand that section and would show a small embedded editor, right under the step/link, showing the implementation of the corresponding step definition method.
- In the embedded editor, one could directly edit the step definition method's code, or use the regular editor features to navigate, etc.

Hooks:
- Implementations of *before* and *after* hooks could be displayed somewhere at the beginning of Gherkin features to have them available at a common location.

### Visuals

It would look something like:

```gherkin
Feature: Some gherkin feature

  Scenario: Some scenario
    Given some precondition
    > collapsed link to step definition
    When an action happens
    > expanded link to step definition
      ------------------------------------
      |  @Given("...")                   |
      |  public void anActionHappens() { |
      |    actions.performAction()       |
      |  }                               |
      |                                  |
      ------------------------------------
    Then validation should happen
```

### Implementation notes

- This could use the editor view that is displayed at the bottom of the editor when using e.g. the Extract Method action that shows the contents of the method being extracted, and it is out of the current view.

Possible advantages and disadvantages I currently see:
- A: It could potentially simplify test implementation or maintenance because users would need less navigation between classes.
    - This might be better suited for cases when the glue code is not a thin layer, because via element references and shortcuts like Ctrl+Click, users can navigate to step definitions quickly.
- D: It would potentially make the readability of Gherkin file worse but hopefully just slightly.
    - This would depend on the visual representation of the step definition links.

Related links: [Req'n'roll VS Code extension 2024.1 release](https://reqnroll.net/news/2024/02/reqnroll-visual-studio-extension-v2024-1-released/)

----

## Test Results Vocalizer

I had this idea for a while to assign audio feedback to test executions based on the results they produce. And not just a simple beep, ring, or some random noise, but human speech. It came to my mind when one day I was running my integration tests for a plugin and the IDE popped up the test result notification balloon with the results.

Now, the idea came not from the productivity perspective, rather from the cheeky side. Although it may be useful to have a human voice tell you about your test results, I wanted to add some flavor to it by emphasizing the positive or negative nature of test results by emotions in those reactions.

I mean like encouraging, cheerful on the positive side, or for instance condescending (in a cheeky way) on the negative side, with the added flavor of language specific sayings and slang, like Blimey! in the UK.

The idea is that it would take into account not just whether the test result passed or failed, but the ratio of passed/failed tests, how many-th the same test execution was, and if it shows any improving or worsening trend between the runs.

While coming up with sentences to play to the users, the challenge is to properly assign them to the right test results based on the passed/failed nature, ratio, etc.

### Technical side

Challenges/obstacles to tackle:
- One has to work with properly compressed audio files, and see what formats are optimal and suitable for inclusion in such a plugin.
- One has to find out if there are proper listeners in the IntelliJ Platform code base that could be bound to JUnit test execution results and statues.
- One has to find out if there are JUnit test execution result statistics stored, or if not, how to store them, so that the appropriate audio files can be played.

Starting points:
- The [FridayMario](https://github.com/dkandalov/friday-mario) plugin that plays various Super Mario sounds bound to different IDE actions. (not endorsing the plugin, or is not a sponsored segment, simply mentioning it)
- The [Extension Point and Listener List](https://plugins.jetbrains.com/docs/intellij/extension-point-list.html) in the IntelliJ Platform Plugin SDK docs to find a suitable listener.
    - The first find is the `com.intellij.junitlistener` EP, but from a quick glance it doesn't return any test result stats. However, I'm not throwing this EP away just yet, it might be useful for something.

So, it would play different audio files depending on the passed/failed nature of the tests, the ratio and the trend of test executions.

Examples for passed tests:
- You're the man!
- You're awesome!
- You're breathtaking!

Examples for failed tests:
- You have failed. Now go, make me proud.
- You can do better than that.

Passed with failed tests before:
- Look at you, you can fix things!

It could support different languages and emotions.

----

## ASCII (art) Menu System in an Editor

![Open to work on](https://img.shields.io/badge/Open%20to%20work%20on--blue)

I had this idea when I was playing with Hollow Knight and moving around in its menus.

So, the basic idea is to display custom menus inside an editor only using ASCII characters. That would include the text and styling of the menu buttons.

Users could create menu with custom actions assigned, (or could even re-create portions of IntelliJ's if they wanted to). This might be useful in full-screen or distraction free modes where there is no GUI menu displayed, but users want some kind of (fancy!) access to menus without invoking the actions by their shortcut keys, or using the Find Everything popup. Anything can be useful for someone. :)

Related ASCII game engine: [CosPlay: 2D ASCII Game Engine for Scala3](https://github.com/nivanov/cosplay)

The configuration file of a menu system could look something like this (it doesn't have to be XML though):

```xml
<menus>
    <menu id="Main.Menu" type="main" textStyler="fqn.MenuTextStyler">
        <menuItem text="Git Clone..." action="ij.git.clone" />
        <menuItem text="GitHub" opens="GitHub.Menu" />
    </menu>
    <menu id="GitHub.Menu">
        <menuItem text="Create Pull Request" action="ij.github..create.pull.request" />
        <menuItem text="Sync Fork" action="ij.github.sync.fork"/>
    </menu>
</menus>
```

### Visuals

Menus could look something like this, and selection could be changed via the Up and Down arrows. Selected item could be invoked via hitting Enter.

```
[ Git Clone... ]
  GitHub
```

```
> Git Clone...
  GitHub
```

### Configuration

Menu configuration options may be:
- alignment: horizontal, vertical, left, right, center
- type: main menu, submenu
- text size and style/font:
    - this could be simple text size only for the given editor, or
    - a custom ASCII-art with the given size, style and font
- menu item frame
- selected menu item indicator
