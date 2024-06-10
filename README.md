# Homebrew for Linux ARM

> [!CAUTION] **This Fork**
> Since this is an unsupported hack, this may eventually break.

> [!NOTE] **How to use**
>
> All this fork does is _hack_ to allow ruby installed by rbenv to be used instead of `/usr/bin/ruby`:
> [GitHub commit - Allow ruby from rbenv to take precedence](https://github.com/sergeycherepanov/linuxbrew-arm/commit/18f7e3d5a54078201430fade8ab76fadd9b282ea):
>
> ```bash
> # filter the user environment
> PATH="/usr/bin:/bin:/usr/sbin:/sbin"
> PATH="${HOME}/.rbenv/shims:${PATH}"
>
> #FILTERED_ENV=()
> FILTERED_ENV=("RBENV_VERSION=${RBENV_VERSION-}")
> ```
>
>
> To install this Homebrew, you'll first have to install `rbenv` (e.g. Ubuntu 22.04's version from `apt` is ok):
>
> ```shell
> git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build  # apt's ruby-build is too out of date
> rbenv install 3.1.4
> rbenv shell 3.1.4
> export HOMEBREW_BREW_GIT_REMOTE=https://github.com/sergeycherepanov/linuxbrew-arm
> export HOMEBREW_DEVELOPER=1
> /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh | sed '532s/abort/warn/')"
> ```
>
> You'll also need to add the core tap back (nowadays deprecated):
>
> ```shell
> brew tap homebrew/core
> ```
>
> After installation, to run `brew` you should probably create a `brew` wrapper that contains:
>
> ```bash
> #!/bin/bash
> [[ -d ~/.rbenv/bin ]] && export PATH="~/.rbenv/bin:$PATH"
> if command -v rbenv &>/dev/null; then
>   export PATH="${PATH//$(rbenv root)\/shims:/}"
>   eval "$(rbenv init -)"
>   rbenv shell 3.1.4
> else
>   echo "Warning: rbenv not found" >&2
> fi
> HOMEBREW_BREW_GIT_REMOTE=https://github.com/sergeycherepanov/linuxbrew-arm HOMEBREW_DEVELOPER=1 exec /home/linuxbrew/.linuxbrew/bin/brew "$@"
> ```

> [!IMPORTANT] **Partial solution**
> Of course, this is only half the battle. Many official formulas won't work because they weren't
> written with Linux ARM in mind. (For example, as of 2023-08-26, installing python, which the rust
> formula requires, will fail.)

> [!WARNING] I'll try to regularly rebase my changes on top of the latest upstream commits, and then
> I'll force push the new rebased commits. So you might need to `reset --hard` after fetching
> updates for this repo

