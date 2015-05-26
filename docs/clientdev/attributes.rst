Attributes
==========

Attributes are simple data that Sentry understands to provide the most
basic information about events.  These are things like the unique ID of an
event, the human readable message etc.

Attributes are separate from :doc:`interfaces` which provide very specific
and tailored data such as exception data, stacktraces etc.

Required Attributes
-------------------

The following attributes are required for all events:

.. describe:: event_id

    Hexadecimal string representing a uuid4 value.

    The length is exactly 32 characters (no dashes!)

    .. sourcecode:: json

        {
          "event_id": "fc6d8c0c43fc4630ad850ee518f1b9d0"
        }

.. describe:: message

    User-readable representation of this event

    Maximum length is 1000 characters.

    .. sourcecode:: json

        {
          "message": "SyntaxError: Wattttt!"
        }

.. describe:: timestamp

    Indicates when the logging record was created (in the Sentry client).

    The Sentry server assumes the time is in UTC.

    The timestamp should be in ISO 8601 format, without a timezone.

    .. sourcecode:: json

        {
          "timestamp": "2011-05-02T17:41:36"
        }

.. describe:: level

    The record severity.

    Defaults to ``error``.

    The value needs to be one on the supported level string values.

    .. sourcecode:: json

        {
          "level": "warning"
        }

    Acceptable values are:

    * ``fatal``
    * ``error``
    * ``warning``
    * ``info``
    * ``debug``

.. describe:: logger

    The name of the logger which created the record.

    .. sourcecode:: json

        {
          "logger": "my.logger.name"
        }

Optional Attributes
-------------------

Additionally, there are several optional values which Sentry recognizes and are
highly encouraged:

.. describe:: platform

    A string representing the platform the client is submitting from. This will
    be used by the Sentry interface to customize various components in the
    interface.

    .. sourcecode:: json

        {
          "platform": "python"
        }


.. describe:: culprit

    Function call which was the primary perpetrator of this event.

    .. sourcecode:: json

        {
          "culprit": "my.module.function_name"
        }


.. describe:: server_name

    Identifies the host client from which the event was recorded.

    .. sourcecode:: json

        {
          "server_name": "foo.example.com"
        }


.. describe:: release

    The release version of the application.

    This value will generally be something along the lines of the git SHA
    for the given project.

    .. sourcecode:: json

        {
          "release": "721e41770371db95eee98ca2707686226b993eda"
        }


.. describe:: tags

    A map or list of tags for this event.

    .. sourcecode:: json

        {
          "tags": {
            "ios_version": "4.0",
            "context": "production"
          }
        }

    .. sourcecode:: json

        {
          "tags": [
            ["ios_version", "4.0"],
            ["context", "production"]
          ]
        }

.. describe:: modules

    A list of relevant modules and their versions.

    .. sourcecode:: json

        {
          "modules": {
            "my.module.name": "1.0"
          }
        }

.. describe:: extra

    An arbitrary mapping of additional metadata to store with the event.

    .. sourcecode:: json

        {
          "extra": {
            "my_key": 1,
            "some_other_value": "foo bar"
          }
        }

Custom Grouping
---------------

A feature that clients should not be using, but exists anyways for very
specialized cases is the `checksum`.  If supplied then Sentry will not
group by its own rules but purely by the supplied `checksum` value.

.. describe:: checksum

    A hash value that is used as grouping identifier.  It should not be
    supplied by clients unless they have a very specialized usecase where
    they need to override the default grouping that sentry performs.

    It's a 32 character checksum.  Typically it can be implemented as an
    MD5 hash.

    .. sourcecode:: json

        {
          "checksum": "d41d8cd98f00b204e9800998ecf8427e"
        }

For information about overriding grouping see :ref:`custom-grouping`.