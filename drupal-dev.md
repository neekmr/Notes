# DevSite

#### Composer.json
* ```
{
            "type": "package",
            "package": {
                "name": "uikit/uikit",
                "version": "3.1.4",
                "type": "drupal-library",
                "dist": {
                    "url": "https://github.com/uikit/uikit/archive/develop.zip",
                    "type": "zip"
                }
            }
        }
```

#### Info.yml
* ```
libraries:
  - 'xero/global-styling'
  - 'xero/global-scripts'  
  - 'xero/uikit'   
```


#### Libraries.yml
* ```
uikit:
  css:
    theme:
      /libraries/uikit/dist/css/uikit.min.css: {}
  js:
    /libraries/uikit/dist/js/uikit.min.js: {}
    /libraries/uikit/dist/js/uikit-icons.min.js: {}
```

#### Cmod
* ```
function cmod_entity_presave(Drupal\Core\Entity\EntityInterface $entity) {
  if ($entity->isNew()) {
    switch ($entity->bundle()) {
    // Here you modify only your tarticle content type
    case 'ct_002':
      // Setting the title with the publish date.
      $entity->setTitle(date('jS \of F \, Y'));
     break;
    }
  }
}
```