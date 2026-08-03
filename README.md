# Consistent Hashing Demo

An interactive browser demo that explains how consistent hashing assigns keys to servers, and why only a small number of keys move when servers are added or removed.

The demo is inspired by the consistent hashing concept commonly discussed in system design interviews and books such as Alex Xu's *System Design Interview*.

## What It Shows

- A circular hash ring from `0` to `359`
- Physical cache servers such as `Cache A`, `Cache B`, and `Cache C`
- Virtual nodes for each physical server
- Sample keys such as users, carts, sessions, and photos
- Which server owns each key
- How many keys move after the server topology changes

## Why Consistent Hashing Matters

With simple modulo hashing, a key is often assigned like this:

```text
server = hash(key) % number_of_servers
```

That works until the number of servers changes. If a server is added or removed, many keys may be remapped to different servers.

Consistent hashing reduces that disruption by placing both servers and keys on the same hash ring. A key belongs to the first server found while moving clockwise around the ring.

When a server is added, only keys in the ranges newly claimed by that server move. When a server is removed, only that server's keys move to the next clockwise server.

## How To Run

Open `consistent-hashing-demo.html` in any modern browser.

No build step, package install, or backend server is required.

## How To Use The Demo

1. Open the demo in your browser.
2. Look at the circular ring.
3. Notice the colored server ranges and the key markers.
4. Click **Add server** and watch the **Keys moved** counter.
5. Click **Remove server** and notice that only some keys move.
6. Adjust **Virtual nodes per server** to see how virtual nodes improve distribution.
7. Click **Shuffle keys** to try a different set of sample keys.

## Important Concepts

### Hash Ring

The ring represents the available hash space. In this demo, the hash space is simplified to `0` through `359`, like degrees on a circle.

### Keys

Keys represent items that need to be stored or routed, such as:

- User IDs
- Session IDs
- Cache keys
- Image IDs
- Shopping carts

Each key is hashed to a position on the ring.

### Servers

Servers are also hashed to positions on the ring. A key is assigned to the first server found while moving clockwise from the key's hash position.

### Virtual Nodes

Virtual nodes are multiple ring positions for the same physical server.

They help distribute keys more evenly. Without virtual nodes, one server may accidentally own a very large part of the ring while another owns very little.

## What Affects What

| Action | Effect |
| --- | --- |
| Add a server | The new server claims some ranges, and only keys in those ranges move |
| Remove a server | Keys owned by that server move to the next clockwise server |
| Increase virtual nodes | Usually improves load distribution across servers |
| Decrease virtual nodes | Can make ownership uneven |
| Shuffle keys | Changes key hash positions and may change ownership |

## Mental Model

```text
hash(key) -> position on ring -> walk clockwise -> first virtual node -> physical server
```

That is the core idea behind consistent hashing.

## Project Structure

```text
.
├── consistent-hashing-demo.html
└── README.md
```

## Deploying To GitHub Pages

To publish this as a simple GitHub Pages project:

1. Rename `consistent-hashing-demo.html` to `index.html`.
2. Push `index.html` and `README.md` to a GitHub repository.
3. In the repository settings, enable GitHub Pages.
4. Choose the branch and folder where `index.html` lives.

GitHub Pages will serve the demo as a static website.
