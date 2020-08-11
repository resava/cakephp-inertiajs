# Inertia.js CakePHP Adapter

## Installation

1. Get project into your system via [composer](https://getcomposer.org)

    ```bash
    composer require ishanvyas22/cakephp-inertiajs
    ```

2. Load plugin into your application

    ```bash
    bin/cake plugin load Inertia
    ```

3. Generate inertia scaffolding

    ```bash
    bin/cake asset_mix generate inertia-vue
    ```

    **Note:** Above command will generate basic scaffolding to use inertia with Vue.js. For more frontend adapters please visit https://inertiajs.com/client-side-setup.

3. Install dependencies

    ```bash
    npm install
    ```

## Setup

1. Load Inertia component into your application, provide default template file path in `defaultTemplate` key into `AppController.php`

    ```
    public function initialize()
    {
        parent::initialize();

        $this->loadComponent('Inertia.Inertia', [
            'request' => $this->getRequest(),
            'response' => $this->getResponse(),
            'defaultTemplate' => '/Main/inertia'
        ]);
    }
    ```

2. To setup root template, create new template file that you've provided into previous step. In our case create ``src/Template/Main/inertia.ctp``

    First load Inertia helper into your ``AppView.php`` file:
    ```
    $this->loadHelper('Inertia.Inertia');
    ```

    Into ``inertia.ctp`` file:
    ```
    echo $this->Inertia->make($page, 'app');
    ```

    Behind the scene it will create a ``div`` element with ``id="app"`` attribute. You can change ``app`` id according to your convenience.

## Creating responses
To make inertia response you can simply call ``render()`` function of ``InertiaComponent``:

```
<?php

namespace App\Controller;

class UsersController extends AppController
{
    public function index()
    {
        $component = 'Users/Index';
        $props = [
            'users' => $this->Users->find()->toArray()
        ];

        return $this->Inertia->render($component, $props);
    }
}
```

By default if you have copied the ``webpack.mix.js`` file from plugin, it will render ``Index.vue`` file into ``webroot/js/Pages/Users`` directory.

## Sharing data

Often you want to use some data across your application, for example accessing current logged in user's name or flash messages. You can easily share these type data using ``share()`` function.

Set application name:

```
$this->Inertia->share('app.name', 'Inertia');
```

Set currently logged in user's details:

```
$this->Inertia->share('auth.user', function () {
    $authUser = null;

    if ($this->Authentication->getIdentity()) {
        $authUser = $this->Authentication->getIdentity();
    }

    return $authUser;
});
```

Set flash messages:

```
$this->Inertia->share('app.flash', function () {
    return $this->Inertia->getFlashData();
});
```

## Issues
Feel free to submit issues and enhancement requests.