[![GitHub release](https://img.shields.io/github/release/Homebrew/brew.svg)](https://github.com/Homebrew/brew/releases)

Features, usage and installation instructions are [summarised on the homepage](https://brew.sh). Terminology (e.g. the difference between a Cellar, Tap, Cask and so forth) is [explained here](https://docs.brew.sh/Formula-Cookbook#homebrew-terminology).

## What Packages Are Available?

1. Type `brew formulae` for a list.
2. Or visit [formulae.brew.sh](https://formulae.brew.sh) to browse packages online.

## More Documentation

`brew help`, `man brew` or check [our documentation](https://docs.brew.sh/).

## Troubleshooting

First, please run `brew update` and `brew doctor`.

Second, read the [Troubleshooting Checklist](https://docs.brew.sh/Troubleshooting).

**If you don't read these it will take us far longer to help you with your problem.**

## Donations

Homebrew is a non-profit project run entirely by unpaid volunteers. We need your funds to pay for software, hardware and hosting around continuous integration and future improvements to the project. Every donation will be spent on making Homebrew better for our users.

Please consider a regular donation through [GitHub Sponsors](https://github.com/sponsors/Homebrew), [Open Collective](https://opencollective.com/homebrew) or [Patreon](https://www.patreon.com/homebrew). Homebrew is fiscally hosted by the [Open Source Collective](https://opencollective.com/opensource).

For questions about donations, including corporate giving, please email the Homebrew PLC at [plc@brew.sh](mailto:plc@brew.sh).

## Community

- [Homebrew/discussions (forum)](https://github.com/orgs/Homebrew/discussions)
- [@homebrew@fosstodon.org (Mastodon)](https://fosstodon.org/@homebrew)
- [@MacHomebrew (𝕏 (formerly known as Twitter))](https://twitter.com/MacHomebrew)

## Contributing

We'd love you to contribute to Homebrew. First, please read our [Contribution Guide](CONTRIBUTING.md) and [Code of Conduct](https://github.com/Homebrew/.github/blob/HEAD/CODE_OF_CONDUCT.md#code-of-conduct).

We explicitly welcome contributions from people who have never contributed to open-source before: we were all beginners once! We can help build on a partially working pull request with the aim of getting it merged. We are also actively seeking to diversify our contributors and especially welcome contributions from women from all backgrounds and people of colour.

A good starting point for contributing is to first [tap `homebrew/core`](https://docs.brew.sh/FAQ#can-i-edit-formulae-myself), then run `brew audit --strict` with some of the packages you use (e.g. `brew audit --strict wget` if you use `wget`) and read through the warnings. Try to fix them until `brew audit --strict` shows no results and [submit a pull request](https://docs.brew.sh/How-To-Open-a-Homebrew-Pull-Request). If no formulae you use have warnings you can run `brew audit --strict` without arguments to have it run on all packages and pick one.

Alternatively, for something more substantial, check out one of the issues labelled `help wanted` in [Homebrew/brew](https://github.com/homebrew/brew/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+wanted%22) or [Homebrew/homebrew-core](https://github.com/homebrew/homebrew-core/issues?q=is%3Aopen+is%3Aissue+label%3A%22help+wanted%22).

Good luck!

## Security

Please report security issues by filling in [the security advisory form](https://github.com/homebrew/brew/security/advisories/new).

## Who We Are

Homebrew's [Project Leader](https://docs.brew.sh/Homebrew-Governance#6-project-leader) is [Mike McQuaid](https://github.com/MikeMcQuaid).

Homebrew's [Project Leadership Committee](https://docs.brew.sh/Homebrew-Governance#4-project-leadership-committee) is [Colin Dean](https://github.com/colindean), [Issy Long](https://github.com/issyl0), [Mike McQuaid](https://github.com/MikeMcQuaid), [Patrick Linnane](https://github.com/p-linnane) and [Vanessa Gennarelli](https://github.com/mozzadrella).

Homebrew's [Technical Steering Committee](https://docs.brew.sh/Homebrew-Governance#7-technical-steering-committee) is [Bo Anderson](https://github.com/Bo98), [FX Coudert](https://github.com/fxcoudert), [Michka Popoff](https://github.com/iMichka), [Mike McQuaid](https://github.com/MikeMcQuaid) and [Rylan Polster](https://github.com/Rylan12).

Homebrew's maintainers are [Alexander Bayandin](https://github.com/bayandin), [Bevan Kay](https://github.com/bevanjkay), [Bo Anderson](https://github.com/Bo98), [Branch Vincent](https://github.com/branchvincent), [Caleb Xu](https://github.com/alebcay), [Carlo Cabrera](https://github.com/carlocab), [David Baumgold](https://github.com/singingwolfboy), [Douglas Eichelberger](https://github.com/dduugg), [Dustin Rodrigues](https://github.com/dtrodrigues), [Eric Knibbe](https://github.com/EricFromCanada), [FX Coudert](https://github.com/fxcoudert), [Issy Long](https://github.com/issyl0), [Justin Krehel](https://github.com/krehel), [Markus Reiter](https://github.com/reitermarkus), [Miccal Matthews](https://github.com/miccal), [Michael Cho](https://github.com/cho-m), [Michka Popoff](https://github.com/iMichka), [Mike McQuaid](https://github.com/MikeMcQuaid), [Nanda H Krishna](https://github.com/nandahkrishna), [Patrick Linnane](https://github.com/p-linnane), [Razvan Azamfirei](https://github.com/razvanazamfirei), [Rui Chen](https://github.com/chenrui333), [Ruoyu Zhong](https://github.com/ZhongRuoyu), [Rylan Polster](https://github.com/Rylan12), [Sam Ford](https://github.com/samford), [Sean Molenaar](https://github.com/SMillerDev), [Thierry Moisan](https://github.com/Moisan), [Timothy Sutton](https://github.com/timsutton) and [William Woodruff](https://github.com/woodruffw).

Former maintainers with significant contributions include [Misty De Méo](https://github.com/mistydemeo), [Shaun Jackman](https://github.com/sjackman), [Vítor Galvão](https://github.com/vitorgalvao), [Claudia Pellegrino](https://github.com/claui), [Seeker](https://github.com/SeekingMeaning), [Jan Viljanen](https://github.com/javian), [JCount](https://github.com/jcount), [commitay](https://github.com/commitay), [Dominyk Tiller](https://github.com/DomT4), [Tim Smith](https://github.com/tdsmith), [Baptiste Fontaine](https://github.com/bfontaine), [Xu Cheng](https://github.com/xu-cheng), [Martin Afanasjew](https://github.com/UniqMartin), [Brett Koonce](https://github.com/asparagui), [Charlie Sharpsteen](https://github.com/Sharpie), [Jack Nagel](https://github.com/jacknagel), [Adam Vandenberg](https://github.com/adamv), [Andrew Janke](https://github.com/apjanke), [Alex Dunn](https://github.com/dunn), [neutric](https://github.com/neutric), [Tomasz Pajor](https://github.com/nijikon), [Uladzislau Shablinski](https://github.com/vladshablinsky), [Alyssa Ross](https://github.com/alyssais), [ilovezfs](https://github.com/ilovezfs), [Chongyu Zhu](https://github.com/lembacon) and Homebrew's creator: [Max Howell](https://github.com/mxcl).

## License

Code is under the [BSD 2-clause "Simplified" License](LICENSE.txt).
Documentation is under the [Creative Commons Attribution license](https://creativecommons.org/licenses/by/4.0/).

## Sponsors

Our macOS continuous integration infrastructure is hosted by [MacStadium's Orka](https://www.macstadium.com/customers/homebrew).

[![Powered by MacStadium](https://cloud.githubusercontent.com/assets/125011/22776032/097557ac-eea6-11e6-8ba8-eff22dfd58f1.png)](https://www.macstadium.com)

Secure password storage and syncing is provided by [1Password for Teams](https://1password.com/teams/).

[<img src="https://i.1password.com/akb/featured/1password-icon.svg" alt="1Password" height="64">](https://1password.com)

Flaky test detection and tracking is provided by [BuildPulse](https://buildpulse.io/).

[![BuildPulse](https://github.com/Homebrew/brew/assets/1699443/87385e9a-6c47-4e59-b17e-fe083e945709)](https://buildpulse.io)

<https://brew.sh>'s DNS is [resolving with DNSimple](https://dnsimple.com/resolving/homebrew).

[![DNSimple](https://cdn.dnsimple.com/assets/resolving-with-us/logo-light.png)](https://dnsimple.com/resolving/homebrew#gh-light-mode-only)
[![DNSimple](https://cdn.dnsimple.com/assets/resolving-with-us/logo-dark.png)](https://dnsimple.com/resolving/homebrew#gh-dark-mode-only)

Homebrew is generously supported by [GitHub](https://github.com/github), [Custom Ink](https://github.com/customink), [Randy Reddig](https://github.com/ydnar), [Codecademy](https://github.com/Codecademy), [MacPaw Inc.](https://github.com/MacPaw), [fly.io](https://github.com/superfly), [Workbrew](https://github.com/Workbrew) and many other users and organisations via [GitHub Sponsors](https://github.com/sponsors/Homebrew).

[![GitHub](https://github.com/github.png?size=64)](https://github.com/github)[![fly.io](https://github.com/superfly.png?size=64)](https://github.com/superfly)
