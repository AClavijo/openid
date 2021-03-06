# Plugin Google OpenId Connect for Cake 1.3 applications

This plugin provide Google OpenId Connect to a cake 1.3 Framework.

> I try to make the installation easy. So please read the installation part :)

## Dependancies

This project include one dependancie:
- "google/apiclient": "^2.0" ['Documentation'](https://github.com/google/google-api-php-client)
- "symfony/yaml" :    "3.1.*" ['Documentation'](http://symfony.com/doc/current/components/yaml.html)
- "aclavijo/cakephp-installer-plugin": "0.1.*" Custom composer installer

## Installation

### via composer

```
composer require aclavijo/cakeopenidconnect
```

When you've done the installation, composer will place the plugin in 'app/plugins/openid'

### Parameter.yml

you need to create a parameter.yml in app/plugins/openid/config from the parameter.yml.dist

## Configuration

```
#parameters.yml
OpenidConnect:
    client_id: string
    secret_id: string
    scopes: array
    #optionnal
    domains: null|array #default null
    log_path: null|array #default null
    cake_log: false|true #default true
    flash_ctp: string #default flash_bas
```

- **client_id**: client_id provide by google
- **secret_id**: client_id provide by google
- **scopes**: google openid scope ['openid', 'email', 'profile'] minimum ['openid', 'email']
- **domains**: array if you want to only authorize authentification by email address domains (all other will be rejected)
- **log_path**: specifie another log file path (Apache for example)
- **cake_log**: false to desactivate cake logging
- **flash_ctp**: name of the cake 1.3 flash template to display error loggin message
## Deploy it on your project

### User search fonction

- In *app/models/uilisateur.php*, add the following function

```php
    function getUserByPseudo($pseudo)
    {
        $params = array(
            'conditions' => array('Utilisateur.pseudo' => $pseudo)
        );
        $result = $this->find('first', $params);
        
        return $result['Utilisateur'];
    }
```

### (optionnal) add the route to the router

- in the file *app/config/router.php*

```php
Router::connect('/openid/oauth/authentification', array('plugin' => 'openid', 'controller' => 'oauth', 'action' => 'authentification'));
```

### Add a connection link

- In *login.ctp*:

```php
 echo $html->link('Name your button', array(
        'plugin' => 'openid', 
        'controller' => 'oauth',
        'action' => 'authentification' 
    ), array(
        'class' => 'btn btn-primary'
    )); ?>
```

## To do

- Implement the log_path feature