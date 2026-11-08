<p align="center">
  <img src="https://sncfoundation.github.io/logos/sheetstor.svg" width="96" alt="Sheetstor logo">
</p>

<h1 align="center">Sheetstor</h1>

<p align="center"><b>Distributed replicated block storage</b><br>
A <a href="https://sncfoundation.github.io">Sheet-Native Computing Foundation</a> project &#183; analog of <b>LINSTOR</b></p>

---

**Status:** 📝 Unsaved Draft &#183; two backends. The registry now lives in the Sheet: **Sheetstor on cells** ships with Sheeternetes ([`Sheetstor.gs`](https://github.com/sncfoundation/sheeternetes/blob/master/Sheetstor.gs), [docs](https://github.com/sncfoundation/sheeternetes/blob/master/docs/script-runtime.md#sheetstor-on-cells)) — a `Volumes` tab plus one hidden tab per volume whose files are base64 chunks below the 50,000-character cell limit, sha256-verified and written copy-on-write with an atomic generation flip, with `rw`/`ro` volumes for script pods (`skctl volume create|ls|put|get|rm|delete`). This CLI ([v0.1.0](https://github.com/sncfoundation/sheetstor/releases/tag/v0.1.0)) remains the host-path backend for Docker hosts, with its registry still in a local JSON file and replicas as metadata only. [Tracking issue #30](https://github.com/sncfoundation/sheeternetes/issues/30) stays open for mounting volumes into Docker pods and replicas across spreadsheets.

## About

Distributed replicated block storage Part of the spreadsheet-native stack — the Sheet stays the source of truth, and it reconciles.

## Get involved

- 📋 Tracking issue &amp; design: [sncfoundation/sheeternetes#30](https://github.com/sncfoundation/sheeternetes/issues/30) (open)
- 🗺️ [SNCF Landscape](https://sncfoundation.github.io/landscape.html)
- 🧩 [All projects](https://sncfoundation.github.io/projects.html)
- ⚖️ [Governance &amp; how to contribute](https://github.com/sncfoundation/governance)
- 🎓 [Get certified (CSFE)](https://sncfoundation.github.io/certification.html)

## Status legend

Everything starts as an **Unsaved Draft**. It reconciles up the tiers from there — see the
[maturity model](https://sncfoundation.github.io/foundation.html#maturity).

---

<sub>Licensed under Apache-2.0. The SNCF does not recommend running production on a spreadsheet. If you do, please film it.</sub>


## Usage

```bash
sheetstor create pgdata 1024 --replicas 2   # provision a 1 GiB volume, replica factor 2
sheetstor list                              # list volumes
sheetstor run pgdata postgres:16            # docker run with the volume mounted at /data
sheetstor path pgdata                       # print the host path (for your own docker -v)
sheetstor delete pgdata                     # forget the volume (keeps data)
sheetstor purge  pgdata                     # delete the volume AND its data (destructive)
```

Registry & data live under `SHEETSTOR_HOME` (default `~/.sheetstor`). Requires `jq` (and `docker` for `run`).

## Model

Each volume is a host-path directory tracked in a **Volumes registry** (`volumes.json`) with its
size and replica factor. On a single Docker host the replicas are metadata; multi-host resync
(DRBD/LINSTOR-style) is on the roadmap.

