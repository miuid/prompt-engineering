# Bash commands
## For UI projects
- `yarn build` : Build the project
- `yarn test:all` : Run all test with coverage

## For API/BFF projects
- `make start-local-dependencies` : Start local dependency services in docker
- `dotnet run --project xxx` : Run application locally
- `dotnet test` : Test api application

# Code style
## For UI projects (Xero SPA application)
Here is list of Xero XUI components that should be used for develop UI application:

- `XUIContentBlock`, `XUIContentBlockItem` - Content containers
- `XUIButton` - Buttons
- `XUIModal` - Modals
- `XUITable` - Tables
- `XUIStepper` - Step indicators
- `XUIIcon` - Icons, which import required icon paths, and Provide XUIIcon with imported icon path (for standardised viewbox sizing) or the icon object (for icon-specific sizing that is only as big as the underlying icon).
- `XUIIllustration` - Illustrations, a centralised illustrations repository to host illustrations, and a component to help teams implement illustrations consistently into products.
- `XUIRow`, `XUIColumn` - Grid layout, an assortment of components are available for adding quick, tested, standardised structure to pages.
- `XUIActionMenu`, `XUIActionMenuIconButtonTrigger`, `XUIActionMenuItems`, and `XUIActionMenuItem` - Action menus, to perform an action on the current page, or to provide users with options for initiating actions to interact with the page content. For example: “Edit“ or “Export as PDF”, or when there are space constraints in the layout
- `XUIButtonGroup` - Buttons can be grouped together (e.g. if their actions are all related)
- `XUISplitButtonGroup` - A split button group is used to present a primary action coupled with a dropdown trigger for secondary actions. The <XUISplitButtonGroup> component will inherit the isDisabled and the variant props from the parent component down to the button children. Provide an aria-label for the XUISecondaryButton to ensure accessibility.
- `XUIIconButton` - For buttons that only contain an icon
- `XUIActions` - Actions are used to wrap a selection of buttons (often two) and provide formatting. They can be embedded in a XUIModal, in a XUIPageHeader, in the footer of a XUIPanel, and other places. These are more generic than Banner Actions or Toast Actions.
- `XUICheckbox` - Checkbox component, supports properties for use with forms like the HTML checkbox input, including isRequired, name, and value.
- `XUICheckboxGroup` - Checkboxes can be grouped together, making it easier to include them alongside inputs and other form elements.
- `XUIRolloverCheckbox` - A simple toggle that will display a provided component by default but change to a checkbox on rollover.
- `XUIFileUploader` - is responsible for the file input and file list rendering, not the actual uploading process.
- `XUIRadio` - supports properties for use with forms like the HTML radio input, including isRequired, name, and value.
- `XUIRadioGroup` - Radios can be grouped together, which makes it easier to include them alongside inputs and other form elements. When grouping radios together, you still need to add a name to each Radio so that only one can be selected at a time.
- `XUIRange` - provides a custom range slider for consistency across platforms and visual alignment with other form elements.
- `XUISelectBox` - is an opinionated component which wraps XUIDropdown and XUIDropdownToggled. It's designed as a simple alternative to using an HTML `<select />`. If the user only needs to select a single item, use `XUISingleSelect`. ‘Single select’ is a new component that fixes accessibility issues within the ‘Select’ component.
- `XUISelectBoxOption` - A wrapper of XUIPickitem that assumes your children are only text. This means it can truncate the text when the prop truncateText is set to true. It also assumes you require the default layout of a Pickitem. The API for XUISelectBoxOption is otherwise very similar to XUIPickitems.
- `XUISingleSelect` - Single select allows users to select a value from a list of options in a flyout. It is functionally similar to a native `<select>`. Use it when to give users a single choice from a basic list of options, or commonly used within forms
- `XUISwitch` - uses an HTML checkbox under the hood and can be styled just as other control components.
- `XUISwitchGroup` - Switches can be grouped together, making it easier to include them alongside inputs and other form elements.
- `XUITextInput` - Most input use cases can be solved using XUITextInput's base props. Additional attributes that aren't available as base props can be passed down to the input via inputProps.
- `XUIToggle` - is a control that can behave like a radio, or like a checkbox. It supports different layout patterns for a variety of use cases. Avoid partially disabled groups in which one of the disabled options is pre-selected. This combination has been known to cause unexpected results for keyboard navigation.
- `XUIControlGroup` - Control groups are used to gather multiple controls into a single, connected row or column. Wrapping controls in additional elements is not recommended, as it is likely to prevent the grid layout from working correctly.
- `XUIAvatar` - Avatars come in two variants: Circular, used to represent people, and Rectangular, used to represent businesses. Both variants support the use of images. XUI provides ten approved colours for Avatars. XUIAvatar handles selecting a colour for you based on its contents and calculating the abbreviated text value from the full value you pass it. XUIAvatars can be grouped together using XUIAvatarGroup.
- `XUICapsule` - Capsules are used to draw attention to placeholders that will be replaced with data.
- `XUILoader` - is given a layout class by default. This is good for putting in large empty states, like panels, while loading data. Use the required ariaLabel prop to provide information to screen readers.
- `XUIPill` - Pills are used for signifying a selection has been made, either single or multiple. They can include the option to remove the selection with a delete button. To see pills used in context, refer to the Autocompleter section.
- `XUIProgressCircular` - The Progress Indicator comes in two main variants (Circular, Linear). They are isolated as individual components import { XUIProgressCircular, XUIProgressLinear } from '@xero/xui/react/progressindicator';.
- `XUITag` - Tags are used to categorise content or indicate sentiment.
- `XUIBanner`, `XUIBannerIcon`, `XUIBannerActions`, `XUIBannerMessage`, `XUIBannerMessageDetail` - Standard banners are given a layout class by default and with no actionable buttons. The close button is only added when a onCloseClick callback prop is added.
- `XUIIntroBannerBody` - is the only required subcomponent of the intro banner. It is a wrapper for the content of the intro banner, and provides the recommended default layout for this component.
- `XUIIntroBannerFooter` - is an optional component that can be used to display actions for an intro banner. This provides default responsive behaviour, with any provided XUIButton components becoming full-width buttons at mobile breakpoints. Add this component via the footer prop in XUIIntroBanner.
- `XUIPopover`, `XUIPopoverBody`, `XUIPopoverHeader`, `XUIPopoverFooter` - XUIPopover provides additional information in line with page elements. It is useful for providing information with interactive content, such as a link to another page or an onboarding sequence.
- `XUIToast`, `XUIToastActions`, `XUIToastMessage`, `XUIToastWrapper` - XUIToast is given a layout class by default with no actionable buttons. The close button is only added when a onCloseClick callback prop is added.
- `XUITooltip` - provides additional information in-line with page elements.
- `XUIBreadcrumbTrail` creates a list of sequenced nav items from a provided array. It is most commonly found in addition to a title in a XUIPageHeader. The array can contain objects used to generate anchor links, or it can contain HTML nodes which will have the xui-breadcrumb--link class added to them. As always, be mindful of accessibility concerns if using non-semantic elements for interactivity.
- `XUIIsolationHeader` replaces the standard global header for tasks that are part of a focused workflow.
- `XUIPageHeader` appears beneath the global header on a page. In a basic example, it is a white bar with a title. In more complex cases it could contain a XUIBreadcrumbTrail, a XUIAvatar, a XUITabs to present tabs for organising related content, a XUIPicklist to present navigating between pages with links, or a XUIActions component (and some combinations).
- `XUIPagination` is a useful and clear way to break up sets of data within a page that could be over an optimum length. An optimum length should be determined by things like digestibility, performance, data usage, screen real estate and sensible URL schema.
- `XUIStepper` renders a set of steps and a content area corresponding to the currently active step.
- `XUIFixedFooter` is an empty white bar with a top-edge shadow that remains fixed to the bottom of the viewport and prevents nearby elements from getting hidden behind it.
- `XUIAutocompleter` is a component that composes many other components together. It's an input where users can type to filter a list of items to select.
- `XUIDatePicker` is a date select component
- `XUIDateInput` and `XUIDateRangeInput` are an experimental components for selecting dates. They combine XUITextInput for keyboard interaction and XUIDatePicker for selecting dates from a calendar.
- `XUIDropdown` dropdown component
- `XUIDropdownToggled` connects the trigger element with the dropdown, in terms of behavior and wrapping the two elements for positioning.
- `XUIDropdownHeader` and `XUIDropdownFooter` are used to add a fixed header and/or footer element to dropdowns. These elements don't scroll with the rest of the list, and are ignored by the default arrow key handlers. Add these components via the header and footer props in XUIDropdown.
- `XUINestedDropdown` is designed as a XUIDropdown replacement that allows consumers to implement small, multi-step flows inside of a triggered dropdown. A quick example would be allowing the user to choose between some suggested dates and a fixed custom date like below.
- `XUIForm` is a component that wraps a group of form inputs, and provides a React Context that sets default properties on the fields. Note you must still markup the inputs as required where applicable.
- `XUIModal`, `XUIModalHeader`, `XUIModalHeading`, `XUIModalFooter` provides a container for custom content, along with a background mask. They should primarily be used for prompting user actions, such as confirming a change, providing additional information, or copying some text.
- `XUIAccordion`, `XUIAccordionItem` are used to display a vertically expandable & collapsible list that reveals and hides additional content
- `XUIContentBlock` components are typically used inside a Panel to display a simplified top-level view for a detailed sub page, often containing quick access to actions. If you are not using them inside a XUIPanel component, we recommend applying the xui-panel class to the `XUIContentBlock`, as per the examples below. Any content should be placed inside a `XUIContentBlockItem` component.
- `XUIEditableTable` are used to display sets of static or interactive data in a way that’s easy for the user to scan, organise, and manipulate.
- `XUIFilePreview` will fill its wrapping container with a header, footer, and body area to display a file preview. XUIFilePreviewHeader has an API very similar to XUIIsolationHeader, and XUIFilePreviewFooter can be populated with the recommended preview controls. We've shown a responsive toolbar that stows some preview control buttons (aligned to the left) behind an overflow menu at narrow widths, alongside a pagination component (aligned to the right).
- `XUIOverviewBlock` components are used to show a top-level summary of stats and figures. They should contain at least one and no more than six `XUIOverviewSection` components.
- `XUIPanel` are top-level containers for grouping page content. XUIPanel can optionally accept XUIPanelHeader and XUIPanelFooter components as props, as well as content designated as a sidebar. XUIPanelHeading and XUIPanelSectionHeading components can also be passed through as the first child component of XUIPanel and XUIPanelSection respectively, if required.
- `XUIPicklist` is set of components that brings in the XUI styles to render a list of items. XUIPicklist and XUIPickitems are presentational components, and XUIStatefulPicklist is a wrapper available to handle keyboard navigation.
- `XUITable`, `XUITableColumn`, `XUITableCell` The Table scaffold is a convenient way to lay out data sets with an accessible and responsive design mindset.
- `XUITabs` A set of tab elements and their tab panels
- `XUICompositionDetail`, `XUICompositionDetailSummary`, `XUICompositionMasterDetail`, `XUICompositionMasterDetailSummary`, `XUICompositionSplit` are compositions for divide screen to master (left side navigation) and detail (right side content) view.

