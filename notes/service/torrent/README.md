---
title: Torrent
tags:
  - Torrent
  - Reference
---

# Torrent

- .torrent
- Magnet Link
  - 替代 .torrent 文件，通过 URI 表示共享的文件信息。
- Tracker
  - Peer Protocol
  - HTTP Tracker
  - UDP Tracker Protocol
  - Distributed Hash Table (DHT)
    - Kademlia
- P2P Protocol
  - Handshake：初次连接时交换信息。
  - Piece：交换文件数据。
  - Have：通知拥有的数据块。
- PEX - Peer Exchange
  - 让节点之间共享已知的 Peer 列表，减少对 Tracker 的依赖。
- 分片（Piece）机制
  - 将大文件分成小片段（Piece），实现高效的并行下载。
  - 每个文件被分成多个 Piece，每个 Piece 都有一个 SHA-1 哈希值。
  - Peer 下载时验证每个 Piece 的完整性。
- 超种（Super-Seeding）模式
  - 提高 Seeder 初次分发文件的效率。
  - Seeder 优先将不同的 Piece 传给不同的 Peer，减少重复传输。
  - 适用于文件刚发布、节点数较少的场景。
- 扩展
  - uTP（Micro Transport Protocol）：
    - 基于 UDP 的传输协议，减少网络拥塞。
  - Fast Extension：
    - 提高下载速度的功能，例如允许 Peer 请求跳跃式的 Piece。
  - Peer Identification：
    - 识别 Peer 的下载客户端（如 μTorrent、qBittorrent）。

| Spec    | Title                                                            | Status |
| ------- | ---------------------------------------------------------------- | ------ |
| BEP03   | The BitTorrent Protocol Specification                            | 🟢     |
| BEP04   | Known Number Allocations                                         | 🟢     |
| BEP05   | DHT Protocol                                                     | ✅     |
| BEP06   | Fast Extension                                                   | ✅     |
| BEP07   | IPv6 Tracker Extension                                           | 📝     |
| BEP08   | Tracker Peer Obfuscation                                         | 🟡     |
| [BEP09] | Extension for Peers to Send Metadata Files                       | ✅     |
| BEP10   | Extension Protocol                                               | ✅     |
| BEP11   | Peer Exchange (PEX)                                              | ✅     |
| BEP12   | Multitracker Metadata Extension                                  | ✅     |
| BEP14   | Local Service Discovery                                          | ✅     |
| BEP15   | UDP Tracker Protocol                                             | ✅     |
| BEP16   | Superseeding                                                     | 📝     |
| BEP17   | HTTP Seeding (Hoffman-style)                                     | 📝     |
| BEP18   | Search Engine Specification                                      | 🟡     |
| BEP19   | WebSeed - HTTP/FTP Seeding (GetRight-style)                      | ✅     |
| BEP20   | Peer ID Conventions                                              | 🟢     |
| BEP21   | Extension for Partial Seeds                                      | 📝     |
| BEP22   | BitTorrent Local Tracker Discovery Protocol                      | 🟡     |
| [BEP23] | Tracker Returns Compact Peer Lists                               | ✅     |
| BEP24   | Tracker Returns External IP                                      | 📝     |
| BEP26   | Zeroconf Peer Advertising and Discovery                          | 🟡     |
| BEP27   | Private Torrents                                                 | ✅     |
| BEP28   | Tracker exchange                                                 | 🟡     |
| BEP29   | uTorrent transport protocol                                      | ✅     |
| BEP30   | Merkle tree torrent extension                                    | 📝     |
| BEP31   | Tracker Failure Retry Extension                                  | 📝     |
| BEP32   | IPv6 extension for DHT                                           | 📝     |
| BEP33   | DHT scrape                                                       | 📝     |
| BEP34   | DNS Tracker Preferences                                          | 📝     |
| BEP35   | Torrent Signing                                                  | 📝     |
| BEP36   | Torrent RSS feeds                                                | 📝     |
| BEP38   | Finding Local Data Via Torrent File Hints                        | 📝     |
| BEP39   | Updating Torrents Via Feed URL                                   | 📝     |
| BEP40   | Canonical Peer Priority                                          | 📝     |
| BEP41   | UDP Tracker Protocol Extensions                                  | 📝     |
| BEP42   | DHT Security Extension                                           | 📝     |
| BEP43   | Read-only DHT Nodes                                              | 📝     |
| BEP44   | Storing arbitrary data in the DHT                                | 📝     |
| BEP45   | Multiple-address operation for the BitTorrent DHT                | 📝     |
| BEP46   | Updating Torrents Via DHT Mutable Items                          | 📝     |
| BEP47   | Padding files and extended file attributes                       | 📝     |
| BEP48   | Tracker Protocol Extension: Scrape                               | 📝     |
| BEP49   | Distributed Torrent Feeds                                        | 📝     |
| BEP50   | Publish/Subscribe Protocol                                       | 📝     |
| BEP51   | DHT Infohash Indexing                                            | 📝     |
| [BEP52] | The BitTorrent Protocol Specification v2                         | 📝     |
| BEP53   | Magnet URI extension - Select specific file indices for download | 📝     |
| BEP54   | The lt_donthave extension                                        | 📝     |
| BEP55   | Holepunch extension                                              | ✅     |
| BEP1000 | Pending Standards Track Documents                                | 🟢     |

