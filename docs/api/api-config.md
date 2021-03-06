<!-- ***********************************************************************************************
	Config API
************************************************************************************************ -->
<h1 id="config-api"><code>Config</code> API</h1>

The `Config` object controls various aspects of SugarCube's behavior.

<p role="note"><b>Note:</b>
<code>Config</code> object settings should be placed within a script section (Twine&nbsp;2: the Story JavaScript; Twine&nbsp;1/Twee: a <code>script</code>-tagged passage).
</p>


<!-- ***************************************************************************
	Audio
**************************************************************************** -->
<span id="config-api-audio"></span>
## Audio Settings

<!-- *********************************************************************** -->

<span id="config-api-property-audio-pauseonfadetozero"></span>
### `Config.audio.pauseOnFadeToZero` ↔ *boolean* (default: `true`)

Determines whether the audio subsystem automatically pauses tracks that have been faded to `0` volume (silent).

#### Since:

* `v2.28.0`

#### Example:

```
Config.audio.pauseOnFadeToZero = false;
```

<!-- *********************************************************************** -->

<span id="config-api-property-audio-preloadmetadata"></span>
### `Config.audio.preloadMetadata` ↔ *boolean* (default: `true`)

Determines whether the audio subsystem attempts to preload track metadata—meaning information about the track (e.g., duration), not its audio frames.

<p role="note"><b>Note:</b>
It is unlikely that you will ever want to disable this setting.
</p>

#### Since:

* `v2.28.0`

#### Example:

```
Config.audio.preloadMetadata = false;
```


<!-- ***************************************************************************
	History
**************************************************************************** -->
<span id="config-api-history"></span>
## History Settings

<!-- *********************************************************************** -->

<span id="config-api-property-history-controls"></span>
### `Config.history.controls` ↔ *boolean* (default: `true`)

Determines whether the story's history controls (Backward, Jump To, & Forward buttons) are enabled within the UI bar.

#### Since:

* `v2.0.0`

#### Example:

```
Config.history.controls = false;
```

<!-- *********************************************************************** -->

<span id="config-api-property-history-maxstates"></span>
### `Config.history.maxStates` ↔ *integer* (default: `100`)

Sets the maximum number of states (moments) to which the history is allowed to grow.  Should the history exceed the limit, states will be dropped from the past (oldest first).  A setting of `0` means that there is no limit to how large the history may grow, though doing so is not recommended.

#### Since:

* `v2.0.0`

#### Example:

```
// No history limit (you should never do this!)
Config.history.maxStates = 0;

// Limit the history to a single state
Config.history.maxStates = 1;

// Limit the history to 150 states
Config.history.maxStates = 150;
```


<!-- ***************************************************************************
	Macros
**************************************************************************** -->
<span id="config-api-macros"></span>
## Macros Settings ##

<!-- *********************************************************************** -->

<span id="config-api-property-macros-ifassignmenterror"></span>
### `Config.macros.ifAssignmentError` ↔ *boolean* (default: `true`)

