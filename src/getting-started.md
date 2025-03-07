# Getting Started

Thank you for your interest in contributing to Rust! There are many ways to
contribute, and we appreciate all of them.

<!-- toc -->

If this is your first time contributing, the [walkthrough] chapter can give you a good example of
how a typical contribution would go.

This documentation is _not_ intended to be comprehensive; it is meant to be a
quick guide for the most useful things. For more information, [see this
chapter on how to build and run the compiler](./building/how-to-build-and-run.md).

[internals]: https://internals.rust-lang.org
[rust-discord]: http://discord.gg/rust-lang
[rust-zulip]: https://rust-lang.zulipchat.com
[coc]: https://www.rust-lang.org/conduct.html
[walkthrough]: ./walkthrough.md
[Getting Started]: ./getting-started.md

## Asking Questions

If you have questions, please make a post on [internals.rust-lang.org][internals] or
hop on the [Rust Discord server][rust-discord] or [Rust Zulip server][rust-zulip].

As a reminder, all contributors are expected to follow our [Code of Conduct][coc].

The compiler team (or `t-compiler`) usually hangs out in Zulip [in this
"stream"][z]; it will be easiest to get questions answered there.

[z]: https://rust-lang.zulipchat.com/#narrow/stream/131828-t-compiler

**Please ask questions!** A lot of people report feeling that they are "wasting
expert time", but nobody on `t-compiler` feels this way. Contributors are
important to us.

Also, if you feel comfortable, prefer public topics, as this means others can
see the questions and answers, and perhaps even integrate them back into this
guide :)

### Experts

Not all `t-compiler` members are experts on all parts of `rustc`; it's a pretty
large project.  To find out who has expertise on different parts of the
compiler, [consult this "experts map"][map].

It's not perfectly complete, though, so please also feel free to ask questions
even if you can't figure out who to ping.

[map]: https://github.com/rust-lang/compiler-team/blob/master/content/experts/map.toml

### Etiquette

We do ask that you be mindful to include as much useful information as you can
in your question, but we recognize this can be hard if you are unfamiliar with
contributing to Rust.

Just pinging someone without providing any context can be a bit annoying and
just create noise, so we ask that you be mindful of the fact that the
`t-compiler` folks get a lot of pings in a day.

## Cloning and Building

See ["How to build and run the compiler"](./building//how-to-build-and-run.md).

## Contributing code to other Rust projects

There are a bunch of other projects that you can contribute to outside of the
`rust-lang/rust` repo, including `clippy`, `miri`, `chalk`, and many others.

These repos might have their own contributing guidelines and procedures. Many
of them are owned by working groups (e.g. `chalk` is largely owned by
WG-traits). For more info, see the documentation in those repos' READMEs.

## Other ways to contribute

There are a bunch of other ways you can contribute, especially if you don't
feel comfortable jumping straight into the large `rust-lang/rust` codebase.

The following tasks are doable without much background knowledge but are
incredibly helpful:

- [Cleanup crew][iceb]: find minimal reproductions of ICEs, bisect
  regressions, etc. This is a way of helping that saves a ton of time for
  others to fix an error later.
- [Writing documentation][wd]: if you are feeling a bit more intrepid, you could try
  to read a part of the code and write doc comments for it. This will help you
  to learn some part of the compiler while also producing a useful artifact!
- [Triaging issues][triage]: categorizing, replicating, and minimizing issues is very helpful to the Rust maintainers.
- [Working groups][wg]: there are a bunch of working groups on a wide variety
  of rust-related things.
- Answer questions in the _Get Help!_ channels on the [Rust Discord
  server][rust-discord], on [users.rust-lang.org][users], or on
  [StackOverflow][so].
- Participate in the [RFC process](https://github.com/rust-lang/rfcs).
- Find a [requested community library][community-library], build it, and publish
  it to [Crates.io](http://crates.io). Easier said than done, but very, very
  valuable!

[rust-discord]: https://discord.gg/rust-lang
[users]: https://users.rust-lang.org/
[so]: http://stackoverflow.com/questions/tagged/rust
[community-library]: https://github.com/rust-lang/rfcs/labels/A-community-library

[iceb]: ./notification-groups/cleanup-crew.md
[wd]: ./contributing.md#writing-documentation
[wg]: https://rust-lang.github.io/compiler-team/working-groups/
[triage]: ./contributing.md#issue-triage

## Contributor Procedures

This section has moved to the ["Contribution Procedures"](./contributing.md) chapter.

## Other Resources

This section has moved to the ["About this guide"][more-links] chapter.

[more-links]: ./about-this-guide.md#other-places-to-find-information
