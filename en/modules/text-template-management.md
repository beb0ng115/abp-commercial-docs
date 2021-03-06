# Text Template Management Module

This module is used to store and edit template contents for [the text templating system](https://docs.abp.io/en/abp/latest/Text-Templating) of the ABP framework. So, you may need to understand it to better understand the purpose of this module.

There are different use cases of the text templating system. For example, [the Account Module](Account.md) is using it to define templates for sending emails when it needs to send emails to users (like sending "password reset link" email). This module provides UI to easily edit these email templates.

See [the module description page](https://commercial.abp.io/modules/Volo.TextTemplateManagement) for an overview of the module features.

## How to Install

Text Template Management module is pre-installed in [the startup templates](../Startup-Templates/Index). So, no need to manually install it.

## Packages

This module follows the [module development best practices guide](https://docs.abp.io/en/abp/latest/Best-Practices/Index) and consists of several NuGet and NPM packages. See the guide if you want to understand the packages and relations between them.

### NuGet Packages

* Volo.Abp.TextTemplateManagement.Domain.Shared
* Volo.Abp.TextTemplateManagement.Domain
* Volo.Abp.TextTemplateManagement.Application.Contracts
* Volo.Abp.TextTemplateManagement.Application
* Volo.Abp.TextTemplateManagement.EntityFrameworkCore
* Volo.Abp.TextTemplateManagement.MongoDB
* Volo.Abp.TextTemplateManagement.HttpApi
* Volo.Abp.TextTemplateManagement.HttpApi.Client
* Volo.Abp.TextTemplateManagement.Web

### NPM Packages

* @volo/abp.ng.text-template-management

## User Interface

### Menu Items

Text Template Management module adds the following items to the "Main" menu, under the "Administration" menu item:

* **Text Templates**: List, view and filter text templates.

`TextTemplateManagementMainMenuNames` class has the constants for the menu item names.

### Pages

#### Text Templates

Text Templates page is used to view the list of templates defined in the application.

![text-template-managemet-text-templates-page](../images/text-template-managemet-text-templates-page.png)

Click to the `Actions -> Edit Contents` to edit content for a template. There are two types of UI to edit a template content:

##### Editing Content for Inline Localized Templates

This kind of templates uses the `L` function to perform inline localization. In this way, it is easier to manage the template for different cultures.

![text-template-management-inline-edit](../images/text-template-management-inline-edit.png)

##### Editing Contents for Culture-Specific Templates

This kind of templates provides different content for each culture. In this way, you can define a completely different content for a specific culture.

![text-template-management-multiple-culture-edit](../images/text-template-management-multiple-culture-edit.png)

## Data Seed

This module doesn't seed any data.

## Internals

### Domain Layer

#### Aggregates

This module follows the [Entity Best Practices & Conventions](https://docs.abp.io/en/abp/latest/Best-Practices/Entities) guide.

##### TextTemplateContent

* `TextTemplateContent` (aggregate root): Represents a content of text template.

#### Repositories

This module follows the [Repository Best Practices & Conventions](https://docs.abp.io/en/abp/latest/Best-Practices/Repositories) guide.

Following custom repositories are defined for this module:

* `ITextTemplateContentRepository`

#### Domain Services

This module follows the [Domain Services Best Practices & Conventions]( https://docs.abp.io/en/abp/latest/Best-Practices/Domain-Services) guide.

##### DatabaseTemplateContentContributor

`DatabaseTemplateContentContributor` is used by `ITemplateContentProvider` to get template contents that stored in DB.

### Settings

This module doesn't define any setting.

### Application Layer

#### Application Services

* `TemplateDefinitionAppService` (implements `ITemplateDefinitionAppService`): Implements the use cases of the text template management UI.
* `TemplateContentAppService` (implements `ITemplateContentAppService`): Implements the use cases of the text template management UI.

### Database Providers

#### Common

##### Table/Collection Prefix & Schema

All tables/collections use the `Abp` prefix by default. Set static properties on the `TextTemplateManagementDbProperties` class if you need to change the table prefix or set a schema name (if supported by your database provider).

##### Connection String

This module uses `TextTemplateManagement` for the connection string name. If you don't define a connection string with this name, it fallbacks to the `Default` connection string.

See the [connection strings](https://docs.abp.io/en/abp/latest/Connection-Strings) documentation for details.

#### Entity Framework Core

##### Tables

* **AbpTextTemplateContents**

#### MongoDB

##### Collections

* **AbpTextTemplateContents**

### Permissions

See the `TextTemplateManagementPermissions` class members for all permissions defined for this module.


### Angular UI

#### Installation

In order to configure the application to use the `TextTemplateManagementModule`, you first need to import `TextTemplateManagementConfigModule` from `@volo/abp.ng.text-template-management/config` to root module. `TextTemplateManagementConfigModule` has a static `forRoot` method which you should call for a proper configuration.

```js
// app.module.ts
import { TextTemplateManagementConfigModule } from '@volo/abp.ng.text-template-management/config';

@NgModule({
  imports: [
    // other imports
    TextTemplateManagementConfigModule.forRoot(),
    // other imports
  ],
  // ...
})
export class AppModule {}
```

The `TextTemplateManagementModule` should be imported and lazy-loaded in your routing module. It has a static `forLazy` method for configuration. Available options are listed below. It is available for import from `@volo/abp.ng.text-template-management`.

```js
// app-routing.module.ts
const routes: Routes = [
  // other route definitions
  {
    path: 'text-template-management',
    loadChildren: () =>
      import('@volo/abp.ng.text-template-management').then(m => m.TextTemplateManagementModule.forLazy(/* options here */)),
  },
];

@NgModule(/* AppRoutingModule metadata */)
export class AppRoutingModule {}
```

> If you have generated your project via the startup template, you do not have to do anything, because it already has both `TextTemplateManagementConfigModule` and `TextTemplateManagementModule`.

<h4 id="h-text-template-management-module-options">Options</h4>

You can modify the look and behavior of the module pages by passing the following options to `TextTemplateManagementModule.forLazy` static method:

- **entityActionContributors:** Changes grid actions. Please check [Entity Action Extensions for Angular](https://docs.abp.io/en/commercial/latest/ui/angular/entity-action-extensions) for details.
- **toolbarActionContributors:** Changes page toolbar. Please check [Page Toolbar Extensions for Angular](https://docs.abp.io/en/commercial/latest/ui/angular/page-toolbar-extensions) for details.
- **entityPropContributors:** Changes table columns. Please check [Data Table Column Extensions for Angular](https://docs.abp.io/en/commercial/latest/ui/angular/data-table-column-extensions) for details.


#### Services

The `@volo/abp.ng.text-template-management` package exports the following services which cover HTTP requests to counterpart APIs:

- **TemplateDefinitionService:** Covers several methods that performing HTTP calls for `Text templates` page.
- **TemplateContentService:** Covers several methods that performing HTTP calls for `Create update template content` page.


#### TextTemplateManagementModule Replaceable Components

`eTextTemplateManagementComponents` enum provides all replaceable component keys. It is available for import from `@volo/abp.ng.text-template-management`.

Please check [Component Replacement document](https://docs.abp.io/en/abp/latest/UI/Angular/Component-Replacement) for details.


#### Remote Endpoint URL

The Text Template Management module remote endpoint URL can be configured in the environment files.

```js
export const environment = {
  // other configurations
  apis: {
    default: {
      url: 'default url here',
    },
    TextTemplateManagement: {
      url: 'Text Template Management remote url here'
    }
    // other api configurations
  },
};
```

The Text Template Management module remote URL configuration shown above is optional. If you don't set a URL, the `default.url` will be used as fallback.


## Distributed Events

This module doesn't define any additional distributed event. See the [standard distributed events](https://docs.abp.io/en/abp/latest/Distributed-Event-Bus).
