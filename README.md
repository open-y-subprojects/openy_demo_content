# **OpenY Demo Content**

This module provides demonstration content for a YMCA Website Services distribution. Its primary purpose is to populate a fresh OpenY installation with sample content, including nodes, media, taxonomy terms, blocks, and menu links, to showcase the platform's capabilities and features.

The module leverages Drupal's Migrate API, specifically using the migrate\_plus and migrate\_source\_embedded\_data modules, to define and execute content imports from data embedded directly within the migration configuration files.

## **Installation**

1. Ensure the module and its dependencies are available in your Drupal installation's modules directory.  
2. Enable the OpenY Demo Content module and its submodules through the Drupal administration interface (/admin/modules) or using Drush:  
   drush en openy\_demo\_content \-y

   Enabling the main module will automatically enable its submodules.  
3. Run the migrations to import the demo content. You can do this via the Drupal UI (/admin/structure/migrate) by executing the migration groups provided by this module, or using Drush:  
   drush migrate:import \--group=openy\_demo\_content \--all

   Note that migration groups have dependencies, so running the main group should execute migrations in the correct order.


### **Key Directories and Files**

* openy\_demo\_content.info.yml: The main module's info file, defining dependencies and enabling submodules.  
* modules/: Contains all the submodules, each providing specific demo content.  
* modules/\*/config/install/: Contains the migration configuration files (migrate\_plus.migration.\*.yml and migrate\_plus.migration\_group.\*.yml) and other configuration needed on module installation (like simple\_sitemap settings).  
* modules/\*/assets/: Contains static assets used by the migrations, such as images (images/) and icons (icons/).  
* modules/\*/src/Plugin/migrate/process/: Contains custom Migrate API process plugins, like OpenYMenuIcon.php for handling menu link icons.

## **Migrations**

The core of this module is its extensive set of migrations, defined in the config/install directories of its submodules.

### **Migration Strategy**

* **embedded\_data Source:** Migrations primarily use the embedded\_data source plugin. This means the source data for the migration is defined directly within the .yml migration file itself, making the demo content self-contained within the module.  
* **Migration Groups:** Migrations are organized into groups (e.g., openy\_demo\_nblog, openy\_demo\_ncamp) using migrate\_plus.migration\_group.\*.yml files. This allows related migrations to be run together.  
* **Dependencies:** Migrations utilize dependencies (migration\_dependencies) to ensure they run in the correct order. For example, migrations that create Media entities depend on the migrations that create the corresponding File entities. Paragraph migrations often depend on node or other entity migrations.  
* **File and Media Migrations:** A common pattern is the separation of file and media migration into two steps:  
  1. A migration (e.g., openy\_demo\_nblog\_file) imports the physical file into Drupal's managed file system using the file\_copy process plugin.  
  2. A subsequent migration (e.g., openy\_demo\_nblog\_media\_image) creates the Media entity and links it to the file created in the first step using the migration\_lookup process plugin on the file reference field (field\_media\_image).  
* **Custom Process Plugins:** The module can include custom logic for migration processes. An example is OpenYMenuIcon.php, which likely handles specific logic for migrating menu link icons.

### **Adding or Modifying Demo Content**

To add new demo content or modify existing content:

1. Identify the relevant submodule or create a new one if adding a new type of content.  
2. Define the source data within the source: data\_rows section of the appropriate migration .yml file.  
3. Configure the process section to map the source data fields to the destination entity's fields. Use appropriate process plugins (e.g., get, default\_value, migration\_lookup, entity\_generate, sub\_process, custom plugins).  
4. If migrating files or media, ensure the two-step process (file migration first, then media migration depending on the file migration) is followed.  
5. Add or update migration dependencies as needed to ensure correct execution order.  
6. Clear Drupal's cache (drush cr) after adding or modifying migration files.  
7. Run the relevant migration or migration group.

## **Extending**

This module provides a solid example of using the Migrate API with embedded\_data. Developers can use this structure as a template for creating their own custom demo content modules or for understanding how to migrate content into a YMCA Website Services distribution.

## **Structure**

The openy\_demo\_content module is structured as a main module with several submodules. Each submodule is responsible for providing demo content for a specific content type, entity type, or functionality area.

