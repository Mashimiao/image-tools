# Open Container Initiative Image Format Specification

The OCI Image Format project creates and maintains the software shipping container image format spec (OCI Image Format). The goal of this specification is to enable the creation of interoperable tools for building, transporting, and preparing a container image to run.

This specification defines how to create an OCI Image, which will generally be done by a build system, and output an [image manifest](manifest.md), a set of [filesystem layers](layer.md), and an [image configuration](serialization.md).
At a high level the image manifest contains metadata about the contents and dependencies of the image including the content-addressable identity of one or more filesystem serialization archives that will be unpacked to make up the final runnable filesystem.
The image configuration includes information such as application arguments, environments, etc.
The combination of the image manifest, image configuration, and one or more filesystem serializations is called the "OCI Image".

The keywords "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" are to be interpreted as described in [RFC 2119](http://tools.ietf.org/html/rfc2119) (Bradner, S., "Key words for use in RFCs to Indicate Requirement Levels", BCP 14, RFC 2119, March 1997).

![](img/build-diagram.png)

Once built the OCI Image can then be discovered by name, downloaded, verified by hash, trusted through a signature, and unpacked into an [OCI Runtime Bundle](https://github.com/opencontainers/runtime-spec/blob/master/bundle.md).

![](img/run-diagram.png)

## Understanding the Specification

The [Media Types](media-types.md) document is a starting point to understanding the overall structure of the specification. This document outlines the OCI Image file format specifications, including the critical filesystem serialization and image manifest described above.

The high level components of the spec include:

* An [image manifest](manifest.md), a set of [filesystem layers](layer.md), and [image configuration](serialization.md) (base layer)
* A process of hashing the image format for integrity and content-addressing (base layer)
* Signatures that are based on signing image content address (optional layer)
* Naming that is federated based on DNS and can be delegated (optional layer)

The optional and base layers of all OCI projects are tracked in the [OCI Scope Table](https://www.opencontainers.org/governance/oci-scope-table).

## Running an OCI Image

The OCI Image Format partner project is the [OCI Runtime Spec project](https://github.com/opencontainers/runtime-spec). The Runtime Specification outlines how to run a "[filesystem bundle](https://github.com/opencontainers/runtime-spec/blob/master/bundle.md)" that is unpacked on disk. At a high-level an OCI implementation would download an OCI Image then unpack that image into an OCI Runtime filesystem bundle. At this point the OCI Runtime Bundle would be run by an OCI Runtime.

This entire workflow supports the UX that users have come to expect from container engines like Docker and rkt: primarily, the ability to run an image with no additional arguments:

* docker run example.com/org/app:v1.0.0
* rkt run example.com/org/app,version=v1.0.0

To support this UX the OCI Image Format contains sufficient information to launch the application on the target platform (e.g. command, arguments, environment variables, etc).

## FAQ

**Q: Why doesn't this project mention distribution?**

A: Distribution, for example using HTTP as both Docker v2.2 and AppC do today, is currently out of scope on the [OCI Scope Table](https://www.opencontainers.org/governance/oci-scope-table). There has been [some discussion on the TOB mailing list]( https://groups.google.com/a/opencontainers.org/d/msg/tob/A3JnmI-D-6Y/tLuptPDHAgAJ) to make distribution an optional layer but this topic is a work in progress.

**Q: Why a new project?**

A: The first OCI spec centered around defining the run side of a container. This is generally seen to be an orthogonal concern to the shipping container component. As practical examples of this separation you see many organizations separating these concerns into different teams and organizations: the Docker Distribution project and the Docker containerd project; Amazon ECS and Amazon EC2 Container Registry, etc.

**Q: Why start this work now?**

A: We are seeing many independent implementations of container image handling including build systems, registries, and image analysis tools. As an organization we would like to encourage this growth and bring people together to ensure a technically correct and open specification continues to evolve reflecting the OCI values.

**Q: What happens to AppC or Docker Image Formats?**

A: Existing formats can continue to be a proving ground for technologies, as needed. The OCI Image Format project strives to provide a dependable open specification that can be shared between different tools and be evolved for years or decades of compatibility; as the deb and rpm format have.

## Roadmap

The [GitHub milestones](https://github.com/opencontainers/image-spec/milestones) lays out the path to the OCI v1.0.0 release in June 2016.

# Contributing

Development happens on GitHub for the spec.
Issues are used for bugs and actionable items and longer discussions can happen on the [mailing list](#mailing-list).

The specification and code is licensed under the Apache 2.0 license found in the `LICENSE` file of this repository.

## Code of Conduct

Participation in the OpenContainers community is governed by [OpenContainer's Code of Conduct](https://github.com/opencontainers/tob/blob/d2f9d68c1332870e40693fe077d311e0742bc73d/code-of-conduct.md).

## Discuss your design

The project welcomes submissions, but please let everyone know what you are working on.

Before undertaking a nontrivial change to this specification, send mail to the [mailing list](#mailing-list) to discuss what you plan to do.
This gives everyone a chance to validate the design, helps prevent duplication of effort, and ensures that the idea fits.
It also guarantees that the design is sound before code is written; a GitHub pull-request is not the place for high-level discussions.

Typos and grammatical errors can go straight to a pull-request.
When in doubt, start on the [mailing-list](#mailing-list).

## Weekly Call

The contributors and maintainers of all OCI projects have a weekly meeting Wednesdays at 2:00 PM (USA Pacific.)
Everyone is welcome to participate via [UberConference web][UberConference] or audio-only: 888-587-9088 or 860-706-8529 (no PIN needed.)
An initial agenda will be posted to the [mailing list](#mailing-list) earlier in the week, and everyone is welcome to propose additional topics or suggest other agenda alterations there.
Minutes are posted to the [mailing list](#mailing-list) and minutes from past calls are archived to the [wiki](https://github.com/opencontainers/runtime-spec/wiki) for those who are unable to join the call.

## Mailing List

You can subscribe and join the mailing list on [Google Groups](https://groups.google.com/a/opencontainers.org/forum/#!forum/dev).

## IRC

OCI discussion happens on #opencontainers on Freenode ([logs][irc-logs]).

## Markdown style

To keep consistency throughout the Markdown files in the Open Container spec all files should be formatted one sentence per line.
This fixes two things: it makes diffing easier with git and it resolves fights about line wrapping length.
For example, this paragraph will span three lines in the Markdown source.

## Git commit

### Sign your work

The sign-off is a simple line at the end of the explanation for the patch, which certifies that you wrote it or otherwise have the right to pass it on as an open-source patch.
The rules are pretty simple: if you can certify the below (from [developercertificate.org](http://developercertificate.org/)):

```
Developer Certificate of Origin
Version 1.1

Copyright (C) 2004, 2006 The Linux Foundation and its contributors.
660 York Street, Suite 102,
San Francisco, CA 94110 USA

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.


Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part
    by me, under the same open source license (unless I am
    permitted to submit under a different license), as indicated
    in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    personal information I submit with it, including my sign-off) is
    maintained indefinitely and may be redistributed consistent with
    this project or the open source license(s) involved.
```

then you just add a line to every git commit message:

    Signed-off-by: Joe Smith <joe@gmail.com>

using your real name (sorry, no pseudonyms or anonymous contributions.)

You can add the sign off when creating the git commit via `git commit -s`.

### Commit Style

Simple house-keeping for clean git history.
Read more on [How to Write a Git Commit Message](http://chris.beams.io/posts/git-commit/) or the Discussion section of [`git-commit(1)`](http://git-scm.com/docs/git-commit).

1. Separate the subject from body with a blank line
2. Limit the subject line to 50 characters
3. Capitalize the subject line
4. Do not end the subject line with a period
5. Use the imperative mood in the subject line
6. Wrap the body at 72 characters
7. Use the body to explain what and why vs. how
  * If there was important/useful/essential conversation or information, copy or include a reference
8. When possible, one keyword to scope the change in the subject (i.e. "README: ...", "runtime: ...")


[UberConference]: https://www.uberconference.com/opencontainers
[irc-logs]: http://ircbot.wl.linuxfoundation.org/eavesdrop/%23opencontainers/
