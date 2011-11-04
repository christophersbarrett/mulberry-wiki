# Versioning Philosophy

While we are in beta, we will be following a modified form of [Semantic Versioning](http://semver.org/). Instead of Major revisions indicating breaking changes, we'll generally behave as follows:

* "Master" at a given point in time may be unstable and/or include breaking changes
* "Blessed" releases will be tagged - we aim to have a release once per month
* Non breaking changes will bump the patch version (e.g. 0.1.0 -> 0.1.1)
* Breaking changes will bump the minor version (e.g. 0.1.1 -> 0.2.0)

# Commitment

We pledge that we will document any/all breaking changes (things you must do to upgrade) for all minor version updates. We will keep this document up-to-date in as realtime a manner as possible.

When we release a new version (0.x) we will simultaneously create a wiki article called "Upgrading from 0.x to 0.x+1". Whenever a pull request lands that introduces a breaking change, we will add an entry in that article explaining how to implement the change, along with a link to the pull request.

As we go along, we will continue to release additional documents but will NOT go back and update older ones. For example, we will not modify the "Upgrading from 0.1 to 0.2" document to be something akin to "Upgrading from 0.1 to 0.3". If you are on 0.1 and wish to upgrade to 0.3, you'll have to perform the steps in "Upgrading from 0.1 to 0.2" and then "Upgrading from 0.2 to 0.3".

**Note**: This only applies to the BETA. Once we hit 1.0, we will revise the policy to more accurately reflect SemVer.