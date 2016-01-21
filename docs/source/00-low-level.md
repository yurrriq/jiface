# Using the Low-level API

## The JInterface Package in Clojang

**NOTICE**: This document is a copy of the JInterface Users Guide, a [copy](erlang/jinterface_users_guide.html) of which is provided in the published documentation for the Clojang project.

The [Jinterface](erlang/java/com/ericsson/otp/erlang/package-summary.html) package provides a set of tools for communication with Erlang processes. It can also be used for communication with other Java processes using the same package, as well as C processes using the Erl_Interface library.

The set of classes in the package can be divided into two categories: those that provide the actual communication, and those that provide a Java representation of the Erlang data types. The latter are all subclasses of OtpErlangObject, and they are identified by the ``OtpErlang`` prefix.

Since this package provides a mechanism for communicating with Erlang, message recipients can be Erlang processes or instances of com.ericsson.otp.erlang.OtpMbox, both of which are identified with pids and possibly registered names. When pids or mailboxes are mentioned as message senders or recipients in this section, it should assumed that even Erlang processes are included, unless specified otherwise. The classes in [Jinterface](erlang/java/com/ericsson/otp/erlang/package-summary.html) support the following:


*    manipulation of data represented as Erlang data types
*    conversion of data between Java and Erlang formats
*    encoding and decoding of Erlang data types for transmission or storage
*    communication between Java nodes and Erlang processes

In the following sections, these topics are described:

*    mapping of Erlang types to Java
*    encoding, decoding, and sending Erlang terms
*    connecting to a distributed Erlang node
*    using nodes, mailboxes and EPMD
*    sending and receiving Erlang messages and data
*    remote procedure calls
*    linking to remote processes
*    compiling your code for use with Jinterface
*    tracing message flow


## Mapping of Basic Erlang Types to Java

This section describes the mapping of Erlang basic types to Java.

| Erlang type         | Java type
|---------------------|------------------------------------------------------
|atom                 | [OtpErlangAtom](erlang/java/com/ericsson/otp/erlang/OtpErlangAtom.html)
|binary               | [OtpErlangBinary](erlang/java/com/ericsson/otp/erlang/OtpErlangBinary.html)
|floating point types | [OtpErlangFloat](erlang/java/com/ericsson/otp/erlang/OtpErlangFloat.html) or [OtpErlangDouble](erlang/java/com/ericsson/otp/erlang/OtpErlangDouble.html), depending on the floating point value size
|integral types       | One of [OtpErlangByte](erlang/java/com/ericsson/otp/erlang/OtpErlangByte.html), [OtpErlangChar](erlang/java/com/ericsson/otp/erlang/OtpErlangChar.html),[OtpErlangShort](erlang/java/com/ericsson/otp/erlang/OtpErlangShort.html), [OtpErlangUShort](erlang/java/com/ericsson/otp/erlang/OtpErlangUShort.html),[OtpErlangInt](erlang/java/com/ericsson/otp/erlang/OtpErlangInt.html),[OtpErlangUInt](erlang/java/com/ericsson/otp/erlang/OtpErlangUInt.html) or [OtpErlangLong](erlang/java/com/ericsson/otp/erlang/OtpErlangLong.html), depending on the integral value size and sign
|list                 | [OtpErlangList](erlang/java/com/ericsson/otp/erlang/OtpErlangList.html)
|pid                  | [OtpErlangPid](erlang/java/com/ericsson/otp/erlang/OtpErlangPid.html)
|port                 | [OtpErlangPort](erlang/java/com/ericsson/otp/erlang/OtpErlangPort.html)
|ref                  | [OtpErlangRef](erlang/java/com/ericsson/otp/erlang/OtpErlangRef.html)
|tuple                | [OtpErlangTuple](erlang/java/com/ericsson/otp/erlang/OtpErlangTuple.html)
|map                  | [OtpErlangMap](erlang/java/com/ericsson/otp/erlang/OtpErlangMap.html)
|term                 | [OtpErlangObject](erlang/java/com/ericsson/otp/erlang/OtpErlangObject.html)


## Special Mapping Issues

The atoms ``true`` and ``false`` are special atoms, used as boolean values. The class [OtpErlangBoolean](erlang/java/com/ericsson/otp/erlang/OtpErlangBoolean.html) can be used to represent these.

Lists in Erlang are also used to describe sequences of printable characters (strings). A convenience class [OtpErlangString](erlang/java/com/ericsson/otp/erlang/OtpErlangString.html) is provided to represent Erlang strings.


## Nodes

A node as defined by Erlang/OTP is an instance of the Erlang Runtime System, a virtual machine roughly equivalent to a JVM. Each node has a unique name in the form of an identifier composed partly of the hostname on which the node is running, e.g "gurka@sallad.com". Several such nodes can run on the same host as long as their names are unique. The class [OtpNode](http://oubiwann.github.io/clojang/current/erlang/java/com/ericsson/otp/erlang/OtpNode.html) represents an Erlang node. It is created with a name and optionally a port number on which it listens for incoming connections. Before creating an instance of [OtpNode](http://oubiwann.github.io/clojang/current/erlang/java/com/ericsson/otp/erlang/OtpNode.html), ensure that EPMD is running on the host machine. See the Erlang documentation for more information about EPMD. In this example, the host name is appended automatically to the identifier, and the port number is chosen by the underlying system:

```clojure
=> (require '[clojang.jinterface.otp :as otp])
nil
=> (def node (otp/node "gurka"))
#'user/node
```


## Mailboxes

TBD