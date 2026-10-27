# Agent Discovery, Addressing, and Capabilities

**A comparison of existing standards and the Agent Transfer Protocol (AGTP) approach**

---

## Executive Summary

Three problems sit at the foundation of any system that treats software agents as first-class network participants:

1. **Find an agent by name or ID**
2. **Lookup an agent address** (reachability)
3. **Discover agent capabilities**

These are the core issues because every higher-level interaction depends on them. Before an agent can be delegated to, negotiated with, audited, or composed into a workflow, a caller must be able to:

- Unambiguously identify *which* agent is being addressed,
- Determine *how* to reach it in the current network conditions, and
- Determine *what* it is actually able and authorized to do.

If any one of these three fails, the interaction either cannot start or cannot be governed.

There is no single correct solution to these problems. Different environments impose different constraints:

- Local networks favor zero-configuration announcement (mDNS-style patterns).
- Global public networks favor stable cryptographic identifiers and resolution systems that do not require every participant to own a domain.
- Mobile or home-network agents require NAT traversal.
- Ephemeral or serverless agents lack stable IP addresses and long-lived DNS records.
- High-assurance deployments require verifiable identity and signed capability statements.

Consequently, multiple existing standards already solve pieces of each problem. The design question is not “which single standard wins,” but “which combination of proven mechanisms gives agents stable identity, location agility, and capability advertisement without forcing them to pretend they are ordinary web resources.”

The sections that follow first catalogue the relevant existing standards, then compare that landscape against the positioning taken by the Agent Transfer Protocol (AGTP).

---

## 1. Find an Agent by Name or ID

This is the identity → locator (or pure identity resolution) problem.

| Standard / Mechanism | How it helps | Notes |
|----------------------|--------------|-------|
| **DNS + SRV / NAPTR** (RFC 2782, RFC 3401–3406) | Classic name → service mapping | Works, but forces domain ownership and stability assumptions agents often lack. |
| **mDNS + DNS-SD** (RFC 6762 / 6763) | Local-name resolution without a central server | Excellent for the “Living-Room-TV.local” pattern. Scales poorly beyond a single LAN. |
| **Distributed Hash Tables (Kademlia-style)** | Content-addressable or ID-addressable lookup | Already used at scale (BitTorrent, IPFS, Ethereum DHT). An agent ID can be the DHT key. No domain required. |
| **Handle System** (RFC 3650 series) | Persistent identifier resolution independent of location | Designed for long-lived IDs that outlive any particular host. |
| **URN + DDDS** (RFC 8141 + 3401) | Name that is not a location | “urn:agent:…” style identifiers that resolve through a registry or DHT. |
| **WebFinger** (RFC 7033) | `acct:` or other URI → JSON resource descriptor | Lightweight; already used for actor discovery in ActivityPub. |
| **Host Identity Protocol (HIP)** (RFC 7401) | Cryptographic identity separate from IP address | The HIT (Host Identity Tag) is a stable ID; location can change. |
| **Decentralized Identifiers (DIDs)** (W3C) | Self-sovereign or registry-based IDs with resolution methods | Multiple resolution methods (blockchain, DHT, HTTPS, etc.). |
| **X.509 / JOSE signed documents** | Identity = hash of a signed statement | The identifier itself carries verifiable claims. |

**Creative combination**: Treat the agent ID as a cryptographic hash (or HIT). Resolve it first via DHT or Handle-like system; only fall back to DNS when a domain is actually present.

---

## 2. Lookup an Agent Address

Once an ID is known, the caller still needs the current network location, including NAT traversal and multi-homing.

| Standard / Mechanism | How it helps | Notes |
|----------------------|--------------|-------|
| **DNS A/AAAA + Dynamic Updates** | Classic address binding | Fragile for ephemeral or mobile agents. |
| **STUN / TURN / ICE** (RFC 8489, 8656, 8445) | Discover reflexive addresses and establish connectivity through NAT | Directly applicable to agents behind home/office networks. Battle-tested in WebRTC. |
| **HIP** (again) | Location updates are separate from identity | The agent can move; the HIT stays the same. |
| **LISP** (Locator/ID Separation Protocol, RFC 6830+) | Explicit separation of identity and routing locator | Designed for this class of problem at Internet scale. |
| **Mobile IPv6 / Proxy Mobile IPv6** | Home address + care-of address | Classic mobility solution. |
| **SIP Location Service / REGISTER** | Soft-state address binding | Agents can periodically REGISTER their current contact addresses. |
| **Anycast or Multicast groups** | Reach the “nearest” or “any” instance of an agent type | Useful for capability-oriented rather than instance-oriented discovery. |
| **DHT again** | Store current multi-addresses under the agent ID | Common pattern in libp2p / IPFS. |
| **QUIC Connection IDs + Migration** | Connection can survive address changes | The transport itself becomes location-agile. |

