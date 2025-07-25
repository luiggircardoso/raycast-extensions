# Preferences

Use the Preferences API to make your extension configurable.

Preferences are configured in the [manifest](../information/manifest.md#preference-properties) per command or shared in the context of an extension.

Required preferences need to be set by the user before a command opens. They are a great way to make sure that the user of your extension has everything set up properly.

## API Reference

### getPreferenceValues

A function to access the preference values that have been passed to the command.

Each preference name is mapped to its value, and the defined default values are used as fallback values.

#### Signature

```typescript
function getPreferenceValues(): { [preferenceName: string]: any };
```

#### Example

```typescript
import { getPreferenceValues } from "@raycast/api";

interface Preferences {
  name: string;
  bodyWeight?: string;
  bodyHeight?: string;
}

export default async function Command() {
  const preferences = getPreferenceValues<Preferences>();
  console.log(preferences);
}
```

#### Return

An object with the preference names as property key and the typed value as property value.

Depending on the type of the preference, the type of its value will be different.

| Preference type        | Value type                                             |
| :--------------------- | :----------------------------------------------------- |
| <code>textfield</code> | <code>string</code>                                    |
| <code>password</code>  | <code>string</code>                                    |
| <code>checkbox</code>  | <code>boolean</code>                                   |
| <code>dropdown</code>  | <code>string</code>                                    |
| <code>appPicker</code> | <code>[Application](./utilities.md#application)</code> |
| <code>file</code>      | <code>string</code>                                    |
| <code>directory</code> | <code>string</code>                                    |

### openExtensionPreferences

Opens the extension's preferences screen.

#### Signature

```typescript
export declare function openExtensionPreferences(): Promise<void>;
```

#### Example

```typescript
import { ActionPanel, Action, Detail, openExtensionPreferences } from "@raycast/api";

export default function Command() {
  const markdown = "API key incorrect. Please update it in extension preferences and try again.";

  return (
    <Detail
      markdown={markdown}
      actions={
        <ActionPanel>
          <Action title="Open Extension Preferences" onAction={openExtensionPreferences} />
        </ActionPanel>
      }
    />
  );
}
```

#### Return

A Promise that resolves when the extensions preferences screen is opened.

### openCommandPreferences

Opens the command's preferences screen.

#### Signature

```typescript
export declare function openCommandPreferences(): Promise<void>;
```

#### Example

```typescript
import { ActionPanel, Action, Detail, openCommandPreferences } from "@raycast/api";

export default function Command() {
  const markdown = "API key incorrect. Please update it in command preferences and try again.";

  return (
    <Detail
      markdown={markdown}
      actions={
        <ActionPanel>
          <Action title="Open Command Preferences" onAction={openCommandPreferences} />
        </ActionPanel>
      }
    />
  );
}
```

#### Return

A Promise that resolves when the command's preferences screen is opened.

## Types

### Preferences

A command receives the values of its preferences via the [`getPreferenceValues`](#getpreferencevalues) function. It is an object with the preferences' `name` as keys and their values as the property's values.

Depending on the type of the preference, the type of its value will be different.

| Preference type        | Value type                                             |
| :--------------------- | :----------------------------------------------------- |
| <code>textfield</code> | <code>string</code>                                    |
| <code>password</code>  | <code>string</code>                                    |
| <code>checkbox</code>  | <code>boolean</code>                                   |
| <code>dropdown</code>  | <code>string</code>                                    |
| <code>appPicker</code> | <code>[Application](./utilities.md#application)</code> |
| <code>file</code>      | <code>string</code>                                    |
| <code>directory</code> | <code>string</code>                                    |

{% hint style="info" %}
Raycast provides a global TypeScript namespace called `Preferences` which contains the types of the preferences of all the commands of the extension.

For example, if a command named `show-todos` has some preferences, its `getPreferenceValues`'s return type can be specified with `getPreferenceValues<Preferences.ShowTodos>()`. This will make sure that the types used in the command stay in sync with the manifest.
{% endhint %}
