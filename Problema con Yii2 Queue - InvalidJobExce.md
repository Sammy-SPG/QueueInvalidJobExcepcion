Problema con Yii2 Queue - InvalidJobException: Failed to instantiate component or class

Hola a todos,

Estoy trabajando en un proyecto usando Yii2 y estoy intentando implementar una cola de tareas utilizando la extensión Yii2 Queue con el driver de base de datos. Después de la instalación y la configuración, estoy enfrentando un problema que no puedo solucionar.

La configuración de mi cola de tareas es la siguiente:

```php

return [
    'bootstrap' => ['log','queue'],
    'components' => [
        'db' => [
            'class' => 'yii\db\Connection',
            'dsn' => 'mysql:host=127.0.0.1;dbname=lectorum_erp',
            'username' => 'root',
            'password' => '',
            'charset' => 'utf8',
        ],
        'log' => [
            'targets' => [
                [
                    'class' => 'yii\log\FileTarget',
                    'levels' => ['error', 'warning'],
                ],
            ],
        ],
        'queue' => [
            'class' => \yii\queue\db\Queue::class,
            'db' => 'db', // DB connection component or its config
            'as log' => \yii\queue\LogBehavior::class, 
            'tableName' => '{{%queue}}', // Table name
            'channel' => 'default', // Queue channel key
            'mutex' => \yii\mutex\MysqlMutex::class, // Mutex used to sync queries
            'strictJobType' => false,
            //'serializer' => \yii\queue\serializers\JsonSerializer::class, // change to JSON serializer
        ],
    ],
];

```

He creado un trabajo llamado TestJob en la carpeta de modelos de mi aplicación backend, que implementa JobInterface:

```php

namespace app\models;
use Yii;
use yii\base\BaseObject;

class TestJob extends BaseObject implements \yii\queue\JobInterface
{
    public function execute($queue)
    {
        echo "El trabajo de prueba se ejecutó correctamente.\n";
    }
}


```

Al intentar ejecutar el trabajo, recibo el siguiente error: yii\queue\InvalidJobException: Failed to instantiate component or class "app\models\TestJob". He revisado la ruta de la clase, el nombre del archivo y he regenerado el autoloader de Composer con composer dump-autoload, pero el error persiste.

Además, si quito la línea del serializador en mi configuración de la cola y utilizo la serialización por defecto de PHP, obtengo un error diferente: yii\queue\InvalidJobException: Job must be a JobInterface instance instead of __PHP_Incomplete_Class.

¿Alguien ha enfrentado un problema similar o tiene alguna sugerencia sobre qué podría estar causando esto?

Gracias de antemano por su ayuda.