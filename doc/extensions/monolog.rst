MonologExtension
================

The *MonologExtension* provides a default logging mechanism
through Jordi Boggiano's `Monolog <https://github.com/Seldaek/monolog>`_
library.

It will log requests and errors and allow you to add debug
logging to your application, so you don't have to use
``var_dump`` so much anymore. You can use the grown-up
version called ``tail -f``.

Parameters
----------

* **monolog.logfile**: File where logs are written to.

* **monolog.class_path** (optional): Path to where the
  Monolog library is located.

* **monolog.level** (optional): Level of logging defaults
  to ``DEBUG``. Must be one of ``Logger::DEBUG``, ``Logger::INFO``,
  ``Logger::WARNING``, ``Logger::ERROR``. ``DEBUG`` will log
  everything, ``INFO`` will log everything except ``DEBUG``,
  etc.

* **monolog.name** (optional): Name of the monolog channel,
  defaults to ``myapp``.

Services
--------

* **monolog**: The monolog logger instance.

  Example usage::

    $app['monolog']->addDebug('Testing the Monolog logging.');

* **monolog.configure**: Protected closure that takes the
  logger as an argument. You can override it if you do not
  want the default behavior.

Registering
-----------

Make sure you place a copy of *Monolog* in the ``vendor/monolog``
directory::

    $app->register(new Silex\Extension\MonologExtension(), array(
        'monolog.logfile'       => __DIR__.'/development.log',
        'monolog.class_path'    => __DIR__.'/vendor/monolog/src',
    ));

.. note::

    Monolog is not compiled into the ``silex.phar`` file. You have to
    add your own copy of Monolog to your application.

Usage
-----

The MonologExtension provides a ``monolog`` service. You can use
it to add log entries for any logging level through ``addDebug()``,
``addInfo()``, ``addWarning()`` and ``addError()``.

::

    use Symfony\Component\HttpFoundation\Response;

    $app->post('/user', function () use ($app) {
        // ...

        $app['monolog']->addInfo(sprintf("User '%s' registered.", $username));

        return new Response('', 201);
    });

For more information, check out the `Monolog documentation
<https://github.com/Seldaek/monolog>`_.
