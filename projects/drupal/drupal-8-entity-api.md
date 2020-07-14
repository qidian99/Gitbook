# Drupal 8 Entity API

{% embed url="https://drupalize.me/series/drupal-entity-api" %}

## Overview

### Definition

* Entities are the basic building blocks of Drupal's data model
* There are several types of entities included in **Drupal core** that make up both the configuration and content of a default installation
  * [**Configuration entities**](https://drupalize.me/tutorial/what-are-configuration-entities) are objects that allow us to **store information** \(and default values\) for _configurable settings_ on our site. 
    * Configuration entities can be exported via core's _configuration management system_. 
    * Configuration entities support translation but they cannot have user-configured fields attached to them. 
    * The data structure of a configuration entity is limited to what is provided by module code
    * Examples: image styles, user roles and displays in views. 
  * **Content entities** are configurable
    * support translation and revisions
    * allow additional fields to be attached for more complex data modeling.
    * included in core include nodes, taxonomy terms, blocks and users.
* Many entity variants come in pairs. 
  * The Block module
    * provides configuration entities to define custom block types
    * content entities to provide the actual content of custom blocks.

### Key terms

1. **Bundles**

   * used to describe containers for a sub-type of a particular entity
     * For example, nodes are a type of entity. The node entity has a bundle for each content type \(ie: article, page, blog post, etc\). 
     * Taxonomy is another entity type, and _each individual vocabulary is its own bundle_. 
   * Bundles provide an **organizational abstraction layer** that allows for **differences in field definitions and configurations** between entity sub-types. 
   * For a particular entity type \(i.e. node\) all bundles \(article, page, etc\) will have the same **base fields** \(title, author\) but will have different **bundle fields** \(articles have tags\)

2. **Fields**

   * Fields consist of individual \(or compound\) data elements that make up the details of the data model.
   * Drupal core provides [several different types of fields](https://www.drupal.org/docs/8/api/entity-api/fieldtypes-fieldwidgets-and-fieldformatters)
     * boolean, decimal, float, integer, entity reference, link, image, email, telephone, and several text fields. 
   * Fields are built on top of the actual data primitives in Drupal, [Typed Data](https://www.drupal.org/node/1795854). 
   * Fields can be added to content entities and field configuration will vary between bundles of the same entity type.

3. **Plugins**

   * provide developers an API to encapsulate re-useable behavior. 

4. **Annotations**

   * specially formatted code comments that are parsed to provide metadata information about particular PHP classes to Drupal.
   * The Entity API uses annotations to discover which classes to load for a particular entity type \(among other things\). 
   * See [annotations tutorial](https://drupalize.me/tutorial/annotations).

5. **Handlers**
   * the Drupal 8 equivalent of Drupal 7's controllers.
   * responsible for acting on and with entities. They help manage things like **storage, access control, building lists and views** of entities, and the **forms** required for creating, viewing, updating and deleting entities.

### Differences from Drupal 7's Entity API

#### Drupal 7's problem

* Drupal 7's version of the Entity API had to account for differences in accessing and working with entity properties \(things like the node title and published status\) and fields \(things like images, reference fields, etc\). 

#### Drupal 8's improvement

* This interface is unified in Drupal 8 because _**everything is a field**_. Developers interact with fields using the same techniques _regardless of whether they are base fields or bundle fields._ 
* Also the method used for querying entity information, the `EntityFieldQuery` class, had limited functionality in core in Drupal 7. The method for querying entities has been simplified in Drupal 8 yet is simultaneously more powerful through chaining.

#### Printing a field value

**...in Drupal 7**

```php
print $node->field_name[LANGUAGE_NONE][0]['value'];
```

**...in Drupal 8**

```php
print $node->field_name->value;
$author_name = $node->uid->entity->name->value;
```

#### Getting a field definition

**...in Drupal 7**

```php
$field = field_info_field($field_name);
$instance = field_info_instance($entity_type, $field_name, $bundle);
```

**... in Drupal 8**

```php
$field_definition = $entity->field_name->getFieldDefinition();
```

#### Saving a field value

**...in Drupal 7**

```php
$node->field_name[LANGUAGE_NONE][0]['value'] = 'Hello world';
$node->save();
```

**...in Drupal 8**

```php
$node->field_name->value = 'Hello world';
$node->save();
```

## [Entity API Implementation Basics](https://drupalize.me/tutorial/entity-api-implementation-basics)

### Configuration entities vs. content entities

* If the person that will be editing the particular piece of information is primarily an author or editor you're likely working with a content entity. 
* If the information would only be edited by someone in a site building or development role then it's likely a configuration entity. 

#### Configuration entities

* Custom block types
* Views
* Menu
* Role
* [What Are Configuration Entities?](https://drupalize.me/tutorial/what-are-configuration-entities) and [Create a Configuration Entity](https://drupalize.me/tutorial/create-configuration-entity) tutorials.

#### Content entities

* Node
* User
* Taxonomy
* Blocks

### Entity types in Drupal core

![](../../.gitbook/assets/image%20%2852%29.png)

From here we can see that configuration entities extend `\Drupal\Core\Config\Entity\ConfigEntityBase` while content entities extend `\Drupal\Core\Entity\ContentEntityBase`. Both of these base classes extend `Drupal\Core\Entity\Entity` and `\Drupal\Core\Entity\EntityInterface`. Comparing the interfaces of configuration and content entities helps give more insight into their differences.

#### ConfigEntityInterface methods

* `enable()`
* `disable()`
* `setStatus()`
* `setSyncing()`
* `status()`
* `isSyncing()`
* `isUninstalling()`
* `get()`
* `set()`
* `calculateDependencies()`
* `onDependencyRemoval()`
* `getDependencies()`
* `isInstallable()`
* `trustData()`
* `hasTrustedData()`

#### ContentEntityInterface methods \(some via FieldableEntityInterface\)

* `baseFieldDefinitions()`
* `bundleFieldDefinitions()`
* `hasField()`
* `getFieldDefinition()`
* `get()`
* `set()`
* `getFields()`
* `getTranslateableFields()`
* `onChange()`
* `validate()`
* `isValidationRequired()`
* `setValidationRequired()`
* `hasTranslationChanges()`
* `setRevisionTranslationAffected()`
* `isRevisionTranslationAffected()`

Configuration entities' main functionality has to do with information storage and synchronization. 

The main functionality built into content entities has to do with fields, translations, validation and revisions. 

It's worth diving deeply and reading the source code of both the `ConfigEntityBase` and `ContentEntityBase` classes to get a sense of the functionality, similarities and differences between them.

### Content entity basics

Unlike configuration entities, content entities are fieldable. Content entities can have fields that are shared among all entities of a given type. These are called base fields. Fields that are unique among the sub-type \(or bundle\) are called bundle fields. 

The node entity type is defined by the `Node` class in `/core/modules/node/src/Entity/Node.php`. The base fields for nodes are defined in the `Node::baseFieldDefinitions` method.

```php
public static function baseFieldDefinitions(EntityTypeInterface $entity_type) {
    $fields = parent::baseFieldDefinitions($entity_type);

    $fields['title'] = BaseFieldDefinition::create('string')
      ->setLabel(t('Title'))
      ->setRequired(TRUE)
      ->setTranslatable(TRUE)
      ->setRevisionable(TRUE)
      ->setSetting('max_length', 255)
      ->setDisplayOptions('view', array(
        'label' => 'hidden',
        'type' => 'string',
        'weight' => -5,
      ))
      ->setDisplayOptions('form', array(
        'type' => 'string_textfield',
        'weight' => -5,
      ))
      ->setDisplayConfigurable('form', TRUE);

    $fields['uid'] = BaseFieldDefinition::create('entity_reference')
      ->setLabel(t('Authored by'))
      ->setDescription(t('The username of the content author.'))
      ->setRevisionable(TRUE)
      ->setSetting('target_type', 'user')
      ->setDefaultValueCallback('Drupal\node\Entity\Node::getCurrentUserId')
      ->setTranslatable(TRUE)
      ->setDisplayOptions('view', array(
        'label' => 'hidden',
        'type' => 'author',
        'weight' => 0,
      ))
      ->setDisplayOptions('form', array(
        'type' => 'entity_reference_autocomplete',
        'weight' => 5,
        'settings' => array(
          'match_operator' => 'CONTAINS',
          'size' => '60',
          'placeholder' => '',
        ),
      ))
      ->setDisplayConfigurable('form', TRUE);

    $fields['status'] = BaseFieldDefinition::create('boolean')
      ->setLabel(t('Publishing status'))
      ->setDescription(t('A boolean indicating whether the node is published.'))
      ->setRevisionable(TRUE)
      ->setTranslatable(TRUE)
      ->setDefaultValue(TRUE);

    $fields['created'] = BaseFieldDefinition::create('created')
      ->setLabel(t('Authored on'))
      ->setDescription(t('The time that the node was created.'))
      ->setRevisionable(TRUE)
      ->setTranslatable(TRUE)
      ->setDisplayOptions('view', array(
        'label' => 'hidden',
        'type' => 'timestamp',
        'weight' => 0,
      ))
      ->setDisplayOptions('form', array(
        'type' => 'datetime_timestamp',
        'weight' => 10,
      ))
      ->setDisplayConfigurable('form', TRUE);

    $fields['changed'] = BaseFieldDefinition::create('changed')
      ->setLabel(t('Changed'))
      ->setDescription(t('The time that the node was last edited.'))
      ->setRevisionable(TRUE)
      ->setTranslatable(TRUE);

    $fields['promote'] = BaseFieldDefinition::create('boolean')
      ->setLabel(t('Promoted to front page'))
      ->setRevisionable(TRUE)
      ->setTranslatable(TRUE)
      ->setDefaultValue(TRUE)
      ->setDisplayOptions('form', array(
        'type' => 'boolean_checkbox',
        'settings' => array(
          'display_label' => TRUE,
        ),
        'weight' => 15,
      ))
      ->setDisplayConfigurable('form', TRUE);

...

    return $fields;
  }
```

From this method we can see the base fields common to all node entities regardless of bundle \(a.k.a. "node type"\). Every node has the following base fields:

* title
* author ID
* status
* created timestamp
* changed timestamp
* promoted to front page
* sticky
* revision timestamp
* revision author
* revision log message
* revision translation affected

The "Standard" installation of Drupal 8 core includes two node types: Article and Basic Page. Every node, regardless of type, has values for these base fields defined by the `Node` class. The fields that make each node type \(or bundle\) _unique_ are the bundle fields. 

* Out of the box, the Basic Page node type has only one bundle field: **the body field**. You can see a list of bundle fields for a node type by navigating to `Structure > Content types > Page`\(_admin/structure/types/manage/page/fields_\). 
* In contrast, the Article node type has several additional bundle fields: **body, comments, image and tags**. 
  * Even though both Article and Basic Page both have a "body" field, it is not a "shared" field and so it's considered a bundle field.

![Article bundle fields](https://drupalize.me/sites/default/files/tutorials/article_bundle_fields.png)

A full list of the bundle fields available from Drupal core can be [found on drupal.org](https://www.drupal.org/docs/8/api/entity-api/fieldtypes-fieldwidgets-and-fieldformatters)

### EntityType annotations

As we've just seen, every entity type \(configuration or content\) is defined by a specifically typed object \(backed by a class with an interface\). **The class that defines an entity type must have an `EntityType` annotation in order for Drupal to find it.** If you're unfamiliar with annotations, it would be a good idea to familiarize yourself with our [Annotations tutorial](https://drupalize.me/tutorial/annotations?p=2766) before continuing.

Let's continue to look at the core code for the Node entity type as an example. The `EntityType` annotation, required for core to locate our entity type can be found in the `Node` class \(`/core/modules/node/src/Entity/Node.php`\):

```php
/**
 * Defines the node entity type.
 *
 * @ContentEntityType(
 *   id = "node",
 *   label = @Translation("Content"),
 *   label_singular = @Translation("content item"),
 *   label_plural = @Translation("content items"),
 *   label_count = @PluralTranslation(
 *     singular = "@count content item",
 *     plural = "@count content items"
 *   ),
 *   bundle_label = @Translation("Content type"),
 *   handlers = {
 *     "storage" = "Drupal\node\NodeStorage",
 *     "storage_schema" = "Drupal\node\NodeStorageSchema",
 *     "view_builder" = "Drupal\node\NodeViewBuilder",
 *     "access" = "Drupal\node\NodeAccessControlHandler",
 *     "views_data" = "Drupal\node\NodeViewsData",
 *     "form" = {
 *       "default" = "Drupal\node\NodeForm",
 *       "delete" = "Drupal\node\Form\NodeDeleteForm",
 *       "edit" = "Drupal\node\NodeForm"
 *     },
 *     "route_provider" = {
 *       "html" = "Drupal\node\Entity\NodeRouteProvider",
 *     },
 *     "list_builder" = "Drupal\node\NodeListBuilder",
 *     "translation" = "Drupal\node\NodeTranslationHandler"
 *   },
 *   base_table = "node",
 *   data_table = "node_field_data",
 *   revision_table = "node_revision",
 *   revision_data_table = "node_field_revision",
 *   translatable = TRUE,
 *   list_cache_contexts = { "user.node_grants:view" },
 *   entity_keys = {
 *     "id" = "nid",
 *     "revision" = "vid",
 *     "bundle" = "type",
 *     "label" = "title",
 *     "langcode" = "langcode",
 *     "uuid" = "uuid",
 *     "status" = "status",
 *     "uid" = "uid",
 *   },
 *   bundle_entity_type = "node_type",
 *   field_ui_base_route = "entity.node_type.edit_form",
 *   common_reference_target = TRUE,
 *   permission_granularity = "bundle",
 *   links = {
 *     "canonical" = "/node/{node}",
 *     "delete-form" = "/node/{node}/delete",
 *     "edit-form" = "/node/{node}/edit",
 *     "version-history" = "/node/{node}/revisions",
 *     "revision" = "/node/{node}/revisions/{node_revision}/view",
 *   }
 * )
 */
```

Even without understanding all of the details, reading this annotation you can see that it is mapping handlers responsible for storage and access control to particular classes, specifying classes responsible for rendering node forms, and providing the table names and keys used to store node entities. **This metadata helps define some basic entity behavior** \(what url is rendered for particular links\), **information about the database schema** \(table names and keys\), **labels** \(including pluralization\), and **which classes to use for specific handlers** \(or [plugins](https://drupalize.me/tutorial/what-are-plugins?p=2766)\). The full documentation for available metadata properties for `EntityType` annotations is [available on drupal.org here](https://api.drupal.org/api/drupal/core!lib!Drupal!Core!Entity!EntityType.php/class/EntityType/).

## [Create a Custom Content Entity](https://drupalize.me/tutorial/create-custom-content-entity)

### Steps

#### Set up the custom module

The first thing we need in place to create a new content entity is a module that can encapsulate the entity code. If you're familiar with creating a module in Drupal 7 the process hasn't changed all that much. In order to create a module we'll need to create a directory and two files. In this tutorial we're going to use the content entity example from the [Examples module](https://www.drupal.org/project/examples). We'll create a content entity type called "Contact" with a basic administrative interface for creating, updating and listing our entities. The [already completed code is available here](https://git.drupalcode.org/project/examples/-/tree/9.x-1.x/content_entity_example) if you'd like to follow along with the answers. The module that's going to hold our new content entity is going to be called the "Content Entity Example Module". Create a directory to hold the code for this new module _/modules/content\_entity\_example_.

#### Create the module file

In this case our actual module file won't have to do too much, but we still need the stub present in order for Drupal to recognize it.

Create the file `/modules/content_entity_example/content_entity_example.module` with:

```php
<?php

/**
 * @file
 * Contains Drupal\content_entity_example\content_entity_example.module.
 */
```

You may notice the [Examples module code](https://git.drupalcode.org/project/examples/-/blob/9.x-1.x/content_entity_example/content_entity_example.module) has more thorough code comments, but for our purposes in this tutorial, simplified comments will suffice.

#### Create the info file

Next we need an info file. Drupal scans for these YAML-formatted files to identify modules. Create the file _/modules/content\_entity\_example/content\_entity\_example.info.yml_:

```yaml
name: Content Entity Example
type: module
description: Demonstrates how to create a content entity.
package: Example modules
core: 8.x
# These modules are required by the tests, must be available at bootstrap time
dependencies:
  - options
  - examples
```

This file contains some basic information about the module such as the human- and machine-readable name of the module, which version of core it supports, and any dependencies.

#### Define basic permissions

Static permissions in Drupal 8 are defined in another YAML file. Add some basic "create", "read", "update", and "delete" permissions by creating the _/modules/content\_entity\_example/content\_entity\_example.permissions.yml_ file:

```yaml
'delete contact entity':
  title: Delete entity content.
'add contact entity':
  title: Add entity content
'view contact entity':
  title: View entity content
'edit contact entity':
  title: Edit entity content
'administer contact entity':
  title: Administer settings
```

#### Define links

Modules can provide various types of links via YAML files. We're going to add three different types to our contact entity module: menu links, action links and task links.

![Contact entity link types](https://drupalize.me/sites/default/files/tutorials/content_entity_example_links.png)

A module [can provide menu links](https://www.drupal.org/docs/8/api/menu-api/providing-module-defined-menu-links) via yet another YAML file. This file provides the title used in the menu, the route name Drupal needs to call to render the page, as well as a description and weight.

Create the file _/modules/content\_entity\_example/content\_entity\_example.links.menu.yml_:

```yaml
# Define the menu links for this module

entity.content_entity_example_contact.collection:
  title: 'Content Entity Example: Contacts Listing'
  route_name: entity.content_entity_example_contact.collection
  description: 'List Contacts'
  weight: 10
content_entity_example_contact.admin.structure.settings:
  title: Contact Settings
  description: 'Configure Contact entity'
  route_name:  content_entity_example.contact_settings
  parent: system.admin_structure

```

The tabs, or local tasks, that show up when viewing our contacts include the "view", "edit" and "delete" operations. These links are created in the _/modules/content\_entity\_example.links.task.yml_ file:

```text
# Define the 'local' links for the module

contact.settings_tab:
  route_name: content_entity_example.contact_settings
  title: Settings
  base_route: content_entity_example.contact_settings

contact.view:
  route_name: entity.content_entity_example_contact.canonical
  base_route: entity.content_entity_example_contact.canonical
  title: View

contact.page_edit:
  route_name: entity.content_entity_example_contact.edit_form
  base_route: entity.content_entity_example_contact.canonical
  title: Edit

contact.delete_confirm:
  route_name:  entity.content_entity_example_contact.delete_form
  base_route:  entity.content_entity_example_contact.canonical
  title: Delete
  weight: 10
```

Lastly, we can create the action link \(to add a new Contact\) in _/modules/content\_entity\_example/content\_entity\_example.links.action.yml_:

```text
# All action links for this module

content_entity_example.contact_add:
  # Which route will be called by the link
  route_name: content_entity_example.contact_add
  title: 'Add Contact'

  # Where will the link appear, defined by route name.
  appears_on:
    - entity.content_entity_example_contact.collection
    - entity.content_entity_example_contact.canonical
```

#### Routing

In order to map the URLs an end-user will visit to the code responsible for generating the output for those pages we need to define a series of routes. We define routes via another YAML file that maps the route name we've seen used in links to the actual path. The full route specification will also include the view or listing controllers needed to generate the route, a title for the page, and any access restriction or permissions checks.

We will set up routes to handle viewing a single or list of contacts, and forms to add, edit, delete and modify their settings. Create the _/modules/content\_entity\_example/content\_entity\_example.routing.yml_ file:

```text
# This file brings everything together. Very nifty!

# Route name can be used in several places; e.g. links, redirects, and local
# actions.
entity.content_entity_example_contact.canonical:
  path: '/content_entity_example_contact/{content_entity_example_contact}'
  defaults:
  # Calls the view controller, defined in the annotation of the contact entity
    _entity_view: 'content_entity_example_contact'
    _title: 'Contact Content'
  requirements:
  # Calls the access controller of the entity, $operation 'view'
    _entity_access: 'content_entity_example_contact.view'

entity.content_entity_example_contact.collection:
  path: '/content_entity_example_contact/list'
  defaults:
  # Calls the list controller, defined in the annotation of the contact entity.
    _entity_list: 'content_entity_example_contact'
    _title: 'Contact List'
  requirements:
  # Checks for permission directly.
    _permission: 'view contact entity'

content_entity_example.contact_add:
  path: '/content_entity_example_contact/add'
  defaults:
  # Calls the form.add controller, defined in the contact entity.
    _entity_form: content_entity_example_contact.add
    _title: 'Add Contact'
  requirements:
    _entity_create_access: 'content_entity_example_contact'

entity.content_entity_example_contact.edit_form:
  path: '/content_entity_example_contact/{content_entity_example_contact}/edit'
  defaults:
  # Calls the form.edit controller, defined in the contact entity.
    _entity_form: content_entity_example_contact.edit
    _title: 'Edit Contact'
  requirements:
    _entity_access: 'content_entity_example_contact.edit'

entity.content_entity_example_contact.delete_form:
  path: '/contact/{content_entity_example_contact}/delete'
  defaults:
    # Calls the form.delete controller, defined in the contact entity.
    _entity_form: content_entity_example_contact.delete
    _title: 'Delete Contact'
  requirements:
    _entity_access: 'content_entity_example_contact.delete'

content_entity_example.contact_settings:
  path: 'admin/structure/content_entity_example_contact_settings'
  defaults:
    _form: '\Drupal\content_entity_example\Form\ContactSettingsForm'
    _title: 'Contact Settings'
  requirements:
    _permission: 'administer contact entity'

```

With that scaffolding out of the way, we're finally ready to actually create our custom contact entity.

### Creating a new entity type

In order for Drupal to recognize our new entity type, we need to register our entity via a `ContentEntityType` annotation. If you're not already familiar with this concept, you can find [our tutorial on annotations here](https://drupalize.me/tutorial/annotations). We're going to add an annotation to the class that will encapsulate the behavior of our contact entity. First, we'll create a new file for this class: _/modules/contact\_entity\_example/src/Entity/Contact.php_:

```text
<?php

namespace Drupal\content_entity_example\Entity;

use Drupal\Core\Entity\EntityStorageInterface;
use Drupal\Core\Field\BaseFieldDefinition;
use Drupal\Core\Entity\ContentEntityBase;
use Drupal\Core\Entity\EntityTypeInterface;
use Drupal\content_entity_example\ContactInterface;
use Drupal\user\UserInterface;
use Drupal\Core\Entity\EntityChangedTrait;

/**
 * Defines the ContentEntityExample entity.
 *
 * @ingroup content_entity_example
 *
 * [...]
 * 
 *  The following annotation is the actual definition of the entity type which
 *  is read and cached. Don't forget to clear cache after changes.
 * 
 * @ContentEntityType(
 *   id = "content_entity_example_contact",
 *   label = @Translation("Contact entity"),
 *   handlers = {
 *     "view_builder" = "Drupal\Core\Entity\EntityViewBuilder",
 *     "list_builder" = "Drupal\content_entity_example\Entity\Controller\ContactListBuilder",
 *     "form" = {
 *       "add" = "Drupal\content_entity_example\Form\ContactForm",
 *       "edit" = "Drupal\content_entity_example\Form\ContactForm",
 *       "delete" = "Drupal\content_entity_example\Form\ContactDeleteForm",
 *     },
 *     "access" = "Drupal\content_entity_example\ContactAccessControlHandler",
 *   },
 *   list_cache_contexts = { "user" },
 *   base_table = "contact",
 *   admin_permission = "administer content_entity_example entity",
 *   entity_keys = {
 *     "id" = "id",
 *     "label" = "name",
 *     "uuid" = "uuid"
 *   },
 *   links = {
 *     "canonical" = "/content_entity_example_contact/{content_entity_example_contact}",
 *     "edit-form" = "/content_entity_example_contact/{content_entity_example_contact}/edit",
 *     "delete-form" = "/contact/{content_entity_example_contact}/delete",
 *     "collection" = "/content_entity_example_contact/list"
 *   },
 *   field_ui_base_route = "content_entity_example.contact_settings",
 * )
 */
 class Contact extends ContentEntityBase implements ContactInterface {
    // ...
 }
```

Drupal uses the metadata provided by this `ContentEntityType` annotation to define some of the behavior of our contact entities:

* The `id` and `label` values provide the unique identifier and human-readable name of our entity type.
* `handlers` define the classes used for various operations on our contacts, like building lists, or generating an edit form. Notice that we're using a view builder class included in core but custom classes for the other values.
* The `base_table` defines the table name used to store data about these entities. This table is automatically created when our module is installed.
* The `entity_keys` property tells Drupal how to access fields for our entity. Links provide the paths to perform standard tasks. The full documentation of properties that can be used in an entity type definition can be seen in the class definition of [`\Drupal\Core\Entity\EntityType`](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Entity%21EntityType.php/class/EntityType/8.2.x).

Notice that our `Contact` class extends core's `ContentEntityBase` class. It's worth taking the time to look at [the source code for this class](https://api.drupal.org/api/drupal/core%21lib%21Drupal%21Core%21Entity%21ContentEntityBase.php/class/ContentEntityBase/8.2.x) to get a sense of the properties and methods available to our implementation. Also notice that our `Contact` class is implementing an interface `ContactInterface`. We'll need to create this, too.

Create the file _/modules/content\_entity\_example/src/ContactInterface.php_.

```text
<?php

namespace Drupal\content_entity_example;

use Drupal\Core\Entity\ContentEntityInterface;
use Drupal\user\EntityOwnerInterface;
use Drupal\Core\Entity\EntityChangedInterface;

/**
 * Provides an interface defining a Contact entity.
 *
 * We have this interface so we can join the other interfaces it extends.
 *
 * @ingroup content_entity_example
 */
interface ContactInterface extends ContentEntityInterface, EntityOwnerInterface, EntityChangedInterface {

}
```

Right now our interface itself isn't very sophisticated. It extends a few core interfaces but provides no new custom functionality. Generally speaking it's considered a best practice for your entity to implement an interface. This allows other module developers to better understand the behavior of your custom entity in case they want to override or extend it.

There's one more important method to add to our `Contact` class before we install our module: the `baseFieldDefinitions` method. You should already be familiar with the concept of base fields from [the Entity API basics](https://drupalize.me/tutorial/entity-api-implementation-basics) tutorial. Here's what the implementation looks like in our `Contact` class:

```text
/**
   * {@inheritdoc}
   *
   * Define the field properties here.
   *
   * Field name, type and size determine the table structure.
   *
   * In addition, we can define how the field and its content can be manipulated
   * in the GUI. The behaviour of the widgets used can be determined here.
   */
  public static function baseFieldDefinitions(EntityTypeInterface $entity_type) {

    // Standard field, used as unique if primary index.
    $fields['id'] = BaseFieldDefinition::create('integer')
      ->setLabel(t('ID'))
      ->setDescription(t('The ID of the Contact entity.'))
      ->setReadOnly(TRUE);

    // Standard field, unique outside of the scope of the current project.
    $fields['uuid'] = BaseFieldDefinition::create('uuid')
      ->setLabel(t('UUID'))
      ->setDescription(t('The UUID of the Contact entity.'))
      ->setReadOnly(TRUE);

    // Name field for the contact.
    // We set display options for the view as well as the form.
    // Users with correct privileges can change the view and edit configuration.
    $fields['name'] = BaseFieldDefinition::create('string')
      ->setLabel(t('Name'))
      ->setDescription(t('The name of the Contact entity.'))
      ->setSettings(array(
        'default_value' => '',
        'max_length' => 255,
        'text_processing' => 0,
      ))
      ->setDisplayOptions('view', array(
        'label' => 'above',
        'type' => 'string',
        'weight' => -6,
      ))
      ->setDisplayOptions('form', array(
        'type' => 'string_textfield',
        'weight' => -6,
      ))
      ->setDisplayConfigurable('form', TRUE)
      ->setDisplayConfigurable('view', TRUE);

    $fields['first_name'] = BaseFieldDefinition::create('string')
      ->setLabel(t('First Name'))
      ->setDescription(t('The first name of the Contact entity.'))
      ->setSettings(array(
        'default_value' => '',
        'max_length' => 255,
        'text_processing' => 0,
      ))
      ->setDisplayOptions('view', array(
        'label' => 'above',
        'type' => 'string',
        'weight' => -5,
      ))
      ->setDisplayOptions('form', array(
        'type' => 'string_textfield',
        'weight' => -5,
      ))
      ->setDisplayConfigurable('form', TRUE)
      ->setDisplayConfigurable('view', TRUE);

    // Gender field for the contact.
    // ListTextType with a drop down menu widget.
    // The values shown in the menu are 'male' and 'female'.
    // In the view the field content is shown as string.
    // In the form the choices are presented as options list.
    $fields['gender'] = BaseFieldDefinition::create('list_string')
      ->setLabel(t('Gender'))
      ->setDescription(t('The gender of the Contact entity.'))
      ->setSettings(array(
        'allowed_values' => array(
          'female' => 'female',
          'male' => 'male',
        ),
      ))
      ->setDisplayOptions('view', array(
        'label' => 'above',
        'type' => 'string',
        'weight' => -4,
      ))
      ->setDisplayOptions('form', array(
        'type' => 'options_select',
        'weight' => -4,
      ))
      ->setDisplayConfigurable('form', TRUE)
      ->setDisplayConfigurable('view', TRUE);

    // Owner field of the contact.
    // Entity reference field, holds the reference to the user object.
    // The view shows the user name field of the user.
    // The form presents a auto complete field for the user name.
    $fields['user_id'] = BaseFieldDefinition::create('entity_reference')
      ->setLabel(t('User Name'))
      ->setDescription(t('The Name of the associated user.'))
      ->setSetting('target_type', 'user')
      ->setSetting('handler', 'default')
      ->setDisplayOptions('view', array(
        'label' => 'above',
        'type' => 'author',
        'weight' => -3,
      ))
      ->setDisplayOptions('form', array(
        'type' => 'entity_reference_autocomplete',
        'settings' => array(
          'match_operator' => 'CONTAINS',
          'size' => 60,
          'placeholder' => '',
        ),
        'weight' => -3,
      ))
      ->setDisplayConfigurable('form', TRUE)
      ->setDisplayConfigurable('view', TRUE);

    $fields['langcode'] = BaseFieldDefinition::create('language')
      ->setLabel(t('Language code'))
      ->setDescription(t('The language code of ContentEntityExample entity.'));
    $fields['created'] = BaseFieldDefinition::create('created')
      ->setLabel(t('Created'))
      ->setDescription(t('The time that the entity was created.'));

    $fields['changed'] = BaseFieldDefinition::create('changed')
      ->setLabel(t('Changed'))
      ->setDescription(t('The time that the entity was last edited.'));

    return $fields;
  }
```

Now, we finally have the main pieces in place for our module. It's worth noting that we can't actually install our module yet without experiencing some fatal errors. We're not quite finished with our `Contact` class. It's important to add getter and setter methods to our entity, especially for any base fields that should be accessbile to other modules. Take a look at [the completed code](https://git.drupalcode.org/project/examples/-/blob/9.x-1.x/content_entity_example/src/Entity/Contact.php) for a hint, but our `Contact` entity should have convenient methods to retrieve the created and changed timestamps, and the ability to get or set the ID of the owner.

For brevity's sake, right now we're also excluding a walk-through of the classes used to generate our entity forms. You can see the classes responsible for those forms in _/modules/content\_entity\_example/src/Form_ in [the completed code](https://git.drupalcode.org/project/examples/-/tree/9.x-1.x/content_entity_example).

Here is the listing view of our Contact entities in action:

![Contact entity listing](https://drupalize.me/sites/default/files/tutorials/contact_entity_listing_example.png)

### Creating custom entities with Drupal Console

Having created our Contact entity by hand, let's take a look at how we can use [Drupal Console](https://drupalize.me/tutorial/drupal-console) to speed up that process. Drupal Console provides a `generate` command that will create the files we need to start building our custom entity in two commands.

First, we need to create the module that will contain our entity code:

`drupal generate:module`

When running this command, Drupal Console will provide prompts asking us to fill out the name of the module and description along with a handful of other straightforward questions. After we've finished responding to the prompts, an _MODULE.info.yml_ and _MODULE.module_ file will be automatically generated.

![Drupal console module generation](https://drupalize.me/sites/default/files/tutorials/console_module_generate.gif)

Next we're ready to create our custom entity, and can do so by running another Drupal Console command:

```text
drupal generate:entity:content
```

This command will prompt us for the name of our entity, the module it belongs to, and a few other questions about the entities behavior. Drupal Console then generates the skeleton of the files needed to create our entity. Here are the files it generated in creating a contact entity:

```text
# Files generated by drupal generate:entity:content
 1 - modules/custom/contact_entity/contact_entity.permissions.yml
 2 - modules/custom/contact_entity/contact_entity.links.menu.yml
 3 - modules/custom/contact_entity/contact_entity.links.task.yml
 4 - modules/custom/contact_entity/contact_entity.links.action.yml
 5 - modules/custom/contact_entity/src/ConsoleContactEntityAccessControlHandler.php
 6 - modules/custom/contact_entity/src/ConsoleContactEntityTranslationHandler.php
 7 - modules/custom/contact_entity/src/Entity/ConsoleContactEntityInterface.php
 8 - modules/custom/contact_entity/src/Entity/ConsoleContactEntity.php
 9 - modules/custom/contact_entity/src/ConsoleContactEntityHtmlRouteProvider.php
 10 - modules/custom/contact_entity/src/Entity/ConsoleContactEntityViewsData.php
 11 - modules/custom/contact_entity/src/ConsoleContactEntityListBuilder.php
 12 - modules/custom/contact_entity/src/Form/ConsoleContactEntitySettingsForm.php
 13 - modules/custom/contact_entity/src/Form/ConsoleContactEntityForm.php
 14 - modules/custom/contact_entity/src/Form/ConsoleContactEntityDeleteForm.php
 15 - modules/custom/contact_entity/console_contact_entity.page.inc
 16 - modules/custom/contact_entity/templates/console_contact_entity.html.twig
 17 - modules/custom/contact_entity/src/Form/ConsoleContactEntityRevisionDeleteForm.php
 18 - modules/custom/contact_entity/src/Form/ConsoleContactEntityRevisionRevertTranslationForm.php
 19 - modules/custom/contact_entity/src/Form/ConsoleContactEntityRevisionRevertForm.php
 20 - modules/custom/contact_entity/src/ConsoleContactEntityStorage.php
 21 - modules/custom/contact_entity/src/ConsoleContactEntityStorageInterface.php
 22 - modules/custom/contact_entity/src/Controller/ConsoleContactEntityController.php
```

Looking through these files, you'll see just how much boilerplate skeleton code Drupal Console has saved us from writing. Running these two commands is certainly the fastest way for a developer to create new entity types.

![Drupal console content entity generation](https://drupalize.me/sites/default/files/tutorials/console_entity_generate.gif)



