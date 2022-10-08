# Data Layer @JPsistemasBR

###### The data layer is a persistent abstraction component of your database that PDO has prepared instructions for performing common routines such as registering, reading, editing, and removing data.

O data layer é um componente para abstração de persistência no seu banco de dados que usa PDO com prepared statements
para executar rotinas comuns como cadastrar, ler, editar e remover dados.

## About JPsistemasBR

###### JPsistemasBR is a set of small and optimized PHP components for common tasks. Held by JP Sistemas BR and the UpInside team. With them you perform routine tasks with fewer lines, writing less and doing much more.

JPsistemasBR é um conjunto de pequenos e otimizados componentes PHP para tarefas comuns. Mantido por JP Sistemas BR e a
equipe UpInside. Com eles você executa tarefas rotineiras com poucas linhas, escrevendo menos e fazendo muito mais.

### Highlights

- Easy to set up (Fácil de configurar)
- Total CRUD asbtration (Asbtração total do CRUD)
- Create safe models (Crie de modelos seguros)
- Composer ready (Pronto para o composer)
- PSR-2 compliant (Compatível com PSR-2)

## Installation

Data Layer is available via Composer:

```bash
"coffeecode/datalayer": "2.0.*"
```

or run

```bash
composer require coffeecode/datalayer
```

## Documentation

###### For details on how to use the Data Layer, see the sample folder with details in the component directory

Para mais detalhes sobre como usar o Data Layer, veja a pasta de exemplo com detalhes no diretório do componente

#### connection

###### To begin using the Data Layer, you need to connect to the database (MariaDB / MySql). For more connections [PDO connections manual on PHP.net](https://www.php.net/manual/pt_BR/pdo.drivers.php)

Para começar a usar o Data Layer precisamos de uma conexão com o seu banco de dados. Para ver as conexões possíveis
acesse o [manual de conexões do PDO em PHP.net](https://www.php.net/manual/pt_BR/pdo.drivers.php)

```php
const DATA_LAYER_CONFIG = [
    "driver" => "mysql",
    "host" => "localhost",
    "port" => "3306",
    "dbname" => "datalayer_example",
    "username" => "root",
    "passwd" => "",
    "options" => [
        PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES utf8",
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_OBJ,
        PDO::ATTR_CASE => PDO::CASE_NATURAL
    ]
];
```

#### your model

###### The Data Layer is based on an MVC structure with the Layer Super Type and Active Record design patterns. Soon to consume it is necessary to create the model of your table and inherit the Data Layer.

O Data Layer é baseado em uma estrutura MVC com os padrões de projeto Layer Super Type e Active Record. Logo para
consumir é necessário criar o modelo de sua tabela e herdar o Data Layer.

```php
<?php

class User extends DataLayer
{
    /**
     * User constructor.
     */
    public function __construct()
    {
        //string "TABLE_NAME", array ["REQUIRED_FIELD_1", "REQUIRED_FIELD_2"], string "PRIMARY_KEY", bool "TIMESTAMPS"
        parent::__construct("users", ["first_name", "last_name"]);
    }
}
```

#### find

```php
<?php

use Example\Models\User;

$model = new User();

//find all users
$users = $model->find()->fetch(true);

//find all users limit 2
$users = $model->find()->limit(2)->fetch(true);

//find all users limit 2 offset 2
$users = $model->find()->limit(2)->offset(2)->fetch(true);

//find all users limit 2 offset 2 order by field ASC
$users = $model->find()->limit(2)->offset(2)->order("first_name ASC")->fetch(true);

//looping users
foreach ($users as $user) {
    echo $user->first_name;
}

//find one user by condition
$user = $model->find("first_name = :name", "name=Robson")->fetch();
echo $user->first_name;

//find one user by two conditions
$user = $model->find("first_name = :name AND last_name = :last", "name=JP&last=Sistemas")->fetch();
echo $user->first_name . " " . $user->first_last;
```

#### findById

```php
<?php

use Example\Models\User;

$model = new User();
$user = $model->findById(2);
echo $user->first_name;
```

#### secure params

###### See example find_example.php and model classes

Consulte exemplo find_example.php e classes modelo

```php
<?php

$params = http_build_query(["name" => "JPsistemas & Associated"]);
$company = (new Company())->find("name = :name", $params);
var_dump($company, $company->fetch());
```

#### join method

###### See example find_example.php and model classes

Consulte exemplo find_example.php e classes modelo

```php
<?php

$addresses = new Address();
$address = $addresses->findById(22);
//get user data to this->user->[all data]
$address->user();
var_dump($address);
```

#### count

```php
<?php

use Example\Models\User;

$model = new User();
$count = $model->find()->count();
```

#### save create

```php
<?php

use Example\Models\User;

$user = new User();
$user->first_name = "JP";
$user->last_name = "Sistemas";
$userId = $user->save();
```

#### save update

```php
<?php

use Example\Models\User;

$user = (new User())->findById(2);
$user->first_name = "JP";
$userId = $user->save();
```

#### destroy

```php
<?php

use Example\Models\User;

$user = (new User())->findById(2);
$user->destroy();
```

#### fail

```php
<?php

use Example\Models\User;

$user = (new User())->findById(2);
if($user->fail()){
    echo $user->fail()->getMessage();
}
```

#### custom data method

````php
<?php

class User{

    public function fullName(): string 
    {
        return "{$this->first_name} {$this->last_name}";
    }
    
    public function document(): string
    {
        return "Restrict";
    }
}

echo $this->full_name; //JP Sistemas BR
echo $this->document; //Restrict
```` 

## Contributing

Please see [CONTRIBUTING](https://github.com/jpsistemasbr/datalayer/blob/master/CONTRIBUTING.md) for details.

## Support

###### Security: If you discover any security related issues, please email jpsistemasbr@gmail.com.br instead of using the issue tracker.

Se você descobrir algum problema relacionado à segurança, envie um e-mail para jpsistemasbr@gmail.com.br em vez de usar o
rastreador de problemas.

Thank you

## Credits

- [JP Sistemas BR](https://github.com/jpsistemasbr) (Developer)

## License

The MIT License (MIT). Please see [License File](https://github.com/jpsistemasbr/datalayer/blob/master/LICENSE) for more
information.