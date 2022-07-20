---
title: Microsoft Power Platform CLI canvas command| Microsoft Docs
description: "Includes descriptions and supported parameters for the canvas command."
keywords: Microsoft Power Platform CLI, code components, component framework, CLI
author: kkanakas
ms.author: kartikka
manager: pemikkel
ms.date: 06/07/2022
ms.reviewer: jdaly
ms.topic: reference
contributors:
  - JimDaly
  - jt000
---

# Canvas (Preview)

Commands for working with canvas app source files. Edit, manage, and collaborate on your app outside of Power Apps Studio with tools such as VS Code and GitHub.

[!INCLUDE [cc-beta-prerelease-disclaimer](../../../includes/cc-beta-prerelease-disclaimer.md)]

> [!IMPORTANT]
>
> - The Canvas commands are in public preview.
> - [!INCLUDE [cc-preview-features-definition](../../../includes/cc-preview-features-definition.md)]

## Parameters

| Property name | Description                                                                                                                                                                                                                                                                                                | Example                                                                                                                                                                         |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| pack          | Creates an `.msapp` file from the previously unpacked source files. <br/>The result can be opened in Power Apps Studio by navigating to **File** > **Open** > **Browse**.<br/> After being unpacked, the source files can be edited and managed with external tools such as Visual Studio Code and GitHub. | `pac canvas pack --sources MyHelloWorldFiles --msapp HelloWorld.msapp`                                                                                                          |
| unpack        | Unpacks the `.msapp` source file.<br/> Download the `.msapp` file from Power Apps Studio by navigating to **File** > **Save as** > **This computer**.<br/> If the **sources** parameter is not specified, a directory with the same name and location as the `.msapp` file is used with `_src` suffix.     | `pac canvas unpack --msapp HelloWorld.msapp --sources MyHelloWorldFiles`<br/>`pac canvas unpack --msapp HelloWorld.msapp`<br/>_unpacks to default_ `HelloWorld_src` _directory_ |

### Folder structure

Unpack and pack properties use the following folder structure:

- **\src** - Control and component files. This contains the sources.
  - **_\*.fx.yaml_** - The formulas extracted from the `control.json` file.
    > [!NOTE]
    > This is the place to edit your formulas.
  - **_CanvasManifest.json_** - A manifest file that contains the information normally present in the header, properties, and publishInfo.
  - **_\*.json_** - The raw `control.json` file.
  - **_\EditorState\*.editorstate.json_** - Cached information for Power Apps Studio to use.
- **\DataSources** - All the data sources used by the app.
- **\Connections** - Connection instances saved with the app and used when reloading into Power Apps Studio.
- **\Assets** - Media files embedded in the app.
- **\pkgs** - A downloaded copy of external references, such as templates, API definition files, and component libraries. These are similar to NuGet/NPM references.
- **\other** - All miscellaneous files needed to re-create the `.msapp`.
  - **_entropy.json_** - Volatile elements (like timestamps) are extracted to this file. This helps reduce noisy differences in other files while ensuring that we can still round-trip.
  - Holds other files from the msapp, such as what's in \references.

### File format

The `.fx.yaml` files use a subset of [YAML](https://yaml.org/spec/1.2/spec.html). Similar to Excel, all expressions should begin with an equal sign `=`. More information: [Power Fx YAML Formula Grammar](/power-platform/power-fx/yaml-formula-grammar)

### Merging changes with Power Apps Studio

When merging changes that are made in two different Power Apps Studio sessions:

- Ensure that all the control names are unique. For example, inserting a button in two different sessions can result in two `Button1` controls. We recommend that you name the controls soon after you create them. The tool doesn't accept two controls with the same name.
- For these files, merge them as you normally do:
  - \src\*.fx.yaml
- If there are conflicts or errors, you can delete these files:
  - \src\editorstate\*.json - These files contain optional information in Power Apps Studio.
  - \other\entropy.json
- For any conflicts in these files, it's ok to accept the latest version:
  - \checksum.json
- If there are any merge conflicts under these paths, it isn't safe to merge. Let us know if this happens often; we'll work on restructuring the file format to avoid conflicts.
  - \Connections\*
  - \DataSources\*
  - \pkgs\*
  - CanvasManifest.json

### Open source

The canvas commands in Microsoft Power Platform CLI are open source. Discuss improvements, raise issues, and access the code from [Power Apps language tooling repository](https://github.com/microsoft/PowerApps-Language-Tooling).

### See also

[What is Microsoft Power Platform CLI?](../introduction.md)

[!INCLUDE [footer-banner](../../../includes/footer-banner.md)]