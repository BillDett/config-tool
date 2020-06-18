# Config Tool

The Quay Config Tool implements several features to capture and validate configuration data based on a predefined schema.

This tool includes the following features:

- Validate Quay configuration using CLI tool
- Generate code for custom field group definitions (includes structs, constructors, defaults)
- Validation tag support from [Validator](https://github.com/go-playground/validator)
- Built-in validator tags for OAuth and JWT structs

## Installation

Clone this repository:

```
git clone github.com/quay/config-tool
cd config-tool
```

Generate files and install from source:

```
make all
```

This will generate files for the Quay validator executable and install the `config-tool` CLI tool.

#### Note: By default, this tool will generate an executable from a pre-built Config definition. For usage on writing a custom Config definition see [here](https://github.com/quay/config-tool/utils/generate)

## Usage

The CLI tool contains two main commands:

#### The `print` command is used to output the entire configuration with defaults specified

```
{
        "HostSettings": (*fieldgroups.HostSettingsFieldGroup)({
                ServerHostname: "quay:8081",
                PreferredURLScheme: "https",
                ExternalTLSTermination: false
        }),
        "TagExpiration": (*fieldgroups.TagExpirationFieldGroup)({
                FeatureChangeTagExpiration: false,
                DefaultTagExpiration: "2w",
                TagExpirationOptions: {
                        "0s",
                        "1d",
                        "1w",
                        "2w",
                        "4w"
                }
        }),
        "UserVisibleSettings": (*fieldgroups.UserVisibleSettingsFieldGroup)({
                RegistryTitle: "Project Quay",
                RegistryTitleShort: "Project Quay",
                SearchResultsPerPage: 10,
                SearchMaxResultPageCount: 10,
                ContactInfo: {
                },
                AvatarKind: "local",
                Branding: (*fieldgroups.BrandingStruct)({
                        Logo: "not_a_url",
                        FooterIMG: "also_not_a_url",
                        FooterURL: ""
                })
        })
}
```

#### The `validate` command is used to show while field groups have been validated succesully

```sh
$ config-tool validate -c <path-to-config.yaml>
+---------------------+--------------------+-------------------------+--------+
|     FIELD GROUP     |       FIELD        |          ERROR          | STATUS |
+---------------------+--------------------+-------------------------+--------+
| HostSettings        | -                  | -                       | 🟢     |
| TagExpiration       | -                  | -                       | 🟢     |
| UserVisibleSettings | Branding.Logo      | Field enforces tag: url | 🔴     |
|                     | Branding.FooterIMG | Field enforces tag: url | 🔴     |
+---------------------+--------------------+-------------------------+--------+
```
