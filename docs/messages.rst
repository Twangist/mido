Messages
=========

A Mido message is a Python object with methods and attributes. The
attributes will vary depending on message type.

To create a new message::

    >>> mido.Message('note_on')
    <message note_on channel=0, note=0, velocity=64, time=0>

You can pass attribute as keyword arguments::

    >>> mido.Message('note_on', note=100, velocity=3, time=6.2)
    <message note_on channel=0, note=100, velocity=3, time=6.2>

All attributes will default to 0. The exceptions are ``velocity``,
which defaults to 64 (middle velocity) and ``data`` which defaults to
``()``.

You can set and get attrbutes as you would expect::

    >>> msg = mido.Message('note_on')
    >>> msg.note
    0
    >>> msg.note = 100
    >>> msg.note
    100

The ``type`` attribute can be used to determine message type::

    >>> msg.type
    'note_on'

Mido supports all message types defined by the MIDI standard. For a
full list of messages and their attributes, see :doc:`message_types`.


Converting To Bytes
--------------------

You can convert a message to MIDI bytes with one of these methods:

    >>> msg = mido.Message('note_on')
    >>> msg
    <message note_on channel=0, note=0, velocity=64, time=0>
    >>> msg.bytes()
    [144, 0, 64]
    >>> msg.bin()
    bytearray(b'\x90\x00@')
    >>> msg.hex()
    '90 00 40'

You can turn bytes back into messages with the :doc:`parser`.


The Time Attribute
-------------------

Each message has a ``time`` attribute, which can be set to any value
of type ``int`` or ``float`` (and in Python 2 also ``long``). What you
do with this value is entirely up to you.

Some parts of Mido uses the attribute for special purposes. In MIDI
file tracks, it is used as delta time (in ticks).

*Note*: the ``time`` attribute is not included in comparisons, so if
 you want it included you'll have to do::

    (msg1, msg1.time) == (msg2, msg2.time)


System Exclusive Messages
--------------------------

Sytem Exclusive (SysEx) messages are used to send device specific
data. They have one attribute, ``data``, which is the payload of the
message::

    >>> msg = Message('sysex', data=(1, 2, 3))
    >>> msg
    <message sysex data=(1, 2, 3), time=0>

You can pass or set any (finite) iterable and will be converted to a
tuple::

    >>> msg.data = range(3)
    >>> msg.data
    (0, 1, 2)