> 🟢 - Final & Active
> ✅ - Accepted
> 📝 - Draft
> 🟡 - Deferred

- [BEPS](http://bittorrent.org/beps/bep_0000.html)
- https://wiki.theory.org/index.php/BitTorrentSpecification
- https://wiki.wireshark.org/BitTorrent
- http://rakjar.de/gnuticles/MAGMA-Specsv22.txt
  - MAGnet MAnifest

[bep09]: ./magnet.md
[bep23]: http://bittorrent.org/beps/bep_0023.html
[bep52]: http://bittorrent.org/beps/bep_0052.html

## BEP03

**.torrent**

- bencode meta file

```json title="metainfo.json"
{
  "announce": "",
  "announce-list": [[""]],
  "comment": "",
  "created by": "",
  "creation date": 0,
  "encoding": "UTF-8",
  "info": {
    "name": "file",
    "piece length": 262144,
    "pieces": [[]],
    "length": 0,
    "files": [
      {
        "length": 0,
        "path": ["file.mkv"]
      }
    ]
  },
  // 非标准字段
  "nodes": [["", 0]],
  "saved by": "",
  "save date": 0,
  "hash": ""
}
```

- announce 有时为 数组
- info 为主要内容 - infohash 基于 sha1(info)
  - piece length
    - 源文件切分块大小
      - 2^18 = 256 K - BitTorrent 3.2+
      - 2^20 = 1 M
  - pieces - `[][]byte`
    - sh1 数组 - sha1 长为 20
    - md4
  - length - 单文件时存在
  - files - 目录时存在
    - path
    - length

:::caution

- piece 可能包含多文件 - 有的客户端会生成 padding 文件

:::

## BEP05 DHT

- router.bittorrent.com

## BEP30 Merkle hash torrent extension

- SHA1

## BEP48

Scrape for HTTP Tracker

```http
GET http://tracker/scrape?info_hash=xxxxxxxxxxxxxxxxxxxx&info_hash=yyyyyyyyyyyyyyyyyyyy
```

```pre title="bencode"
d5:filesd20:xxxxxxxxxxxxxxxxxxxxd8:completei11e10:downloadedi13772e10:incompletei19e
20:yyyyyyyyyyyyyyyyyyyyd8:completei21e10:downloadedi206e10:incompletei20eee
```

```json
{
  "files": {
    "xxxxxxxxxxxxxxxxxxxx": { "complete": 11, "downloaded": 13772, "incomplete": 19 },
    "yyyyyyyyyyyyyyyyyyyy": { "complete": 21, "downloaded": 206, "incomplete": 20 }
  }
}
```

## BEP52 BitTorrent v2

- metainfo
  - announce
  - info
    - name
    - piece length
    - meta version=2
      - v1 不会设置
    - file tree
      - `<path>` - UTF-8, 不能有 . 和 .., root 必须是目录
        - 文件包含 空 key
        - length - 文件长度
        - pieces root
          - root hash of a merkle tree with a branching factor of 2
          - 32 bytes
          - N\*16KiB
          - SHA2-256
    - files - 兼容 v1
      - attr=p 为 padding 文件
      - length
      - path
  - piece layers
    - root -> tree

---

- infohash - sha2-256
  - 有时作为 torrent id 会截取 20 bytes

---

- [bittorrent-v2](https://newshimalaya.com/2020/09/07/bittorrent-v2)
  - SHA-256 File Hash - 之前 sha-1
  - Hash Tree - 可以用 目录 hash 找文件 - v1 文件多的时候非常慢，包含所有 hash
    - 16 kiB 块 hash
  - 文件 Hash - 相同复用，会有 Pad 补齐块
  - 磁力链接支持 v2 btmh - `magnet:?xt=urn:btmh: &dn=&tr=`
    - [multiformats/multihash](https://github.com/multiformats/multihash)
- https://blog.libtorrent.org/2020/09/bittorrent-v2/
- [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree)
