# L1 - Cloudflare bindings for Laravel

![CI](https://github.com/renoki-co/l1/workflows/CI/badge.svg?branch=master)
[![codecov](https://codecov.io/gh/renoki-co/l1/branch/master/graph/badge.svg)](https://codecov.io/gh/renoki-co/l1/branch/master)
[![StyleCI](https://github.styleci.io/repos/651202208/shield?branch=master)](https://github.styleci.io/repos/651202208)
[![Latest Stable Version](https://poser.pugx.org/renoki-co/l1/v/stable)](https://packagist.org/packages/renoki-co/l1)
[![Total Downloads](https://poser.pugx.org/renoki-co/l1/downloads)](https://packagist.org/packages/renoki-co/l1)
[![Monthly Downloads](https://poser.pugx.org/renoki-co/l1/d/monthly)](https://packagist.org/packages/renoki-co/l1)
[![License](https://poser.pugx.org/renoki-co/l1/license)](https://packagist.org/packages/renoki-co/l1)

Extend your PHP/Laravel application with Cloudflare bindings.

This package offers support for:

- [x] [Cloudflare D1](https://developers.cloudflare.com/d1)
- [ ] [Cloudflare KV](https://developers.cloudflare.com/kv/)
- [ ] [Cloudflare Queues](https://developers.cloudflare.com/queues)

## 🚀 Installation

You can install the package via Composer:

```bash
composer require renoki-co/l1
```

## 🚀 Installation - Fork
To ensure that the modified version of the package is installed in the Laravel application, you can follow these steps:

Remove the original package from your Laravel application's composer.json file and run composer update to remove any traces of the original package.

Add a custom repository to your composer.json file that points to your forked package. This will tell Composer to use your modified version instead of the original one. Here's an example of how to add the repository:

Copy
```
"repositories": [
    {
        "type": "vcs",
        "url": "https://github.com/your-username/your-forked-package"
    }
]
```
Replace `"https://github.com/your-username/your-forked-package"` with the URL of your forked package's repository.

Require your package in your Laravel application's composer.json file. Make sure to specify the version constraint to use your forked package. For example:
Copy
```
"require": {
    "your-username/your-package": "dev-master"
}
```
Replace `"your-username/your-package"` with the name of your package.

Run `composer update` to install your package and its dependencies. Composer will now fetch your forked package from the custom repository you added.
By following these steps, your Laravel application should now use the modified version of the package instead of the original one.

## 🙌 Usage

### D1 with raw PDO

Though D1 is not connectable via SQL protocols, it can be used as a PDO driver via the package connector. This proxies the query and bindings to the D1's `/query` endpoint in the Cloudflare API.

```php
use RenokiCo\L1\D1\D1Pdo;
use RenokiCo\L1\D1\D1PdoStatement;
use RenokiCo\L1\CloudflareD1Connector;

$pdo = new D1Pdo(
    dsn: 'sqlite::memory:', // irrelevant
    connector: new CloudflareD1Connector(
        database: 'your_database_id',
        token: 'your_api_token',
        accountId: 'your_cf_account_id',
    ),
);
```

### D1 with Laravel

In your `config/database.php` file, add a new connection:

```php
'connections' => [
    'd1' => [
        'driver' => 'd1',
        'prefix' => '',
        'database' => env('CLOUDFLARE_D1_DATABASE_ID', ''),
        'api' => 'https://api.cloudflare.com/client/v4',
        'auth' => [
            'token' => env('CLOUDFLARE_TOKEN', ''),
            'account_id' => env('CLOUDFLARE_ACCOUNT_ID', ''),
        ],
    ],
]
```

Then in your `.env` file, set up your Cloudflare credentials:

```
CLOUDFLARE_TOKEN=
CLOUDFLARE_ACCOUNT_ID=
CLOUDFLARE_D1_DATABASE_ID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

The `d1` driver will proxy the PDO queries to the Cloudflare D1 API to run queries.

## 🐛 Testing

Start the built-in Worker that simulates the Cloudflare API:

```bash
cd tests/worker
npm ci
npm run start
```

In a separate terminal, run the tests:

``` bash
vendor/bin/phpunit
```

## 🤝 Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.

## 🔒  Security

If you discover any security related issues, please email <alex@renoki.org> instead of using the issue tracker.

## 🎉 Credits

- [Alex Renoki](https://github.com/rennokki)
- [All Contributors](../../contributors)
