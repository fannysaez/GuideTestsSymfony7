# Test avec Symfony

Créer et faire des tests dans Symfony.

---

## Pré-requis

- PHP >= 8.1
- Symfony CLI
- Composer
- MySQL ou MariaDB
- Node.js + Yarn
- PHPUnit
- Extension PHP `pdo_mysql`

---

## Environnement de test

Symfony permet d’exécuter des commandes et des tests dans un environnement spécifique (`test`) défini par le fichier `.env.test`.

Les fixtures permette de créer des fausses données voir `dwwm_sjsr_pe5_2023` grâce au fichier `fixtures.yaml`.

### Configuration

Dans `.env.test`, définir l’URL de la base de données de test :

```bash
DATABASE_URL="mysql://root:@127.0.0.1:3306/db_steamish_test"
```
---

### Création de la base et des migrations (environnement de test)

La base de données de test est définie dans le `.env.test` comme pour un `.env` ajouter le `DATABASE_URL`.

Pour créer la base de données de test il y à la possibilité d'ajouter des paramètres au commande Symfony. Ont créer l'environnement de test.

```bash
php bin/console doctrine:database:create --env=test
php bin/console doctrine:migrations:migrate --env=test
```

ou

```bash
symfony console d:d:c --env=test
symfony console d:m:m --env=test

```
---

### Chargement des fixtures (fausses données en test)

```bash
php bin/console hautelook:fixtures:load --purge-with-truncate -n --env=test
```

---

## Environnement de développement

### Création de la base et application des migrations

```bash
php bin/console doctrine:database:create
php bin/console doctrine:migrations:migrate
```

ou

```bash
symfony console d:d:c
symfony console d:m:m
```
---

### Exécution des fixtures (fausses données)

```bash
php bin/console hautelook:fixtures:load --purge-with-truncate -n
```

---

## Frontend (si Webpack Encore est utilisé)

```bash
npm install --global yarn
yarn install
yarn build
```

---

## Créer une classe de test

Symfony propose une commande pour générer automatiquement une classe de test :

```bash
symfony console make:test
```
---

### Étapes :

- **Type de test** : choisir `WebTestCase` pour un test fonctionnel.
- **Nom du fichier** : par exemple `Controller\Front\HomeController`.

Cela crée un fichier de test dans `tests/Controller/Front/HomeControllerTest.php` correspondant à l’arborescence du code source (`src/Controller/Front`).

---

## Exemple de test fonctionnel

```php
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;

class HomeControllerTest extends WebTestCase
{
    public function testSomething(): void
    {
        $client = static::createClient();
        $crawler = $client->request('GET', '/');

        $this->assertResponseIsSuccessful();
        $this->assertSelectorTextContains('h1', 'Instant-Faking');
    }
}
```

### Explication :

- `createClient()` : simule un navigateur
- `request('GET', '/')` : effectue une requête HTTP vers la page d’accueil
- `assertResponseIsSuccessful()` : vérifie que le code HTTP est 200
- `assertSelectorTextContains()` : vérifie la présence d’un élément HTML contenant un texte donné

---

## Exemple avec Crawler

Test d’un nombre précis d’éléments HTML :

```php
$crawler = $crawlerGlobal->filter('h2');
$this->assertCount(4, $crawler, "One title is missing");
```

Ici, on s’attend à trouver exactement 4 balises `<h2>`, sinon le message d’erreur est affiché.

---

## Lancer les tests

### Lancer un test spécifique

```bash
php bin/phpunit tests/Controller/Front/HomeControllerTest.php
```

### Lancer tous les tests

```bash
php bin/phpunit
```

> ✅ Pense à renommer `phpunit.xml.dist` en `phpunit.xml` pour activer la configuration locale.

---

## Git et tests

### Créer une branche

```bash
git checkout -b new
```

### Ajouter les changements

```bash
git add .
```

### Faire un commit

```bash
git commit -m "saves"
```

### Vérifier l'état

```bash
git status
```

### Revenir sur la branche principale

```bash
git checkout main
```

### Récupérer les dernières modifications

```bash
git pull
```

---

## Requête SQL : Réinitialisation des mots de passe

Changer tous les mots de passe utilisateurs pour la valeur `12345` (bcrypt) :

```sql
UPDATE user
SET password = '$2y$13$lB3ddnHoChlOGiOoP1kXL.jPCnRZWFRrGIEdt27DBeD.MmuYIunAK'
WHERE 1;
```

---

## Récapitulatif des commandes utiles

| Action                           | Commande                                                                      |
| -------------------------------- | ----------------------------------------------------------------------------- |
| Créer la BDD                     | `php bin/console doctrine:database:create`                                    |
| Appliquer les migrations         | `php bin/console doctrine:migrations:migrate`                                 |
| Charger les fixtures             | `php bin/console hautelook:fixtures:load --purge-with-truncate -n`            |
| Créer la BDD de test             | `php bin/console doctrine:database:create --env=test`                         |
| Appliquer les migrations en test | `php bin/console doctrine:migrations:migrate --env=test`                      |
| Charger les fixtures en test     | `php bin/console hautelook:fixtures:load --purge-with-truncate -n --env=test` |
| Créer un test                    | `symfony console make:test`                                                   |
| Lancer un test                   | `php bin/phpunit` ou `php bin/phpunit tests/...`                              |
| Compiler les assets front (yarn) | `yarn install && yarn build`                                                  |
