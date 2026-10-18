---
title: The Future of ghq
type: tech
published: true
---

# The Future of ghq

Finally, I'd like to close this document by writing about the future of ghq.

## Future features

Feature requests are accepted on GitHub Issues. Pull requests are of course welcome too.

With v1 released, I consider the feature set to have reached a resting point. I want it to remain a simple tool, so I'm not especially eager to add features. That said, and this is purely daydreaming, I have been thinking about things like the following:

- Improving `ghq list <query>`
    - Improving the confusing query syntax
    - Regular expressions, other filtering options, etc.
- JSON output, e.g. via `ghq list --json`
- Making `ghq list` even faster
    - Periodic caching by a background server, etc.

## Acknowledgments

To motemen, the original author: thank you for bringing such a wonderful tool into the world. By taking over its maintenance, I have also learned a great deal about Go.

Thanks to Go. The original idea came from `go get`, and `ghq` itself is written in Go; without Go, ghq would not be what it is today. To everyone on the Go team, thank you very much.

Thank you for the many pull requests. As of January 4, 2020, no fewer than 46 people have contributed to the codebase. My thanks also go to everyone who has filed issues.

Thank you for the many GitHub stars. As of January 4, 2020, the project has received 1,572 stars. I feel fortunate to be involved in maintaining an OSS project like this.

Thank you for the many articles introducing it. Thanks to introductions on blogs, Twitter, and many other places, `ghq` has gained many users.

Thank you to our sponsors. motemen, the original author, and I, the main maintainer, both have GitHub Sponsors pages, and several people are already sponsoring us.

Thanks to my family. This document was written on a sudden impulse during the 2019–2020 New Year holidays. My regular OSS activities also take place in my spare time, so they would not be possible without my family's understanding. Thank you so much.

## Contributing

Seen the other way around, the things mentioned in the acknowledgments above are ways to contribute. Just starring the project on GitHub or tweeting about it on Twitter is a big contribution.

Please also consider GitHub Sponsors or purchasing this document.

- https://github.com/sponsors/motemen
- https://github.com/sponsors/Songmu
- https://leanpub.com/ghq-handbook

In particular, if this document generates revenue, I would like to use it to consider things like outsourcing an English translation of this document, so your support is much appreciated.

Thank you for your continued support of `ghq`.
