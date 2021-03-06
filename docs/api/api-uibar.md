<!-- ***********************************************************************************************
	UIBar API
************************************************************************************************ -->
<h1 id="uibar-api"><code>UIBar</code> API</h1>

<!-- *********************************************************************** -->

<span id="uibar-api-method-destroy"></span>
### `UIBar.destroy()`

Completely removes the UI bar and all of its associated styles and event handlers.

#### Since:

* `v2.17.0`

#### Parameters: *none*

#### Example:

```
UIBar.destroy();
```

<!-- *********************************************************************** -->

<span id="uibar-api-method-hide"></span>
### `UIBar.hide()` → *`UIBar` object*

Hides the UI bar.  Returns a reference to the `UIBar` object for chaining.

**NOTE:** This does not reclaim the space reserved for the UI bar.  Thus, a call to [`UIBar.stow()`](#uibar-api-method-stow) may also be necessary.  Alternatively, if you simply want the UI bar gone completely and permanently, either using [`UIBar.destroy()`](#uibar-api-method-destroy) or the [`StoryInterface` special passage](#special-passage-storyinterface) may be a better choice.

#### Since:

* `v2.29.0`

#### Parameters: *none*

#### Example:

##### Basic usage

```
UIBar.hide();
```

##### With stow

```
UIBar.hide().stow();
```

<!-- *********************************************************************** -->

<span id="uibar-api-method-ishidden"></span>
### `UIBar.isHidden()` → *boolean*

Returns whether the UI bar is currently hidden.

#### Since:

* `v2.29.0`

#### Parameters: *none*

#### Example:

```
if (UIBar.isHidden()) {
	/* code to execute if the UI bar is hidden… */
}

if (!UIBar.isHidden()) {
	/* code to execute if the UI bar is not hidden… */
}
```

<!-- *********************************************************************** -->

<span id="uibar-api-method-isstowed"></span>
### `UIBar.isStowed()` → *boolean*

Returns whether the UI bar is currently stowed.

#### Since:

* `v2.29.0`

#### Parameters: *none*

#### Example:

```
if (UIBar.isStowed()) {
	/* code to execute if the UI bar is stowed… */
}

if (!UIBar.isStowed()) {
	/* code to execute if the UI bar is not stowed… */
}
```

<!-- *********************************************************************** -->

<span id="uibar-api-method-show"></span>
### `UIBar.show()` → *`UIBar` object*

Shows the UI bar.  Returns a reference to the `UIBar` object for chaining.

#### Since:

* `v2.29.0`

#### Parameters: *none*

#### Example:

##### Basic usage

```
UIBar.show();
```

##### With unstow

```
UIBar.unstow().show();
```

<!-- *********************************************************************** -->

<span id="uibar-api-method-stow"></span>
### `UIBar.stow([noAnimation])` → *`UIBar` object*

Stows the UI bar, so that it takes up less space.  Returns a reference to the `UIBar` object for chaining.

#### Since:

* `v2.17.0`: Basic syntax.
* `v2.29.0`: Added returned `UIBar` chaining reference.

#### Parameters:

* **`noAnimation`:** (optional, *boolean*) Whether to skip the default animation.

#### Example:

##### Basic usage

```
UIBar.stow();
```

##### With no animation

```
UIBar.stow(true);
```

<!-- *********************************************************************** -->

<span id="uibar-api-method-unstow"></span>
### `UIBar.unstow([noAnimation])` → *`UIBar` object*

Unstows the UI bar, so that it is fully accessible again.  Returns a reference to the `UIBar` object for chaining.

#### Since:

* `v2.17.0`: Basic syntax.
* `v2.29.0`: Added returned `UIBar` chaining reference.

#### Parameters:

* **`noAnimation`:** (optional, *boolean*) Whether to skip the default animation.

#### Example:

##### Basic usage

```
UIBar.unstow();
```

##### With no animation

```
UIBar.unstow(true);
```