```

openy\_demo\_content/  
├── composer.json  
├── README.md          \<-- This file  
├── openy\_demo\_content.info.yml  
├── modules/  
│   ├── openy\_demo\_ahb/         \# Demo content for Advanced Help Blocks  
│   │   ├── config/install/  
│   │   │   └── migrate\_plus.migration.openy\_demo\_entity\_ahb.yml  
│   │   └── openy\_demo\_ahb.info.yml  
│   ├── openy\_demo\_nalert/      \# Demo content for Alert nodes  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_alert.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_alert\_standard\_extended.yml  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_nalert.yml  
│   │   └── openy\_demo\_nalert.info.yml  
│   ├── openy\_demo\_nbranch/     \# Demo content for Branch nodes and related paragraphs/media  
│   │   ├── assets/images/      \# Demo images for branches  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_nbranch\_file.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_nbranch\_media\_image.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_branch.yml  
│   │   │   ├── migrate\_plus.migration\_group.openy\_demo\_nbranch.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_prgf\_3c\_branch.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_prgf\_branch\_contacts\_info.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_prgf\_gallery.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_prgf\_grid\_columns.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_prgf\_grid\_content.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_prgf\_news\_branch.yml  
│   │   └── openy\_demo\_nbranch.info.yml  
│   ├── openy\_demo\_nblog/       \# Demo content for Blog nodes and related media  
│   │   ├── assets/images/      \# Demo images for blog posts  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_nblog\_file.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_nblog\_media\_image.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_blog.yml  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_nblog.yml  
│   │   └── openy\_demo\_nblog.info.yml  
│   ├── openy\_demo\_ncamp/       \# Demo content for Camp nodes and related paragraphs/media  
│   │   ├── assets/images/      \# Demo images for camps  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_ncamp\_file.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_ncamp\_media\_image.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_camp.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_camp\_news.yml  
│   │   │   ├── migrate\_plus.migration\_group.openy\_demo\_ncamp.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_prgf\_camp\_gallery.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_prgf\_menu\_camp.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_prgf\_simple\_camp.yml  
│   │   └── openy\_demo\_ncamp.info.yml  
│   ├── openy\_demo\_ncategory/   \# Demo content for Program Category nodes and related paragraphs/media  
│   │   ├── assets/images/      \# Demo images for program categories  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_ncategory\_file.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_ncategory\_media\_image.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_program\_subcategory.yml  
│   │   │   ├── migrate\_plus.migration\_group.openy\_demo\_ncategory.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_branches\_popup\_all.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_classes\_listing.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_paragraph\_classes\_listing\_filters.yml  
│   │   └── openy\_demo\_ncategory.info.yml  
│   ├── openy\_demo\_nclass/      \# Demo content for Class/Activity nodes and related paragraphs  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_activity.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_class\_01.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_class\_02.yml  
│   │   │   ├── migrate\_plus.migration\_group.openy\_demo\_nclass.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_branches\_popup\_class\_01.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_branches\_popup\_class\_02.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_class\_location\_01.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_class\_location\_02.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_class\_sessions\_01.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_paragraph\_class\_sessions\_02.yml  
│   │   └── openy\_demo\_nclass.info.yml  
│   ├── openy\_demo\_nevent/      \# Demo content for Event nodes and related media/paragraphs  
│   │   ├── assets/images/      \# Demo images for events  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_event\_landing.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_nevent\_file.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_nevent\_media\_image.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_event.yml  
│   │   │   ├── migrate\_plus.migration\_group.openy\_demo\_nevent.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_event\_posts\_listing.yml  
│   │   └── openy\_demo\_nevent.info.yml  
│   ├── openy\_demo\_nfacility/   \# Demo content for Facility nodes and related paragraphs  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_facility\_paragraph\_simple.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_facility.yml  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_nfacility.yml  
│   │   └── openy\_demo\_nfacility.info.yml  
│   ├── openy\_demo\_nhome\_alt/   \# Alternate demo home page content  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_home\_alt\_landing.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_home\_alt\_simple.yml  
│   │   └── openy\_demo\_nhome\_alt.info.yml  
│   ├── openy\_demo\_nlanding/    \# Demo content for Landing pages and various paragraphs/media  
│   │   ├── assets/images/      \# Demo images for landing pages  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_landing\_paragraph\_branches\_popup\_all.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_file.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_media\_image.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_all\_amenities.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_banner.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_blog\_posts\_listing.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_embedded\_groupexpro\_schedule.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_featured\_blogs.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_featured\_content.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_grid\_columns.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_grid\_content.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_latest\_news\_posts.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_loc\_by\_amnts.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_location\_finder.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_location\_finder\_filters.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_membership\_calculator.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_menu\_camp.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_microsites\_menu.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_schedule\_search\_form.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_schedule\_search\_list.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_simple.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_small\_banner.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_teaser.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_landing.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_node\_landing\_standard.yml  
│   │   └── openy\_demo\_nlanding.info.yml  
│   ├── openy\_demo\_nmbrshp/   \# Demo content for Membership nodes and related paragraphs/media  
│   │   ├── assets/images/    \# Demo images for membership types  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_membership\_file.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_membership\_media\_image.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_membership\_paragraph\_membership\_info.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_membership.yml  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_nmbrshp.yml  
│   │   └── openy\_demo\_nmbrshp.info.yml  
│   ├── openy\_demo\_nnews/       \# Demo content for News nodes and related media/paragraphs/menu links  
│   │   ├── assets/images/      \# Demo images for news posts  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_news\_paragraph\_latest\_news\_posts.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_news\_posts\_listing.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_lp\_paragraph\_news\_small\_banner.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_menu\_link\_footer\_news.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_news\_landing.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_nnews\_file.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_nnews\_media\_image.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_news.yml  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_nnews.yml  
│   │   └── openy\_demo\_nnews.info.yml  
│   ├── openy\_demo\_nsessions/   \# Demo content for Session nodes and related paragraphs  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_session\_01.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_session\_02.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_session\_03.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_session\_04.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_session\_05.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_node\_session\_06.yml  
│   │   │   ├── migrate\_plus.migration\_group.openy\_demo\_nsessions.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_session\_time\_01.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_session\_time\_02.yml  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_paragraph\_session\_time\_03.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_paragraph\_session\_time\_04.yml  
│   │   └── openy\_demo\_nsessions.info.yml  
│   ├── openy\_demo\_tamenities/  \# Demo content for Amenities taxonomy terms and media  
│   │   ├── assets/icons/amenities/ \# Demo icons for amenities  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_amenities\_file.yml  
│   │   │   └── migrate\_plus.migration.openy\_demo\_taxonomy\_term\_amenities.yml  
│   │   └── openy\_demo\_tamenities.info.yml  
│   ├── openy\_demo\_tarea/       \# Demo content for Area taxonomy terms  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_taxonomy\_term\_area.yml  
│   │   │   └── simple\_sitemap.bundle\_settings.taxonomy\_term.area.yml  
│   │   └── openy\_demo\_tarea.info.yml  
│   ├── openy\_demo\_tblog/       \# Demo content for Blog Category taxonomy terms  
│   │   ├── config/install/  
│   │   │   └── migrate\_plus.migration.openy\_demo\_taxonomy\_term\_blog\_category.yml  
│   │   └── openy\_demo\_tblog.info.yml  
│   ├── openy\_demo\_tfacility/   \# Demo content for Facility Type taxonomy terms  
│   │   ├── config/install/  
│   │   │   └── migrate\_plus.migration.openy\_demo\_taxonomy\_term\_facility\_type.yml  
│   │   └── openy\_demo\_tfacility.info.yml  
│   ├── openy\_demo\_tfitness/    \# Demo content for Fitness Category taxonomy terms  
│   │   ├── config/install/  
│   │   │   └── migrate\_plus.migration.openy\_demo\_taxonomy\_term\_fitness\_category.yml  
│   │   └── openy\_demo\_tfitness.info.yml  
│   ├── openy\_demo\_addthis/     \# Demo configuration for AddThis module  
│   │   └── openy\_demo\_addthis.info.yml  
│   ├── openy\_demo\_bamenities/  \# Demo content for Branch Amenities blocks  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_block\_content\_amenities.yml  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_branch\_amenities.yml  
│   │   └── openy\_demo\_bamenities.info.yml  
│   ├── openy\_demo\_bmicrosites\_menu/ \# Demo content for Microsites Menu blocks  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_block\_microsites\_menu.yml  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_mblock.yml  
│   │   └── openy\_demo\_bmicrosites\_menu.info.yml  
│   ├── openy\_demo\_bsimple/     \# Demo content for Simple blocks  
│   │   ├── config/install/  
│   │   │   ├── migrate\_plus.migration.openy\_demo\_block\_content\_simple.yml  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_simple\_block.yml  
│   │   └── openy\_demo\_bsimple.info.yml  
│   ├── openy\_demo\_menu/        \# Parent module for menu-related demo content  
│   │   ├── config/install/  
│   │   │   └── migrate\_plus.migration\_group.openy\_demo\_menu.yml  
│   │   ├── modules/  
│   │   │   ├── openy\_demo\_menu\_footer/ \# Demo content for Footer menu links  
│   │   │   │   ├── config/install/  
│   │   │   │   │   ├── migrate\_plus.migration.openy\_demo\_extended\_menu\_link\_footer.yml  
│   │   │   │   │   ├── migrate\_plus.migration.openy\_demo\_menu\_link\_footer.yml  
│   │   │   │   │   └── migrate\_plus.migration.openy\_demo\_standard\_menu\_link\_footer.yml  
│   │   │   │   └── openy\_demo\_menu\_footer.info.yml  
│   │   │   └── openy\_demo\_menu\_main/   \# Demo content for Main menu links  
│   │   │   │   ├── assets/icons/menu/main/ \# Demo icons for main menu  
│   │   │   │   ├── config/install/  
│   │   │   │   │   ├── migrate\_plus.migration.openy\_demo\_extended\_menu\_link\_main.yml  
│   │   │   │   │   ├── migrate\_plus.migration.openy\_demo\_menu\_link\_main.yml  
│   │   │   │   │   ├── migrate\_plus.migration.openy\_demo\_menu\_main\_file.yml  
│   │   │   │   │   └── migrate\_plus.migration.openy\_demo\_standard\_menu\_link\_main.yml  
│   │   │   │   └── src/Plugin/migrate/process/ \# Custom migrate process plugins  
│   │   │   │   │   └── OpenYMenuIcon.php  
│   │   │   │   └── openy\_demo\_menu\_main.info.yml  
│   │   └── openy\_demo\_menu.info.yml  
│   └── openy\_demo\_webform/     \# Demo Webform configuration  
│       ├── assets/yml/         \# YAML file for the demo webform structure  
│       │   └── demo\_form.yml  
│       ├── openy\_demo\_webform.features.yml \# Features configuration (if used)  
│       └── openy\_demo\_webform.info.yml  
└── composer.lock         \# (If present)
```
