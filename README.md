
# Comment travailler avec plusieurs gestionnaires d’entités et connexions


#### Le code de configuration suivant montre comment configurer deux gestionnaires d’entités :
````yaml
# config/packages/doctrine.yaml
doctrine:
    dbal:
        default_connection: default
        connections:
            default:
                # configure these for your database server
                url: '%env(resolve:DATABASE_URL)%'
                driver: 'pdo_mysql'
                server_version: '5.7'
                charset: utf8mb4
            customer:
                # configure these for your database server
                url: '%env(resolve:DATABASE_CUSTOMER_URL)%'
                driver: 'pdo_mysql'
                server_version: '5.7'
                charset: utf8mb4
    orm:
        default_entity_manager: default
        entity_managers:
            default:
                connection: default
                mappings:
                    Main:
                        is_bundle: false
                        type: annotation
                        dir: '%kernel.project_dir%/src/Entity/Main'
                        prefix: 'App\Entity\Main'
                        alias: Main
            customer:
                connection: customer
                mappings:
                    Customer:
                        is_bundle: false
                        type: annotation
                        dir: '%kernel.project_dir%/src/Entity/Customer'
                        prefix: 'App\Entity\Customer'
                        alias: Customer
````
#### Lorsque vous travaillez avec plusieurs connexions pour créer vos bases de données :
````composer log
$ php bin/console doctrine:database:create
$ php bin/console doctrine:database:create --connection=custome
````
#### Lorsque vous travaillez avec plusieurs gestionnaires d’entités pour générer des migrations :
````composer log
$ php bin/console doctrine:migrations:diff
$ php bin/console doctrine:migrations:migrate


$ php bin/console doctrine:migrations:diff --em=customer
$ php bin/console doctrine:migrations:migrate --em=customer
````
---

#### Si vous omettez le nom du responsable de l’entité lorsque vous le demandez, Le gestionnaire d’entités par défaut (c’est-à-dire) est renvoyé :default
```php
// src/Controller/UserController.php
namespace App\Controller;

// ...
use Doctrine\ORM\EntityManagerInterface;
use Doctrine\Persistence\ManagerRegistry;

class UserController extends AbstractController
{
    public function index(ManagerRegistry $doctrine): Response
    {
        // Both methods return the default entity manager
        $entityManager = $doctrine->getManager();
        $entityManager = $doctrine->getManager('default');

        // This method returns instead the "customer" entity manager
        $customerEntityManager = $doctrine->getManager('customer');

        // ...
    }
}
```
#### Il en va de même pour les appels de référentiel :

````php
// src/Controller/UserController.php
namespace App\Controller;

use AcmeStoreBundle\Entity\Customer;
use AcmeStoreBundle\Entity\Product;
use Doctrine\Persistence\ManagerRegistry;
// ...

class UserController extends AbstractController
{
    public function index(ManagerRegistry $doctrine): Response
    {
        // Retrieves a repository managed by the "default" entity manager
        $products = $doctrine->getRepository(Product::class)->findAll();

        // Explicit way to deal with the "default" entity manager
        $products = $doctrine->getRepository(Product::class, 'default')->findAll();

        // Retrieves a repository managed by the "customer" entity manager
        $customers = $doctrine->getRepository(Customer::class, 'customer')->findAll();

        // ...
    }
}
````


### Connect with me:

[![website](./img/twitter-dark.svg)](https://twitter.com/surmurthag#gh-dark-mode-only)
&nbsp;&nbsp;
[![website](./img/instagram-dark.svg)](https://instagram.com/surmurthag#gh-dark-mode-only)

****
### Languages and Tools:

<img align="left" alt="HTML5" width="26px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" style="padding-right:10px;" />
<img align="left" alt="CSS3" width="26px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" style="padding-right:10px;" />

<img align="left" alt="JavaScript" width="26px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" style="padding-right:10px;" />

<img align="left" alt="React" width="26px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/react/react-original.svg" style="padding-right:10px;" />

<img align="left" alt="Node.js" width="26px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" style="padding-right:10px;" />

<img align="left" alt="MySQL" width="26px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/mysql/mysql-original.svg" style="padding-right:10px;" />
<img align="left" alt="Git" width="26px" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" style="padding-right:10px;" />

<br />
<br />

