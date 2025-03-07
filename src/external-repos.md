# Using External Repositories

The `rust-lang/rust` git repository depends on several other repos in the `rust-lang` organization.
There are three main ways we use dependencies:
1. As a Cargo dependency through crates.io (e.g. `rustc-rayon`)
2. As a git subtree (e.g. `clippy`)
3. As a git submodule (e.g. `cargo`)

As a general rule, use crates.io for libraries that could be useful for others in the ecosystem; use
subtrees for tools that depend on compiler internals and need to be updated if there are breaking
changes; and use submodules for tools that are independent of the compiler.

## External Dependencies (subtree)

As a developer to this repository, you don't have to treat the following external projects
differently from other crates that are directly in this repo:

* [Clippy](https://github.com/rust-lang/rust-clippy)
* [Miri]
* [rustfmt](https://github.com/rust-lang/rustfmt)
* [rust-analyzer](https://github.com/rust-lang/rust-analyzer)

In contrast to `submodule` dependencies
(see below for those), the `subtree` dependencies are just regular files and directories which can
be updated in tree. However, if possible, enhancements, bug fixes, etc. specific
to these tools should be filed against the tools directly in their respective
upstream repositories. The exception is that when rustc changes are required to
implement a new tool feature or test, that should happen in one collective rustc PR.

### Synchronizing a subtree

Periodically the changes made to subtree based dependencies need to be synchronized between this
repository and the upstream tool repositories.

Subtree synchronizations are typically handled by the respective tool maintainers. Other users
are welcome to submit synchronization PRs, however, in order to do so you will need to modify
your local git installation and follow a very precise set of instructions.
These instructions are documented, along with several useful tips and tricks, in the
[syncing subtree changes][clippy-sync-docs] section in Clippy's Contributing guide.
The instructions are applicable for use with any subtree based tool, just be sure to
use the correct corresponding subtree directory and remote repository.

The synchronization process goes in two directions: `subtree push` and `subtree pull`.

A `subtree push` takes all the changes that happened to the copy in this repo and creates commits
on the remote repo that match the local changes. Every local
commit that touched the subtree causes a commit on the remote repo, but
is modified to move the files from the specified directory to the tool repo root.

A `subtree pull` takes all changes since the last `subtree pull`
from the tool repo and adds these commits to the rustc repo along with a merge commit that moves
the tool changes into the specified directory in the Rust repository.

It is recommended that you always do a push first and get that merged to the tool master branch.
Then, when you do a pull, the merge works without conflicts.
While it's definitely possible to resolve conflicts during a pull, you may have to redo the conflict
resolution if your PR doesn't get merged fast enough and there are new conflicts. Do not try to
rebase the result of a `git subtree pull`, rebasing merge commits is a bad idea in general.

You always need to specify the `-P` prefix to the subtree directory and the corresponding remote
repository. If you specify the wrong directory or repository
you'll get very fun merges that try to push the wrong directory to the wrong remote repository.
Luckily you can just abort this without any consequences by throwing away either the pulled commits
in rustc or the pushed branch on the remote and try again. It is usually fairly obvious
that this is happening because you suddenly get thousands of commits that want to be synchronized.

[clippy-sync-docs]: https://doc.rust-lang.org/nightly/clippy/development/infrastructure/sync.html

### Creating a new subtree dependency

If you want to create a new subtree dependency from an existing repository, call (from this
repository's root directory!)

```
git subtree add -P src/tools/clippy https://github.com/rust-lang/rust-clippy.git master
```

This will create a new commit, which you may not rebase under any circumstances! Delete the commit
and redo the operation if you need to rebase.

Now you're done, the `src/tools/clippy` directory behaves as if Clippy were
part of the rustc monorepo, so no one but you (or others that synchronize
subtrees) actually needs to use `git subtree`.


## External Dependencies (submodules)

Building Rust will also use external git repositories tracked using [git
submodules]. The complete list may be found in the [`.gitmodules`] file. Some
of these projects are required (like `stdarch` for the standard library) and
some of them are optional (like [Miri]).

Usage of submodules is discussed more in the [Using Git
chapter](git.md#git-submodules).

Some of the submodules are allowed to be in a "broken" state where they
either don't build or their tests don't pass, e.g. the documentation books
like [The Rust Reference]. Maintainers of these projects will be notified
when the project is in a broken state, and they should fix them as soon
as possible. The current status is tracked on the [toolstate website].
More information may be found on the Forge [Toolstate chapter].

Breakage is not allowed in the beta and stable channels, and must be addressed
before the PR is merged. They are also not allowed to be broken on master in
the week leading up to the beta cut.

[git submodules]: https://git-scm.com/book/en/v2/Git-Tools-Submodules
[`.gitmodules`]: https://github.com/rust-lang/rust/blob/master/.gitmodules
[The Rust Reference]: https://github.com/rust-lang/reference/
[toolstate website]: https://rust-lang-nursery.github.io/rust-toolstate/
[Toolstate chapter]: https://forge.rust-lang.org/infra/toolstate.html

### Breaking Tools Built With The Compiler

Rust's build system builds a number of tools that make use of the internals of
the compiler and that are hosted in a separate repository, and included in Rust
via git submodules (such as [Miri]). If these tools break because of your
changes, you may run into a sort of "chicken and egg" problem. These tools rely
on the latest compiler to be built so you can't update them (in their own
repositories) to reflect your changes to the compiler until those changes are
merged into the compiler. At the same time, you can't get your changes merged
into the compiler because the rust-lang/rust build won't pass until those tools
build and pass their tests.

Luckily, a feature was
[added to Rust's build](https://github.com/rust-lang/rust/issues/45861) to make
all of this easy to handle. The idea is that we allow these tools to be
"broken", so that the rust-lang/rust build passes without trying to build them,
then land the change in the compiler, and go update the tools that you
broke. Some tools will require waiting for a nightly release before this can
happen, while others use the builds uploaded after each bors merge and thus can
be updated immediately (check the tool's documentation for details). Once you're
done and the tools are working again, you go back in the compiler and update the
tools so they can be distributed again.

This should avoid a bunch of synchronization dances and is also much easier on contributors as
there's no need to block on tools changes going upstream.

Here are those same steps in detail:

1. (optional) First, if it doesn't exist already, create a `config.toml` by copying
   `config.example.toml` in the root directory of the Rust repository.
   Set `submodules = false` in the `[build]` section. This will prevent `x.py`
   from resetting to the original branch after you make your changes. If you
   need to [update any submodules to their latest versions](#updating-submodules),
   see the section of this file about that for more information.
2. (optional) Run `./x.py test src/tools/cargo` (substituting the submodule
   that broke for `cargo`). Fix any errors in the submodule (and possibly others).
3. (optional) Make commits for your changes and send them to upstream repositories as a PR.
4. (optional) Maintainers of these submodules will **not** merge the PR. The PR can't be
   merged because CI will be broken. You'll want to write a message on the PR referencing
   your change, and how the PR should be merged once your change makes it into a nightly.
5. Wait for your PR to merge.
6. Wait for a nightly.
7. (optional) Help land your PR on the upstream repository now that your changes are in nightly.
8. (optional) Send a PR to rust-lang/rust updating the submodule.

