# EPMP

Some new features of EasyRPG Player, primarily for multiplayer.

Not sure how to use it? You can check out [Tutorial].


## Repository

The first commit of EPMP is [Initial commit], based on [ynoclient:cd7b46d](https://github.com/ynoproject/ynoengine/tree/cd7b46d) 2023-05-31.

If you are interested in this project, you can clone the repository to your local system to browse the code. If you don't know how to use Git, refer to: [Git Manual](https://git-scm.com/book/en/v2).

- Use `git log --author=MonokoTech` to see all epmp commits

  Add `-p` to show changes

  Add `--name-status` to show which files changed


## Library dependency

- [libuv] for networking
- [libsodium] for encryption


## Credits

### Powered by

- [EasyRPG](https://github.com/EasyRPG): Free, open-source RPG creation tool

### \[Fork-02 from twig33-ynoclient\] [ynop-ynoengine](https://github.com/ynoproject/ynoengine) (2024-03)

- [Flashfyre (sam)](https://github.com/Flashfyre): Synchronization, Web, ChatName (NameTag), ...
- [aleck099](https://github.com/aleck099): New YNO protocol, Connection and data, Async download, ...
- [maru (pancakes)](https://github.com/patapancakes): Merge upstream, Handle merge conflicts, Web, ChatName, ...

### \[Fork-01 from twig33-ynoclient\] [Char0x61-ynoclient](https://github.com/CataractJustice/ynoclient) (2022-06)

- [Char0x61 (CataractJustice)](https://github.com/CataractJustice): Split & Improve, Char0x61's nametags, Settings Scene, ...
- [xiaodao (苏半岛)](https://github.com/lychees): In-game chat translation, Font bug fix, ...
- [Led (Biel Borel)](https://github.com/Ledgamedev): In-game chat original implementation, ...

### \[Original\] [twig33-ynoclient](https://github.com/twig33/ynoclient) (2021-11)

- [twig33](https://github.com/twig33): Original concept and implementation

### Additional thanks

- [Jixun](https://github.com/jixunmoe) for helping in the C++ problems. ([jixun-forloop])
- [Ratizux](https://github.com/Ratizux) for the podman suggestions
- [Proselyte093](https://github.com/Proselyte093) for giving the project a chance to compile on the macOS ARM
- AI for various types of knowledge
- With help from various participants

### Memo

[JeDaYoshi](https://github.com/JeDaYoshi), [kekami](https://kekami.dev), azarashi, Altiami


## License

For EPMP licensing, see [epmp-license.txt]

### 3rd party software

EPMP makes use of the following 3rd party software:

* [minetest util] Minetest C/C++ code - Copyright (C) 2010 celeron55,
  Perttu Ahola \<celeron55@gmail.com\>, provided under the LGPLv2.1+
* [socks5.h] Modern C++ SOCKS5 Client Handler - by harsath, provided under MIT

[Initial commit]: https://github.com/MonokoTech/EasyRPG-Player-Monoko/tree/href-initial-commit
[Tutorial]: /docs/EPMP/Tutorial.md
[libuv]: https://github.com/libuv/libuv
[libsodium]: https://github.com/jedisct1/libsodium
[jixun-forloop]: https://github.com/MonokoTech/EasyRPG-Player-Monoko/blob/href-jixun-forloop/src/multiplayer/game_multiplayer.cpp#L1333
[epmp-license.txt]: /docs/EPMP/license.txt
[minetest util]: https://github.com/minetest/minetest
[socks5.h]: https://github.com/harsath/SOCKS5-Proxy-Handler
