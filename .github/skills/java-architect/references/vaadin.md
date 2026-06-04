# Vaadin Flow Reference

Use this when building server-side Java UIs with Vaadin Flow, especially Spring Boot applications where the UI is part of the same JVM as the service layer.

## Version And Project Fit

- Prefer the Vaadin version already declared in the project BOM or parent.
- Use Java 21 and Spring Boot 3 conventions.
- Keep views thin: navigation, components, Binder wiring, user feedback, and calls to application services.
- Keep business rules in services and domain classes.
- Keep persistence behind services/repositories; never build ad hoc database access in a view.

## Recommended Package Shape

```text
src/main/java/com/example/
├── application/          # use cases and DTOs
├── domain/               # entities, value objects, domain services
├── infrastructure/       # persistence, security, config
└── ui/
    ├── MainLayout.java
    ├── view/
    ├── component/
    └── form/
```

## Gradle Dependencies

Use the Gradle build file. A typical Spring Boot + Vaadin setup includes:

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.3.5'
    id 'io.spring.dependency-management' version '1.1.6'
    id 'com.vaadin' version '24.5.7'
}

dependencies {
    implementation 'com.vaadin:vaadin-spring-boot-starter'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    runtimeOnly 'com.h2database:h2'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

## Main Layout

```java
package com.example.ui;

import com.example.ui.view.DashboardView;
import com.example.ui.view.PropertiesView;
import com.vaadin.flow.component.applayout.AppLayout;
import com.vaadin.flow.component.applayout.DrawerToggle;
import com.vaadin.flow.component.html.H1;
import com.vaadin.flow.component.orderedlayout.Scroller;
import com.vaadin.flow.component.sidenav.SideNav;
import com.vaadin.flow.component.sidenav.SideNavItem;
import com.vaadin.flow.router.RouterLink;
import com.vaadin.flow.theme.lumo.LumoUtility;

public class MainLayout extends AppLayout {

    public MainLayout() {
        H1 title = new H1("Ireland Rent Dashboard");
        title.addClassNames(LumoUtility.FontSize.LARGE, LumoUtility.Margin.NONE);

        addToNavbar(new DrawerToggle(), title);
        addToDrawer(new Scroller(createNavigation()));
    }

    private SideNav createNavigation() {
        SideNav nav = new SideNav();
        nav.addItem(new SideNavItem("Dashboard", DashboardView.class));
        nav.addItem(new SideNavItem("Properties", PropertiesView.class));
        return nav;
    }
}
```

## Route View Pattern

```java
package com.example.ui.view;

import com.example.application.PropertyService;
import com.example.application.dto.PropertySummary;
import com.example.ui.MainLayout;
import com.vaadin.flow.component.button.Button;
import com.vaadin.flow.component.grid.Grid;
import com.vaadin.flow.component.html.H2;
import com.vaadin.flow.component.notification.Notification;
import com.vaadin.flow.component.orderedlayout.VerticalLayout;
import com.vaadin.flow.router.PageTitle;
import com.vaadin.flow.router.Route;
import jakarta.annotation.security.PermitAll;

@Route(value = "properties", layout = MainLayout.class)
@PageTitle("Properties")
@PermitAll
public class PropertiesView extends VerticalLayout {

    private final PropertyService propertyService;
    private final Grid<PropertySummary> grid = new Grid<>(PropertySummary.class, false);

    public PropertiesView(PropertyService propertyService) {
        this.propertyService = propertyService;

        setSizeFull();
        configureGrid();

        Button refresh = new Button("Refresh", event -> refreshGrid());
        add(new H2("Properties"), refresh, grid);
        refreshGrid();
    }

    private void configureGrid() {
        grid.addColumn(PropertySummary::county).setHeader("County").setAutoWidth(true);
        grid.addColumn(PropertySummary::averageRent).setHeader("Average rent").setAutoWidth(true);
        grid.addColumn(PropertySummary::listingCount).setHeader("Listings").setAutoWidth(true);
        grid.setSizeFull();
    }

    private void refreshGrid() {
        grid.setItems(propertyService.findSummaries());
        Notification.show("Properties refreshed", 2000, Notification.Position.BOTTOM_START);
    }
}
```

## Binder Form Pattern

```java
package com.example.ui.form;

import com.example.application.dto.PropertyCommand;
import com.vaadin.flow.component.button.Button;
import com.vaadin.flow.component.formlayout.FormLayout;
import com.vaadin.flow.component.textfield.BigDecimalField;
import com.vaadin.flow.component.textfield.TextField;
import com.vaadin.flow.data.binder.BeanValidationBinder;
import com.vaadin.flow.data.binder.Binder;

public class PropertyForm extends FormLayout {

    private final Binder<PropertyCommand> binder = new BeanValidationBinder<>(PropertyCommand.class);
    private final TextField county = new TextField("County");
    private final BigDecimalField rent = new BigDecimalField("Rent");
    private final Button save = new Button("Save");

    public PropertyForm() {
        binder.bindInstanceFields(this);
        add(county, rent, save);
    }

    public void setValue(PropertyCommand command) {
        binder.setBean(command);
    }

    public void onSave(Runnable handler) {
        save.addClickListener(event -> {
            if (binder.validate().isOk()) {
                handler.run();
            }
        });
    }
}
```

## Template Engine Pattern

When the workshop asks for a "template engine", treat it as a small deterministic generator for Vaadin views, forms, services, DTOs, and tests.

Recommended placeholders:

```text
{{featureName}}         Property
{{featureNamePlural}}   Properties
{{route}}               properties
{{packageName}}         com.example
{{entityName}}          Property
{{dtoName}}             PropertySummary
{{serviceName}}         PropertyService
{{fields}}              county:String,rent:BigDecimal
```

Generation rules:

- Generate into the existing package layout and naming style.
- Preserve imports and formatting with the project's formatter.
- Create one route view, one form component when the issue needs input UI, one DTO/command record, one service method boundary, and tests.
- Do not overwrite existing files. Patch existing files or create new generated files.
- After generation, run compile/tests before claiming success.

## Security Pattern

```java
@Route(value = "admin", layout = MainLayout.class)
@RolesAllowed("ADMIN")
public class AdminView extends VerticalLayout {
}
```

Use `@AnonymousAllowed` only for login/help/public routes. Use `@PermitAll` for authenticated users.

## Testing

- Unit test service behavior separately from the Vaadin view.
- Use Spring Boot smoke tests to verify routes and dependency injection.
- Add a browser smoke test when the app already has Playwright/Selenium/TestBench.
- For live demos, verify at minimum: app starts, target route loads, grid/form renders, save/search action works, and invalid input shows feedback.