Determines whether the [`<<if>>` macro](#macros-macro-if) returns an error when the `=` assignment operator is used within its conditional—e.g., `<<if $suspect = "Bob">>`.  Does not flag other assignment operators.

<p role="note"><b>Note:</b>
This setting exists because it's unlikely that you'll ever want to actually perform an assignment within a conditional expression and typing <code>=</code> when you meant <code>===</code> (or <code>==</code>) is a fairly easy to mistake make—either from a finger slip or because you just don't know the difference between the operators.
</p>

#### Since:

* `v2.0.0`

#### Example:

```
// No error is returned when = is used within an <<if>> or <<elseif>> conditional
Config.macros.ifAssignmentError = false;
```

<!-- *********************************************************************** -->

<span id="config-api-property-macros-maxloopiterations"></span>
### `Config.macros.maxLoopIterations` ↔ *integer* (default: `1000`)

Sets the maximum number of iterations allowed before the [`<<for>>` macro](#macros-macro-for) conditional forms are terminated with an error.

<p role="note"><b>Note:</b>
This setting exists to prevent a misconfigured loop from making the browser unresponsive.
</p>

#### Since:

* `v2.0.0`

#### Example:

```
// Allow only 5000 iterations
Config.macros.maxLoopIterations = 5000;
```


<!-- ***************************************************************************
	Navigation
**************************************************************************** -->
<span id="config-api-navigation"></span>
## Navigation Settings ##

<!-- *********************************************************************** -->

<span id="config-api-property-navigation-override"></span>
### `Config.navigation.override` ↔ *function* (default: *none*)

Allows the destination of passage navigation to be overridden.  The callback is passed one parameter, the original destination passage title.  If its return value is falsy, the override is cancelled and navigation to the original destination continues unperturbed.  If its return value is truthy, the override succeeds and that value is used as the new destination of the navigation.

#### Since:

* `v2.13.0`

#### Example:

```
Config.navigation.override = function (destinationPassage) {
	/* code */
};
```

##### Based upon a story variable

```
// Force the player to the "You Died" passage if they let $health get too low.
Config.navigation.override = function (dest) {
	var sv = State.variables;

	// If $health is less-than-or-equal to 0, go to the "You Died" passage instead.
	if (sv.health <= 0) {
		return "You Died";
	}
};
```


<!-- ***************************************************************************
	Passages
**************************************************************************** -->
<span id="config-api-passages"></span>
## Passages Settings ##

<!-- *********************************************************************** -->

<span id="config-api-property-passages-descriptions"></span>
### `Config.passages.descriptions` ↔ *boolean* | *object* | *function* (default: *none*)

Determines whether alternate passage descriptions are used by the Saves and Rewind menus (by default an excerpt from the passage is used).  Valid values are boolean `true`, which simply causes the passages' titles to be used, an object, which maps passages' titles to their descriptions, or a function, which should return the passages' description.

<div role="note"><b>Note:</b>
<ul class="asnote">
<li><b>As boolean:</b> You should ensure that all encounterable passage titles are meaningful.</li>
<li><b>As object:</b> If the mapping object does not contain an entry for the passage in question, then the passage's excerpt is used instead.</li>
<li><b>As function:</b> The function is called with the passage in question as its <code>this</code> value.  If the function returns falsy, then the passage's excerpt is used instead.</li>
</ul>
</div>

#### Since:

* `v2.0.0`

#### Example:

```
// Uses passages' titles
Config.passages.descriptions = true;

// Uses the description mapped by the title
Config.passages.descriptions = {
	"title" : "description"
};

// Returns the description to be used
Config.passages.descriptions = function () {
	if (this.title === "title") {
		return "description";
	}
};
```

<!-- *********************************************************************** -->

<span id="config-api-property-passages-displaytitles"></span>
### `Config.passages.displayTitles` ↔ *boolean* (default: `false`)

Determines whether passage titles are combined with the story title, within the browser's/tab's titlebar, when passages are displayed.

#### Since:

* `v2.0.0`

#### Example:

```
Config.passages.displayTitles = true;
```

<!-- *********************************************************************** -->

<span id="config-api-property-passages-nobr"></span>
### `Config.passages.nobr` ↔ *boolean* (default: `false`)

Determines whether rendering passages have their leading/trailing newlines removed and all remaining sequences of newlines replaced with single spaces before they're rendered.  Equivalent to including the [`nobr` special tag](#special-tag-nobr) on every passage.

<p role="note"><b>Note:</b>
Does not affect <code>script</code> or <code>stylesheet</code> tagged passages, for Twine&nbsp;1/Twee, or the Story JavaScript or Story Stylesheet sections, for Twine&nbsp;2.
</p>

#### Since:

* `v2.19.0`

#### Example:

```
Config.passages.nobr = true;
```

<!-- *********************************************************************** -->

<span id="config-api-property-passages-onprocess"></span>
### `Config.passages.onProcess` ↔ *function* (default: *none*)

Allows custom processing of passage text.  The function is invoked each time the [`<Passage>.processText()` method](#passage-api-prototype-method-processtext) is called.  It is passed an abbreviated version of the associated passage's [`Passage` instance](#passage-api)—containing only the `tags`, `text`, and `title` properties.  Its return value should be the post-processed text.

<p role="note"><b>Note:</b>
The function will be called just before the built-in no-break passage processing if you're also using that—see the <a href="#config-api-property-passages-nobr"><code>Config.passages.nobr</code> setting</a> and <a href="#special-tag-nobr"><code>nobr</code> special tag</a>.
</p>

#### Since:

* `v2.30.0`

#### Example:

```
/* Change instancess of "cat" to "dog". */
Config.passages.onProcess = function (p) {
	return p.text.replace(/\bcat(s?)\b/g, 'dog$1');
};
```

<!-- *********************************************************************** -->

<span id="config-api-property-passages-start"></span>
### `Config.passages.start` ↔ *string* (Twine&nbsp;2 default: *user-selected*; Twine&nbsp;1/Twee default: `"Start"`)

Sets the starting passage, the very first passage that will be displayed.

#### Since:

* `v2.0.0`

#### Example:

```
Config.passages.start = "That Other Starting Passage";
```

<!-- *********************************************************************** -->

<span id="config-api-property-passages-transitionout"></span>
### `Config.passages.transitionOut` ↔ *string* | *integer* (default: *none*)

Determines whether outgoing passage transitions are enabled.  Valid values are the name of the property being animated, which causes the outgoing passage element to be removed once that transition animation is complete, or an integer delay (in milliseconds), which causes the outgoing passage element to be removed once the delay has expired.  You will also need some CSS styles to make this work—examples given below.

<p role="note"><b>Note:</b>
If using an integer delay, ideally, it should probably be slightly longer than the outgoing transition delay that you intend to use—e.g., an additional 10ms or so should be sufficient.
</p>

#### Since:

* `v2.0.0`

#### Example:

```
// Remove outgoing elements when their opacity animation ends
Config.passages.transitionOut = "opacity";

// Remove outgoing elements after 1010ms (1.01s)
Config.passages.transitionOut = 1010;
```

#### CSS styles:

At the very least you will need to specify a `.passage-out` style that defines the transition's end state.  For example:

```
.passage-out {
	opacity: 0;
}
```

That probably won't be very pleasing to the eye, however, so you will likely need several styles to make something that looks half-decent.  For example, the following will give you a basic crossfade:

```
#story {
	position: relative;
}
.passage {
	left: 0;
	position: absolute;
	top: 0;
	transition: opacity 1s ease;
}
.passage-out {
	opacity: 0;
}
```


<!-- ***************************************************************************
	Saves
**************************************************************************** -->
<span id="config-api-saves"></span>
## Saves Settings ##

<!-- *********************************************************************** -->

<span id="config-api-property-saves-autoload"></span>
### `Config.saves.autoload` ↔ *boolean* | *string* | *function* (default: *none*)

Determines whether the autosave, if it exists, is automatically loaded upon story startup.  Valid values are boolean `true`, which simply causes the autosave to be loaded, the string `"prompt"`, which prompts the player via a dialog to load the autosave, or a function, which causes the autosave to be loaded if its return value is truthy.

**NOTE:** If the autosave cannot be loaded, for any reason, then the start passage is loaded instead.

#### Since:

* `v2.0.0`

#### Example:

```
// Automatically loads the autosave
Config.saves.autoload = true;

// Prompts the player about loading the autosave
Config.saves.autoload = "prompt";

// Loads the autosave if it returns a truthy value
Config.saves.autoload = function () {
	/* code */
};
```

<!-- *********************************************************************** -->

<span id="config-api-property-saves-autosave"></span>
### `Config.saves.autosave` ↔ *boolean* | *string array* | *function* (default: *none*)

Determines whether the autosave is created/updated when passages are displayed.  Valid values are boolean `true`, which causes the autosave to be updated with every passage, an array of strings, which causes the autosave to be updated for each passage with at least one matching tag, or a function, which causes the autosave to be updated for each passage where its return value is truthy.

<p role="note" class="warning"><b>Warning:</b>
When setting the value to boolean <code>true</code>, you will likely also need to use the <a href="#config-api-property-saves-isallowed"><code>Config.saves.isAllowed</code> property</a> to disallow saving on the start passage.  Or, if you use the start passage as real part of your story and allow the player to reenter it, rather than just as the initial landing/cover page, then you may wish to only disallow saving on the start passage the very first time it's displayed—i.e., at story startup.
</p>

#### Since:

* `v2.0.0`: Basic syntax.
* `v2.30.0`: Added function values and deprecated string values.

#### Example:

```
// Autosaves every passage
Config.saves.autosave = true;

// Autosaves on passages tagged with any of "bookmark" or "autosave"
Config.saves.autosave = ["bookmark", "autosave"];

// Autosaves on passages if it returns a truthy value
Config.saves.autosave = function () {
	/* code */
};
```

<!-- *********************************************************************** -->

<span id="config-api-property-saves-id"></span>
### `Config.saves.id` ↔ *string* (default: *slugified story title*)

Sets the story ID associated with saves.

#### Since:

* `v2.0.0`

#### Example:

```
Config.saves.id = "a-big-huge-story-part-1";
```

<!-- *********************************************************************** -->

<span id="config-api-property-saves-isallowed"></span>
### `Config.saves.isAllowed` ↔ *function* (default: *none*)

Determines whether saving is allowed within the current context.  The callback is invoked each time a save is requested.  If its return value is falsy, the save is disallowed.  If its return value is truthy, the save is allowed to continue unperturbed.

#### Since:

* `v2.0.0`

#### Example:

```
Config.saves.isAllowed = function () {
	/* code */
};
```

<!-- *********************************************************************** -->

<span id="config-api-property-saves-onload"></span>
### `Config.saves.onLoad` ↔ *function* (default: *none*)

Performs any required pre-processing before the save data is loaded—e.g., upgrading out-of-date save data.  The callback is passed one parameter, the save object to be processed.  If it encounters an unrecoverable problem during its processing, it may throw an exception containing an error message; the message will be displayed to the player and loading of the save will be terminated.

<p role="note"><b>Note:</b>
See the <a href="#save-api-save-objects">save objects</a> section of the <a href="#save-api">Save API</a> for information on the format of a save.
</p>

#### Since:

* `v2.0.0`

#### Example:

```
Config.saves.onLoad = function (save) {
	/* code */
};
```

<!-- *********************************************************************** -->

<span id="config-api-property-saves-onsave"></span>
### `Config.saves.onSave` ↔ *function* (default: *none*)

Performs any required post-processing before the save data is saved.  The callback is passed one parameter, the save object to be processed.

<p role="note"><b>Note:</b>
See the <a href="#save-api-save-objects">save objects</a> section of the <a href="#save-api">Save API</a> for information on the format of a save.
</p>

#### Since:

* `v2.0.0`

#### Example:

```
Config.saves.onSave = function (save) {
	/* code */
};
```

<!-- *********************************************************************** -->

<span id="config-api-property-saves-slots"></span>
### `Config.saves.slots` *integer* (default: `8`)

Sets the maximum number of available save slots.

#### Since:

* `v2.0.0`

#### Example:

```
Config.saves.slots = 4;
```

<!-- *********************************************************************** -->

<span id="config-api-property-saves-version"></span>
### `Config.saves.version` ↔ *any* (default: *none*)

Sets the `version` property of saves.

<p role="note"><b>Note:</b>
This setting is only used to set the <code>version</code> property of saves.  Thus, it is only truly useful if you plan to upgrade out-of-date saves via a <a href="#config-api-property-saves-onload"><code>Config.saves.onLoad</code></a> callback.
</p>

#### Since:

* `v2.0.0`

#### Example:

```
// As an integer (recommended)
Config.saves.version = 3;

// As a string (not recommended)
Config.saves.version = "v3";
```


<!-- ***************************************************************************
	UI
**************************************************************************** -->
<span id="config-api-ui"></span>
## UI Settings

<!-- *********************************************************************** -->

<span id="config-api-property-ui-stowbarinitially"></span>
### `Config.ui.stowBarInitially` ↔ *boolean* | *integer* (default: `800`)

Determines whether the UI bar (sidebar) starts in the stowed (shut) state initially.  Valid values are boolean `true`/`false`, which causes the UI bar to always/never start in the stowed state, or an integer, which causes the UI bar to start in the stowed state if the viewport width is less-than-or-equal-to the specified number of pixels.

#### Since:

* `v2.11.0`

#### Example:

```
// As a boolean; always start stowed
Config.ui.stowBarInitially = true;

// As a boolean; never start stowed
Config.ui.stowBarInitially = false;

// As an integer; start stowed if the viewport is 800px or less
Config.ui.stowBarInitially = 800;
```

<!-- *********************************************************************** -->

<span id="config-api-property-ui-updatestoryelements"></span>
### `Config.ui.updateStoryElements` ↔ *boolean* (default: `true`)

Determines whether certain elements within the UI bar are updated when passages are displayed.  The affected elements are the story: banner, subtitle, author, caption, and menu.

**NOTE:** SugarCube uses the story's title as the basis for the key used to store and load data used when playing the story and for saves.  Because of this, the story title is not included in updates and it is ***strongly*** recommended that you do not add any kind of dynamic code to it.

#### Since:

* `v2.0.0`

#### Example:

```
// If you don't need those elements to update
Config.ui.updateStoryElements = false;
```


<!-- ***************************************************************************
	Miscellaneous
**************************************************************************** -->
<span id="config-api-miscellaneous"></span>
## Miscellaneous Settings

<!-- *********************************************************************** -->

<span id="config-api-property-addvisitedlinkclass"></span>
### `Config.addVisitedLinkClass` ↔ *boolean* (default: `false`)

Determines whether the `link-visited` class is added to internal passage links that go to previously visited passages—i.e. the passage already exists within the story history.

**NOTE:** You *must* provide your own styling for the `link-visited` class as none is provided by default.

#### Since:

* `v2.0.0`

#### Example:

```
Config.addVisitedLinkClass = true;
```

An example style: (Twine&nbsp;2: goes in Story Stylesheet; Twine&nbsp;1/Twee: goes in a `stylesheet`-tagged passage)

```
.link-visited {
	color: purple;
}
```

<!-- *********************************************************************** -->

<span id="config-api-property-cleanupwikifieroutput"></span>
### `Config.cleanupWikifierOutput` ↔ *boolean* (default: `false`)

Determines whether the output of the Wikifier is post-processed into more sane markup—i.e. where appropriate, it tries to transition the plethora of `<br>` elements into `<p>` elements.

#### Since:

* `v2.0.0`

#### Example:

```
Config.cleanupWikifierOutput = true;
```

<!-- *********************************************************************** -->

<span id="config-api-property-debug"></span>
### `Config.debug` ↔ *boolean* (default: `false`)

Indicates whether SugarCube is running in test mode, which enables debug views.  See [`Test Mode`](#guide-test-mode) for more information.

**NOTE:** This property is automatically set based on whether you're using a testing mode in a Twine compiler—i.e. *Test* mode in Twine&nbsp;2, *Test Play From Here* in Twine&nbsp;1, or the test mode options (`-t, --test`) in Tweego.  You may, however, forcibly enable it if you need to for some reason—e.g., if you're using another compiler, which doesn't offer a way to enable test mode.

#### Since:

* `v2.2.0`

#### Example:

```
// Forcibly enable test mode
Config.debug = true;

// Check if test mode is enabled in JavaScript
if (Config.debug) {
	/* do something debug related */
}

// Check if test mode is enabled via the <<if>> macro
<<if Config.debug>>
	/* do something debug related */
<</if>>
```

<!-- *********************************************************************** -->

<span id="config-api-property-loaddelay"></span>
### `Config.loadDelay` ↔ *integer* (default: `0`)

Sets the integer delay (in milliseconds) before the loading screen is dismissed, once the document has signaled its readiness.  Not generally necessary, however, some browsers render slower than others and may need a little extra time to get a media-heavy page done.  This allows you to fine tune for those cases.

#### Since:

* `v2.0.0`

#### Example:

```
// Delay the dismissal of the loading screen by 2000ms (2s)
Config.loadDelay = 2000;
```