**Creative combination**: Agent ID resolves (via DHT or HIP) to a set of ICE candidates or a LISP mapping. Connectivity is then established with STUN/ICE or native QUIC migration. No stable public IP or domain is required.

---

## 3. Discover Agent Capabilities

This is service / interface discovery — what the agent can actually do.

| Standard / Mechanism | How it helps | Notes |
|----------------------|--------------|-------|
| **DNS-SD TXT records + service types** | Advertise capabilities as key-value pairs | Works locally with mDNS; can be extended to unicast DNS. |
| **CoRE Link Format + Resource Directory** (RFC 6690, RFC 9176) | Designed for constrained devices to publish and query resources/capabilities | Surprisingly good fit for agents. Link relations + attributes. |
| **SLP** (Service Location Protocol, RFC 2608) | Classic service discovery with attribute matching | Largely forgotten but still a clean design. |
| **UPnP / SSDP** | Device and service description | Heavy, but the description model is useful. |
| **OpenAPI / JSON Schema** | Machine-readable interface description | Already widely used; can be returned as a capability document. |
| **Link Relations** (RFC 8288) + profile links | Standardized way to point to capability descriptions | Very lightweight. |
| **RDF / JSON-LD + schema.org or custom ontologies** | Semantic description of capabilities | Allows richer matching (“can book flights under constraint X”). |
| **SDP** (Session Description Protocol) | Capability negotiation for media sessions | Can be generalized beyond media. |
| **CBOR / CDDL** (RFC 8949, RFC 8610) | Compact binary capability descriptors | Useful when agents are constrained or the capability document is large. |
| **Signed capability statements** (JOSE / COSE) | Verifiable claims about what an agent can do | Ties capability discovery directly to the identity system. |

**Creative combination**: An agent publishes a signed capability document (JOSE/COSE) whose hash or ID is discoverable via DHT or Resource Directory. The document itself uses CoRE Link Format or JSON-LD so that both simple attribute matching and semantic queries are possible.

---

## Summary View of Existing Standards

| Problem | Strong existing candidates | Least dependent on domains/DNS |
|---------|----------------------------|--------------------------------|
| Find by name/ID | DHT, HIP, Handle, URN/DDDS, DID, signed hash | DHT, HIP, Handle, signed hash |
| Lookup address | STUN/ICE, HIP, LISP, DHT, SIP REGISTER, QUIC migration | STUN/ICE + DHT or HIP |
| Discover capabilities | CoRE Link Format + RD, DNS-SD, OpenAPI, signed capability docs, RDF | CoRE RD, signed docs, DHT-stored descriptors |

The interesting design space is which **minimal combination** of these already-specified mechanisms gives agents:

- stable cryptographic identity,
- location agility (including behind NAT), and
- capability advertisement

without forcing every agent to own a domain name or maintain a permanent IP address.

---

## AGTP Positioning Compared to the Existing Landscape

The Agent Transfer Protocol (AGTP) treats the standards above as a toolbox and composes a coherent profile around agent-native requirements.

### 1. Find an agent by name or ID — AGTP approach

| Aspect | Existing Standards | AGTP Positioning |
|--------|--------------------|------------------|
| **Primary identity** | DNS names, DIDs, Handle System, HIP HITs, URNs, cryptographic hashes | **Canonical 256-bit Agent-ID** = SHA-256 hash of a signed Agent Genesis document. This is the authoritative identifier. |
| **Human-readable / hierarchical names** | DNS, mDNS `.local`, LDAP, WebFinger | Optional secondary forms (`agtp://domain/agents/name`, etc.) that **resolve to** the canonical Agent-ID. Friendly names are aliases only. |
| **Resolution mechanism** | DNS, DHT, Handle, DID methods, LDAP | Governance-platform registry or transparency-log lookup of the Agent-ID. Direct form `agtp://[Agent-ID]@[host]` also supported as a bootstrap/hint. |
| **Verification** | X.509, JOSE/COSE, PGP, blockchain proofs | JOSE signatures on the Genesis + optional X.509 agent certificate extension. Identity is derivable from the signed document (analogous to a Git commit hash). |

