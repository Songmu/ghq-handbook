# The future of `ghq`

I would like to conclude this document by writing about the future of `ghq`.

## Feature additions

Requests for additional features are accepted through GitHub Issues. Of course, pull requests are also welcome.

We have released `v1` as well, and we believe that the core functionality has been fully implemented. I want to keep the tool simple, so I'm not too aggressive about adding features. However, although it may be a pipe dream, I have considered the following as potentially useful additions:

- Enhanced `ghq list <query>`
    - Improved ambiguous query specification
    - Specify regular expressions or other filtering options, etc.
- JSON output a la `ghq list --json`, etc.
- Performance improvements to `ghq list`
    - Caching entries via background server, etc.

## Acknowledgments

Thank you to the original author, motemen, for bringing such a wonderful tool to the world. I also learned a lot about Go by taking over the maintenance of this tool.

Thanks to the Go programming language. Since the original idea came from `go get` and `ghq` itself is written in Go, `ghq` wouldn't be what it is today without Go. Many thanks to the Go team.

Thank you for many pull requests. As of January 4, 2020, a whopping 46 people have contributed to the codebase. We would also like to thank those who raised issues.

Thank you for the many GitHub stars. As of January 4, 2020, we have received 1,572 stars. I am grateful to be involved in the maintenance of OSS like this.

Thank you for many introductory articles. Thanks to introductions in various places throughout the web such as blogs and Twitter, `ghq` was able to gain many users.

Thank you for your sponsorship. Motemen, the original author, and I, the main maintainer, have set up GitHub Sponsors, and some people have already sponsored us.

Thanks to my family. This document was written on a whim during the 2019-2020 New Year holidays. In addition, since my OSS contributions are typically worked on during leisure time, they could not be carried out without the understanding and support of my family. Thank you very much.

## Contributions

What was written in the acknowledgments of the previous section will lead to contributions if you pay it forward. Just starring us on GitHub or tweeting us on Twitter would be a huge contribution.

Please consider sponsoring us on GitHub or purchasing this document:

- https://github.com/sponsors/motemen
- https://github.com/sponsors/Songmu
- https://leanpub.com/ghq-handbook


Thank you for your continued support of `ghq`.
