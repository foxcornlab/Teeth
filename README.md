<p align="center">
<a href="https://github.com/foxcornlab/Teeth"><img width="280" height="280" src="img/johntheripper.png"></a>
</p>

<h2 align="center">Teeth</h2>

<p align="center">
<a href="https://github.com/foxcornlab/Teeth/actions"><img src="https://img.shields.io/github/actions/workflow/status/foxcornlab/Teeth/ci.yml?label=build"></a>
<a href="LICENSE-MAIN.txt"><img src="https://img.shields.io/badge/License-GPL--2.0-blue.svg"></a>
<img src="https://img.shields.io/badge/Engine-John--1.9.0--jumbo-orange.svg">
</p>

Teeth is a maintained fork of **John the Ripper jumbo** (bleeding), repackaged with sane defaults, extra converter tooling, and bundled wordlist/rule presets for fast password-auditing work. One binary, hundreds of hash formats, dictionary / brute-force / single / incremental / mask cracking modes, OpenMP + OpenCL + CUDA acceleration.

Use it to audit *your own* password hashes, test password-policy compliance, and recover lost credentials you own.

# `Features`

* Full John the Ripper 1.9.0-jumbo-1 bleeding engine: 400+ hash and cipher formats
* `single`, `wordlist`, `incremental`, `mask`, and external modes in one binary
* OpenMP, OpenCL and CUDA builds; session resume with `--restore`
* Bundled `rules/` scripts: `*2john` extractors for archives, keychains, password managers, DPAPI, RADIUS, 1Password, Android OTP and more
* Bundled wordlists and rule presets under `rules/` (including BIP-39 lists)
* `ThinkPwn` UEFI-driver scanner under `config/tp.py` (Cr4sh/trufae/p3xsouger, see credits)

# `Build`

```bash
cd src
./configure && make -sj$(nproc)
```

Binaries land in `run/` (build output, gitignored). Copy the `john` binary next to
the configs — john resolves `john.conf` and its `rules/`, `opencl/` etc. from its
own directory:

```bash
cp run/john* rules/
cd rules
./john --list=formats | head
./john --wordlist=password.lst hashes.txt
./john --show hashes.txt
```

Need OpenCL/CUDA? Install the vendor SDK first, then `./configure` picks it up automatically. Run `./configure --help` in `src/` for the full build matrix.

# `Usage`

```bash
./john --wordlist=rules/password.lst --rules target.hashes
./john --single target.hashes
./john --incremental --format=raw-sha256 target.hashes
./john --show --format=raw-md5 target.hashes
```

Extract hashes from locked files first:

```bash
python3 rules/7z2john.pl vault.7z > vault.hash
python3 rules/1password2john.py vault.agilekeychain > op.hash
```

Run `./john` with no arguments for the full option list.

# `Repo` `layout`

```
src/             john the ripper core (upstream openwall, unmodified)
run/             build output (gitignored, created by make)
rules/           runtime dir: john.conf, *2john extractors, .chr tables,
                 wordlists, opencl kernels, rule presets (upstream run/)
rules/bip/       bip-0039 wordlists
config/tp.py     thinkpwn uefi scanner
img/             branding assets
```

After building, `cp run/john* rules/` — the binary must sit next to `john.conf`.
# `Thanks` `to`

* `Solar Designer` and the Openwall crew — john the ripper is their work, this fork rides on it
* `magnumripper` and every jumbo contributor
* `Cr4sh`, `trufae`, `p3xsouger` — thinkpwn scanner
* `k4amos` — radius2john

# `Disclaimer`

Educational and authorised-audit use only. Only run this against hashes you own or have explicit written permission to test. Misuse is on you.

> Research by `Vineeth Kumar` (`foxcornlab`). Found a bug or want a feature, open an `issue`.