**AGTP stance**: Identity is cryptographic-first and self-describing. A domain is optional convenience, not a requirement. Living on the protocol already indicates that the participant is an agent.

### 2. Lookup an agent address — AGTP approach

| Aspect | Existing Standards | AGTP Positioning |
|--------|--------------------|------------------|
| **Stable address binding** | DNS A/AAAA/SRV, Dynamic DNS, SIP REGISTER | Registry or transparency-log returns current contact information for the Agent-ID. |
| **Direct / peer addressing** | HIP, LISP, Mobile IP | Explicit support for `agtp://[Agent-ID]@[host]` so a known host can be used immediately while full resolution infrastructure is still maturing. |
| **NAT traversal & multi-homing** | STUN / TURN / ICE, QUIC connection migration | Design contemplates STUN/ICE for agents behind home/office NATs and QUIC as preferred transport (port 4480). Local discovery can reuse mDNS-style announcement patterns. |
| **Location agility** | DHT storage of multi-addresses, anycast | Agent-ID remains stable while the underlying network location can change; resolution layer supplies current locators. |

**AGTP stance**: The protocol separates identity from location. It reuses proven NAT-traversal and transport tools (STUN/ICE, QUIC, mDNS patterns) rather than inventing new ones, while keeping the Agent-ID as the stable pivot.

### 3. Discover agent capabilities — AGTP approach

| Aspect | Existing Standards | AGTP Positioning |
|--------|--------------------|------------------|
| **Capability advertisement** | DNS-SD TXT, CoRE Link Format + Resource Directory, SLP, UPnP, OpenAPI, RDF/JSON-LD | **Native protocol methods**: `DESCRIBE` (returns capabilities, supported methods, modalities, versioned features) and `DISCOVER` (locate agents matching criteria). |
| **Machine-readable description** | OpenAPI, JSON Schema, CoRE Link Format, SDP | Agent Identity Document carries the methods list and capability statements. AGTP-API companion specification defines richer endpoint/contract grammar. |
| **Verifiable claims** | Signed capability documents (JOSE/COSE) | Capabilities are part of (or linked from) the signed identity surface, so they inherit the same cryptographic verification path. |

**AGTP stance**: Capability discovery is a first-class protocol operation, not an application-layer add-on. An agent that speaks AGTP can be asked what it can do using the same transport and identity mechanisms used for everything else.

---

## Overall Design Difference

Most of the existing standards listed were designed either for:

- Human-centric or resource-centric naming (DNS),
- Constrained-device local discovery (mDNS, CoRE),
- Media/session connectivity (STUN/ICE), or
- Generic service description (OpenAPI, SLP).

AGTP’s position is that agents need a **coherent combination** of these properties at the transport layer itself:

- Stable cryptographic identity that does not depend on a domain,
- Location agility (including NAT),
- Native capability discovery,
- The same mechanisms working for both local-network agents and globally reachable agents.

It therefore treats the existing standards as a toolbox rather than as a mandate to remain inside the HTTP + DNS model. The protocol reuses the proven pieces (TCP/TLS/QUIC, JOSE, X.509, STUN/ICE patterns, mDNS-style local announcement, registry/log resolution) and binds them around an agent-native identity and method set.

In short: AGTP is not inventing parallel replacements for DNS, STUN, or service discovery. It is selecting and composing the subset of existing mechanisms that let agents be addressed, reached, and understood without forcing them to pretend they are ordinary web resources.

---

## References & Further Reading

- AGTP core draft family: [datatracker.ietf.org/doc/draft-hood-independent-agtp/](https://datatracker.ietf.org/doc/draft-hood-independent-agtp/)
- Companion drafts (discovery, identifiers, certificates, composition, etc.) are linked from the core draft and the [nomoticai/agent-transfer-protocol](https://github.com/nomoticai/agent-transfer-protocol) repository.
- Relevant RFCs appear in the tables above.

---

*Document prepared for discussion of agent-native discovery, addressing, and capability advertisement. Suitable for sharing in IETF or open-source contexts.*
