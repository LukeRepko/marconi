Marconi Functional Tests
====================

Marconi's functional tests treat Marconi as a black box. In other
words, the API calls attempt to simulate an actual user. Unlike unit tests,
the functional tests do not use mockendpoints.

Configuring the tests
---------------------

(NOTE: take the following section with salt; it's my best guess of how
this works)

The tests look in ``$MARCONI_TESTS_CONFIGS_DIR`` for config files. The
two files that are relevant to the functional tests are
``functional-tests.conf`` and ``functional-marconi.conf``.
``functional-tests.conf`` contains the test configuration.
``functional-marconi.conf`` is a (partial?) mirror of the configuration
of *the server you are testing against*. In particular, the values in
the "limits" section must match the target server. They are used to
check for correct rejection of requests that exceed the limits.

There are example configs in this repository under ``tests/etc``.


Running functional tests (With Tox)
-----------------------------------

#. Setup a Marconi server. Refer to the Marconi `README`_ on
   how to run Marconi locally, or simply use an existing server.

#. Change `$MARCONI_TESTS_CONFIGS_DIR/functional-tests.conf` and
   set `run_tests` to True.

#. Run tests. ::

   $ tox

#. Filter tests. ::

   $ tox -- --tests tests.functional.wsgi.v1.test_messages

#. Run tests for specific environments. ::

   $ tox -epy27,pep8

Running the Functional Tests (Without Tox)
------------------------------------------

#. Setup a Marconi server. Refer to the Marconi `README`_ on
   how to run Marconi locally, or simply use an existing server.

#. Install functional tests dependencies. ::

     pip install -r requirements.txt
     pip install -r test-requirements.txt

#. cd to the marconi/tests/functional directory

#. Copy marconi/tests/etc/functional-tests.conf-sample to one of the following locations::

     ~/.marconi/functional-tests.conf
     /etc/marconi/functional-tests.conf

#. Update the config file to point to the Marconi server you want to run
   the tests against

#. If leaving keystone auth enabled, update system-tests.conf with a
   valid set of credentials.

#. Now, to run the sytem tests, simply use the nosetests commands, e.g.:

    Run all test suites: ::

        nosetests --tests tests.functional -v

Adding New Tests
----------------

#. Add test case to an appropriate  test case file: ::

    queue/test_queue.py
    messages/test_messages.py
    claim/test_claims.py

.. _README : https://github.com/stackforge/marconi/blob/master/README.rst
.. _requests : https://pypi.python.org/pypi/requests
