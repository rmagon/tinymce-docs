---
layout: default
title: Context forms
title_nav: Context forms
description: Context forms overview
keywords: contextforms context forms contextformsbarapi
---

A context form consists of an input field, and a series of related buttons. Context forms can be shown wherever a context toolbar can be shown. Also, when a context form is registered containing a `launch` configuration, a special context toolbar button with name `form:${name}` is registered which will launch that particular context form.
Context forms are a generalisation of the `Insert Link` form that existed in the original [inlite]({{site.url}}/docs-4x/themes/inlite/#quicklink) theme from TinyMCE 4.

### Registering a context form

A context form is registered by calling the `addContextForm` API in the registry. The specification of a context form is separated into two parts:

### Launch

The button strings for launching a context form is `form:${name}` where name is the registered name of the context form (e.g. form:link).

> Note: These buttons will only be present if the `launch` setting of the context form is specified.

The Launch specification. This relates to what the button that launches this form will look like.

| Name | Details |
| ---- | ------- |
| `launch` | This is the specification for the launching button that can appear in a context toolbar only. It will be either type: `contextformbutton` or `contextformtogglebutton`, and will be identical to those definitions below except it will **not** have an `onAction`. |

#### Launching context forms from a context toolbar

If a registered context form has a `launch` setting, then it can be launched from a context toolbar. The name of item will be `form:${name}` (e.g. 'form:link'). When the user presses this button, the toolbar will change into the specified context form. If you the user presses `Esc` in a context form that was launched through a context toolbar, they will be returned to the original context toolbar.

#### Launching a context form programmatically

There is an `editor` event called `contexttoolbar-show` that can be fired to show a context form at the current selection. The event takes a parameter `toolbarKey` which specifies the name of the registered context form or context toolbar to show.

### Context form priority

{% include context/priority.md %}

### Positioning context forms

There are two options for positioning: `selection` or `line`.

* A `selection` position will place the context form above or below the current selection, centred if possible.

* A `line` position will place the context form to the right (or left in Right-to-Left languages) of the current selection.

* A `node` position will place the context form relative to the bounds of a node (e.g. a table or image). It applies to a selected node that does not match the selection due to CSS properties (like float).

### Form

This relates to the form itself. The form specifications are:

| Name | Details |
| ---- | ------- |
| `launch` | This is the specification for the launching button that can appear in a context toolbar only. It will be either type: `contextformbutton` or `contextformtogglebutton`, and will be identical to those definitions below except it will **not** have an `onAction`. |
| `label` | This is the label that will appear in the form. |
| `initValue` | This is the initial value the input will have in the form. |
| `predicate` | This controls when the context toolbar will appear. |
| `position` | This controls where the context toolbar will appear with regards to the current cursor. |
| `scope` | This controls whether the predicate is a `node`-based predicate, or an `editor`-based predicate. See context toolbar proirity for more details. |
| `commands` | This is a list of the items to show in the context form. They can be either `contextformbutton` or `contextformtogglebutton`. |

### Context form buttons

Unlike normal context toolbar buttons, Context form buttons are not registered beforehand. Instead, you need to define each button completely in the `commands` section.

#### contextformbutton

The definition of a context form button is very similar to the definition of a normal toolbar button. The main difference is that the action is slightly different (as it will relate to the form), and the type is `contextformbutton` instead of `button`. The following is the full list of options:

##### type: 'contextformtogglebutton';

| Name | Value | Requirement | Description |
| ---- | ----- | ----------- | ----------- |
| primary | boolean; | Optional | This will activate the button on <enter> in the input form. |
| onAction | (formApi, togglebuttonApi) => void; | Required | This decides what happens when the user clicks the button. |
| active | boolean; | Optional | default:false |
| disabled | boolean; | Optional | default: false - Represents button state. Is toggled by the button's API |
| tooltip | string; | Optional | Text for button tooltip. |
| text | string; | Optional | Text to display if no icon is found. |
| icon | string; | Optional | It displays the icon corresponding to the icon name that has been defined in the icon pack. |
| onSetup | (togglebuttonApi) => (togglebuttonApi) => void; | Optional | default: () => () => {} - Function that's invoked when the button is rendered. |

Where the `buttonApi` is the same as a regular toolbar button and `FormApi` has (getValue: () => string, hide: () => void)

#### Context form toggle button

The context form toggle button is very similar to the a normal toolbar toggle button. The main difference is that the action is slightly different (as it will relate to the form), and the type is `contextformtogglebutton`, instead of `togglebutton`. The following is the full list of options:

##### type: 'contextformtoggleutton';

| Name | Value | Requirement | Description |
| ---- | ----- | ----------- | ----------- |
| primary | boolean; | Optional | This will activate the button on <enter> in the input form. |
| onAction | (formApi, togglebuttonApi) => void; | Required | This decides what happens when the user clicks the button. |
| active | boolean; | Optional | default:false |
| disabled | boolean; | Optional | default: false - Represents button state. Is toggled by the button's API |
| tooltip | string; | Optional | Text for button tooltip. |
| text | string; | Optional | Text to display if no icon is found. |
| icon | string; | Optional | It displays the icon corresponding to the icon name that has been defined in the icon pack. |
| onSetup | (togglebuttonApi) => (togglebuttonApi) => void; | Optional | default: () => () => {} - Function that's invoked when the button is rendered. |

Where the `toggleButtonApi` is the same as a regular toolbar toggle button.

#### formApi

Both `contextformbutton` and `contextformtogglebutton` are passed `formApi` in their `onAction` callback. The `formApi` has two functions:

| Functions | Description |
| --------- | ----------- |
|`hide` | This will hide the form. By default, no button hides the form. It is the responsibility of the developer to hide the form in the `onAction` handler of buttons that require that the context form close after the action. |
| `getValue` | This will retrieve the value current typed in the input field. |

### Example configuration

This example shows how the link plugin adds the standard link context form. The context form will show whenever any content is selected.

{% include codepen.html id="context-form" height="400" tab="js" %}
