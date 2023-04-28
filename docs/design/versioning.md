# Versioning

Below is a set of descriptions and examples describing how versioning works using a sample system named `Mail`. It contains four components; `Mail Sender`, `POP Server`, `SMTP Server`, and `Web Client`. The components are split between the two applications; `Mail Client` and `Mass Mailer`.

```text
Mail
├─ Mail Client 
|  ├─ POP Server 
|  ├─ SMTP Server*
|  └─ Web Client 
└─ Mass Mailer  
   ├─ Mail Sender
   └─ SMTP Server*
```

*\* `SMTP Server` is shared by both applications.*

## Component

Components are versioned using two, dot separated numbers plus the short hash of the latest git commit of the component; `{COMP_REV}.{COMP_PATCH}+{GIT_HASH}`. The version is controlled by KubeFox and updated when a system tag is created using the following rules. `COMP_REV` is an integer increased when a change is made to the component. `COMP_PATCH` is an integer increased when a system tag is made from a branch that was created from an existing tag, `COMP_REV` is not increased in this case.

## Application

Applications are versioned using three, dot separated numbers plus the short hash of the latest git commit of all components; `{APP_MAJOR}.{APP_MINOR}.{APP_PATCH}+{GIT_HASH}`. The version is controlled by KubeFox and updated when a system tag is created using the following rules. `APP_MAJOR` is an integer increased when a component is added to or removed from the application. `APP_MINOR` is an integer increased when a component of the application has a version change. `APP_MINOR` is not reset when `APP_MAJOR` is increased. `APP_PATCH` is an integer increased when a system tag is made from a branch that was created from an existing tag.

## System

Systems are versioned using three, dot separated numbers plus the short hash of the latest git commit of all components; `{SYS_MAJOR}.{SYS_MINOR}.{SYS_PATCH}+{GIT_HASH}`. The version is controlled by KubeFox and updated when a system tag is created using the following rules. `SYS_MAJOR` is an integer increased when an application is added to or removed from the system. `SYS_MINOR` is an integer increased when an application of the system has a version change. `SYS_MINOR` is not reset when `SYS_MAJOR` is increased. `SYS_PATCH` is an integer increased when a system tag is made from a branch that was created from an existing tag.

### System Tag

A system tag is synonymous with a git tag and is used as an identifier for a snapshot of the system in time. When a tag is created KubeFox will inspect the Git repository and find all components that have changed since the last tag of the branch, ensure that those components versions have been revved, and generate an artifact recording the versions of all applications, components, and the container hashes that correspond to those components. System tags can then be assigned to environments.

### System Patch

It is possible to create a patch (fix) to a system tag which is not the latest. Often this occurs when newer system tags include additional features or changes that are still being tested but a fix needs to be deployed quickly to an environment. To patch a system simply branch from an existing system tag. When the fix is ready the new branch can then be tagged. When a tag is created from a branch other than `main` KubeFox inspects the branch lineage to find the original system tag. If non is found, the tag is rejected. The major and minor versions for the applications and system are retained but the patch is increased based on rules above. Only patch version changes are allowed to be made to components when create a system patch. Components and applications cannot be added or removed.

## Example

Tag `A` created from branch `main`.

|                        | **Version** | **Git Hash** | **Commit Date** |
| ---------------------- | ----------- | ------------ | --------------- |
| Mail                   | 1.25.0      | ab883d2      | Jan 3           |
| `──` Mail Client       | 1.24.0      | ab883d2      | Jan 3           |
| `────` POP Server      | 5.0         | 223aa15      | Jan 1           |
| `────` SMTP Server     | 8.0         | 223aa15      | Jan 1           |
| `────` Web Client      | 14.0        | ab883d2      | Jan 3           |
| `──` Mass Mailer       | 1.5.0       | 9d3ea0b      | Jan 2           |
| `────` Mail Sender     | 3.0         | 9d3ea0b      | Jan 2           |
| `────` SMTP Server     | 8.0         | 223aa15      | Jan 1           |

Tag `B` created from branch `main` after feature added to component `Mail Sender`.

|                        | **Version** | **Git Hash** | **Commit Date** |
| ---------------------- | ----------- | ------------ | --------------- |
| Mail                   | **1.26.0**  | **611ab19**  | **Jan 4**       |
| `──` Mail Client       | 1.24.0      | ab883d2      | Jan 3           |
| `────` POP Server      | 5.0         | 223aa15      | Jan 1           |
| `────` SMTP Server     | 8.0         | 223aa15      | Jan 1           |
| `────` Web Client      | 14.0        | ab883d2      | Jan 3           |
| `──` Mass Mailer       | **1.6.0**   | **611ab19**  | **Jan 4**       |
| `────` Mail Sender     | **4.0**     | **611ab19**  | **Jan 4**       |
| `────` SMTP Server     | 8.0         | 223aa15      | Jan 1           |

Tag `C` created from branch `main` after feature added to shared component `SMTP Server`.

|                        | **Version** | **Git Hash** | **Commit Date** |
| ---------------------- | ----------- | ------------ | --------------- |
| Mail                   | **1.27.0**  | **443bc10**  | **Jan 5**       |
| `──` Mail Client       | **1.21.0**  | **443bc10**  | **Jan 5**       |
| `────` POP Server      | 5.0         | 223aa15      | Jan 1           |
| `────` SMTP Server     | **9.0**     | 223aa15      | Jan 1           |
| `────` Web Client      | 14.0        | ab883d2      | Jan 3           |
| `──` Mass Mailer       | **1.7.0**   | **443bc10**  | **Jan 5**       |
| `────` Mail Sender     | 4.0         | 611ab19      | Jan 4           |
| `────` SMTP Server     | **9.0**     | 223aa15      | Jan 1           |

Tag `B-hotfix` created from branch made from tag `B` after bug fix made to shared component `SMTP Server`.

|                        | **Version** | **Git Hash** | **Commit Date** |
| ---------------------- | ----------- | ------------ | --------------- |
| Mail                   | **1.26.1**  | **986dd00**  | **Jan 6**       |
| `──` Mail Client       | **1.24.1**  | **986dd00**  | **Jan 6**       |
| `────` POP Server      | 5.0         | 223aa15      | Jan 1           |
| `────` SMTP Server     | **8.1**     | **986dd00**  | **Jan 6**       |
| `────` Web Client      | 14.0        | ab883d2      | Jan 3           |
| `──` Mass Mailer       | **1.6.1**   | **986dd00**  | **Jan 6**       |
| `────` Mail Sender     | 4.0         | 611ab19      | Jan 4           |
| `────` SMTP Server     | **8.1**     | **986dd00**  | **Jan 6**       |
