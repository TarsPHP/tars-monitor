# tars-monitor

------------------



`Tar monitor is a 'phptars' service and feature monitoring and reporting module



It consists of two sub modules:

*Service monitoring

*Feature monitoring



##How to use



Installation of cable vehicles



Using composer for installation

`composer install phptars/tars-monitor`



The call of



####Service Monitoring Report



*Locator is the report address, which is generally distributed by the server

*Socket mode is set to 1, and socket mode is used for reporting

Socketmode is set to 2. Use swoole TCP client mode for reporting (need swoole support)

Socketmode is set to 3. Use swoole TCP coroutine client to report (need support of swoole 2.0 or above)



####Scheduled reporting (default)

>The use of scheduled reporting requires the support of swoole table. By calling addstat, the reporting information will be temporarily saved. The task process of the tar server will collect and package the reporting information for a period of time (the reporting interval is issued by the server, generally 60s), which can reduce the reporting request

```php

$locator = "tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890";

$socketMode = 1;

$statfWrapper = new \Tars\monitor\StatFWrapper($locator,$socketMode);

$statfWrapper->addStat("PHPTest.helloTars.obj","test","172.16.0.116",51003,200,0,0);

$statfWrapper->addStat("PHPTest.helloTars.obj","test","172.16.0.116",51003,200,0,0);

$statfWrapper->addStat("PHPTest.helloTars.obj","test","172.16.0.116",51003,200,0,0);

` ` ` `



Report data can be stored in a variety of ways. Cache provides the implementation of 'swoole table' and 'redis'. Users can also implement' contract / storecache interface 'by themselves. Refer to the configuration of' Src / services. PHP 'in demo' tar HTTP server 'for the configuration of storage methods.

```php

return array(

'namespaceName' => 'HttpServer\\',

'monitorStoreConf' => [

'className' => Tars\monitor\cache\SwooleTableStoreCache::class,

'config' => []

]

);

` ` ` `

`Monitorstoreconf 'is the configuration of storage mode, where' classname 'is the implementation class and' config 'is the corresponding configuration. For example, when using the redis storage mode, the host, port and key prefix of redis need to be configured in config.

When 'monitorstoreconf' is not configured, 'swooletablestorecache' is used for storage by default.




####Single report

>At the same time, 'tar monitor' also provides a single escalation interface, 'monitorstat'. That is to say, each call of tar request will be reported once, which is not recommended

```php

$locator = "tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890";

$socketMode = 1;

$statfWrapper = new \Tars\monitor\StatFWrapper($locator,$socketMode);

$statfWrapper->monitorStat("PHPTest.helloTars.obj","test","172.16.0.116",51003,200,0,0);

` ` ` `



####Feature monitoring



Parameters are similar to service monitoring



```php

$statfWrapper = new \Tars\monitor\PropertyFWrapper("tars.tarsregistry.QueryObj@tcp -h 172.16.0.161 -p 17890",1);

$statfWrapper->monitorProperty("127.0.0.1","userdefined",'Sum',2);

$statfWrapper->monitorProperty("127.0.0.1","userdefined",'Count',2);

$statfWrapper->monitorProperty("127.0.0.1","userdefined",'Count',1);

` ` ` `



###Monitor view

After data reporting, users can view the reported data in the service monitoring / feature monitoring tab.



Others

Because other modules have integrated this module, in general, service scripts do not need to explicitly use this module. 
