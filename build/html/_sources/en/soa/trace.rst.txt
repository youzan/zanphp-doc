Call Chain
==========

    develop programmer ignore this doc. only service for framework
    development.

Call Chain Init
~~~~~~~~~~~~~~~

Call Chain init in filter.If parentId、rootId and eventId exit,it wont
create new rootId and parentId. These ids transport by nova protocol and
only tcp server support transported by first-layer call chain.

Call Chain Trace
~~~~~~~~~~~~~~~~

Call Chain trace in novaclient, db, httpclient and so on.

Call Chain Data Monitor
~~~~~~~~~~~~~~~~~~~~~~~

Call Chain post data in TraceTerminator.
