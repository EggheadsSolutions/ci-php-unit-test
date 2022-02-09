# php-unit-test

Action для unit тестирования без clickhouse базы проекта.   

Имеет следующие переменные:
 - mysql (по умолчанию 'yes') Если в тесте не используется mysql то нужно указать 'no'
 - mysql-version (по умолчанию 8.0) Версия mysql которая будет установлена, если mysql не нужен, можно проигнорировать
 - migrate-command (по умолчанию 'vendor/bin/phinx') Для новых проектов может использоваться значение 'bin/cake migrations'
 - redis (по умолчанию 'yes'). Если redis не нужен для тестирования нужно проставить 'no'
 - clickhouse (по умолчанию 'yes') Если clickhouse не нужен нужно проставить 'no'
 - php-version (по умолчанию 7.4). Можно использовать матричное тестирование с разными версиями.
   Пример:
   ```yaml
    jobs:
      test:
        name: PHP Unit Tests
        runs-on: ubuntu-latest
        strategy:
          matrix:
            php-versions: [ '7.4', '8.0' ]
        steps:
          - uses: EggheadsSolutions/php-unit-test@v1
            with:
              php-version: ${{ matrix.php-versions }}
   ```
  - extensions (по умолчанию след пакеты) 
   ```yaml
   'bcmath, iconv, ctype, gd, mbstring, mysqli, pdo, pdo_mysql, sockets, zip, soap, intl, fileinfo, json, libxml, openssl, pcntl, posix, simplexml, zend-opcache, runkit7, igbinary, redis'
   ```
  - config-file (по умолчанию 'phpunit.xml.dist'). Кофигурационный файл для тестирования. Если отличается, указать пусть в этой переменной.
  - additional-parameters Дополнительные параметры к команде unittest, можно перечислить все в строке. И то, что внисете будет запущено с командой.
  - use-repository-config (по умолчанию 'yes') - брать ли app_local.php конфиг из репозитория (в репозитории он должен называться app_local_ci.php). Если указать значение 'no', то нужно в переменной app-local-php занести конфиг.
   ```yaml
   jobs:
      test:
        name: PHP Unit Tests
        runs-on: ubuntu-latest
        strategy:
          matrix:
            php-versions: [ '7.4', '8.0' ]
        steps:
          - uses: EggheadsSolutions/php-unit-test@v1
            with:
              php-version: ${{ matrix.php-versions }}
              use-repository-config: 'no'
              app-local-php: |
                '<?php
                 return [
                   'Datasources' => ['test' => ['username' => 'root', 'password' => 'root', 'host' => '127.0.0.1', 'port' => '3306', 'database' => 'cakephp_test', 'persistent'  => false, 'encoding' => 'utf8'], 'default' => ['username' => 'root', 'password' => 'root', 'host' => '127.0.0.1', 'port' => '3306', 'database' => 'cakephp', 'persistent' => false, 'encoding' => 'utf8']],
                   'EmailTransport' => ['test' => ['className' => 'ArtSkills.TestEmail']],
                   'debug' => true,
                   'Security' => ['salt' => '7c47f1e793a39c7f518efc6b909b920ed5ba7a7470efc0501f2960973b7954dd'],
                   'Sentry' => ['dsn' => ''],
                   'testServerName' => 'eggheads.solutions',
                  ];'
   ```
   - post-install-cmd (по умолчанию 'yes'). Переменная вставляет запуск composer run-script post-install-cmd --no-interaction. Если такой запуск не нужен, проставить 'no'
   - config-dir (по умолчанию 'config'). Имя директории в которой хранятся app_local.php конфиг. Если директория в репозитории отличается, то прописать относитьельно корня репозитория.
   
