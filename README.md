# Mandrill wrapper for PhalconPhp
A PhalconPhp component for using [Mandrill's](http://mandrill.com/) super-fine [API](http://mandrillapp.com/api/docs/).

See Mandrill's [help docs](http://help.mandrill.com/home).

# Install
Add Mandrill Api key to your config file. Usually `app/config/application.php`
```php
'mandrill_api_key' => 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
```

Add the Tartan folder into your `app/libraries` or `app/vendor` directory.

Register Tartan namespace in your application bootstrap. Usually `public/index.php`

```php
    $loader = new Phalcon\Loader();
    $loader->registerNamespaces(array(
        // Libraries
        'Tartan'      => __DIR__ . '/library/Tartan', // or /vendor/Tartan
        // Modules
        'Site'    => __DIR__ . '/modules/Site',
        'Backend' => __DIR__ . '/modules/Backend',
    ));
    $loader->register();
```

And add Mandrill to your app DI

```php
    $config = include __DIR__ . '/config/application.php';
    /**
     * Mail service uses AmazonSES
     */
    $di->set('mandrill', function() use ($config) {
        return new Tartan\Mandrill($config->mandrill_api_key);
    });
```


# Usage (in controller)
```php

    //In some controller, far far away

    //Prepare email!
    $email = array(
        'html'       => '<p>This is my message<p>', //Consider using a view file
        'text'       => 'This is my plaintext message',
        'subject'    => 'This is my subject',
        'from_email' => 'me@mydomain.com',
        'from_name'  => 'My website name',
        'to'         => array(array('email' => 'joe@example.com' )) //Check documentation for more details on this one
        //'to'       => array(array('email' => 'joe@example.com' ),array('email' => 'joe2@example.com' )) //for multiple emails
        );

    //Send email!
    $result = $this->mandrill->messages_send($email);

```

#Usage (anywhere)
```php
    //In anywhere, far far away
    $di = \Phalcon\DI::getDefault();
    $mandrill = $di->get('mandrill');

    //Prepare email!
    $email = array(
        'html'       => '<p>This is my message<p>', //Consider using a view file
        'text'       => 'This is my plaintext message',
        'subject'    => 'This is my subject',
        'from_email' => 'me@mydomain.com',
        'from_name'  => 'My website name',
        'to'         => array(array('email' => 'joe@example.com' )) //Check documentation for more details on this one
        //'to'       => array(array('email' => 'joe@example.com' ),array('email' => 'joe2@example.com' )) //for multiple emails
        );

    //Send email!
    $result = $mandrill->messages_send($email);
```