For Styling:
- Component-specific styles: `./ComponentName.scss`
- Xero sass styling standard is at @https://github.com/xero-internal/xui/tree/main/src/sass
- Xero SPA guide is at @https://github.com/xero-internal/spa-boilerplate-tools/blob/master/docs/guides.md
- Create component classes: `${createClass("modifier")}`

## For API projects (includes BFF applications)

Basic project structure like
```
src/
├── Controllers/           # API endpoints
├── Application/Services/  # Business logic layer
├── Common/                # Helpers, extensions, and utilities
├── Configuration/         # Configuration models
├── Domain/                # Domain models, exceptions, extensions and entities
├── Infrastructure/
│   ├── ApiClients/       # Downstream API communication
│   ├── XeroHttpHeaders/  # Header management
│   └── Extensions/       # Extension methods
├── Interfaces/           # Contracts and abstractions
└── Infrastructure/       # Api clients, XeroHeaders, LaunchDarkly, Logger, and other infrastructure concerns
```

### Controller

### Services

### HttpClient

- HttpClient should be registered by type

### Header Propagation

Headers are important in Xero applications. Common headers are `Xero-Correlation-Id`, `Xero-User-Id`, `Xero-Tenant-Id`, `Xero-Client-Name`, `Xero-Payments-Origin`, and `Xero-Payments-PartnerCode`. Most of the time these headers or some of them need to be passed to the downstream API calls

### Error handling

# Workflow
- Think harder before you start to develop any solution
- Ask for clarifications whenever you have doubts
- Ask for more references if you cannot find a good example to reference to
- Prefer running single tests, and not the whole test suite, for performance